---
layout: post
title: Adding an automated Google PageSpeed Insights check to continuous integration with CircleCI and Heroku
---


Building [GetCalFresh](https://www.getcalfresh.org/about), we think a lot about web performance (how fast pages load on different devices). Because our user base is low-income Californians, many of whom are using low-end Android devices.

We want to make sure using GetCalFresh to apply for food assistance is the best digital experience our users have had accessing social services: respectful, easy to use, and certainly *not* frustrating.

One tool in our belt for measuring GCF's web performance is [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/), a free tool from Google that analyzes a web site and checks it for common performance issues, generating both a score and a list of suggested improvements:


Pic: /Users/daveguarino/Pictures/Screenshots/google-psi-screenshot.png 

We have a 71 — that's okay, not great. We should improve that. Then again, we're trying to enroll 2.5 million more Californians in the SNAP program, which — turns out — is a lot of work. So mayyyybe it's not the top priority to do a bunch of work to bump our PSI score.

BUT a useful thing we _can_ do now [1] is making sure we don't accidentally make our PSI go _down_ with other changes we're making.

If you read this and in your head said SOUNDS LIKE A TASK FOR CONTINUOUS INTEGRATION, we're of like mind.

On the GetCalFresh team we're big fans of quick feedback loops via automation. We use test-driven development (TDD) to write almost all our code, yielding about 90% test coverage. [CircleCI](https://circleci.com/) runs this big test suite on all commits to GitHub, and also automates some parts of our deployment pipeline. We also use [Hakiri](https://hakiri.io/) to run an automated security audit on every commit.

So I thought, hrm, this is clearly something we could automate, no?

### Approach

Here's the basic approach I took to doing this:

- Set up a standalone deploy just for a PSI check
- On every commit, have CI:
	- Push to that deploy
	- Ask Google to run a PSI check and compare it to the PSI on master (using a deploy running master)
- Have CI fail if that commit made our PSI go down (generating alerts for us that there's a problem)

I used Heroku for the deployment and Circle for the CI, but folks should be able to translate this approach to their own infrastructure.

### Step 1: Setting up a standalone deploy for our PSI check on Heroku

First I set up a new app on Heroku just for this, which is quite easy, especially given GetCalFresh is a Rails app:

```bash
heroku create -a gcf-pagespeedinsight

# Set some env vars to make sure the deploy is clearly not production
# (demo thing is specific to our app)
heroku config:set -a gcf-pagespeedinsight RAILS_ENV=staging
heroku config:set DEMO_MODE=true -a gcf-pagespeedinsight

# Add a git remote and push our code
git remote add pagespeedinsight https://git.heroku.com/gcf-pagespeedinsight.git
git push pagespeedinsight master
```

Our production infrastructure runs on AWS' GovCloud region, but for setting up instances for other uses like this one, the ease and simplicity of Heroku can't be beat.

### Step 2: Configuring CircleCI to push to that deploy

We've already configured CircleCI with credentials to automatically push to our demo instance (also hosted on Heroku) so it's pretty easy to add in this deployment. Doing so makes our circle.yml file look like this:

```yaml
# circle.yml

test:
  pre:
    - git push git@heroku.com:gcf-pagespeedinsight.git $CIRCLE_SHA1:master
  override:
    - RAILS_ENV=test bundle exec rspec -r rspec_junit_formatter --format RspecJunitFormatter -o $CIRCLE_TEST_REPORTS/rspec/junit.xml

deployment:
  demo:
    branch: master
    commands:
      - git push git@heroku.com:gcfdemo.git $CIRCLE_SHA1:master
```

The `git push ...` on line 3 in the test -> pre section will deploy to our Heroku instance for PSI checks before running the test suite.

### Step 3: Writing some code to run a PSI check and compare the two instances

Next we'll need some actual code to run the check and yell loudly if our PSI fell.

Google PageSpeed Insights has an API just for this. Since it's just a plain old request/response with the score there's plenty of ways to do this. Since our app is Ruby on Rails, I just made a quick, procedural rake task to illustrate this:

```ruby
# lib/tasks/checkpsi.rake

desc "check psi"
task :checkpsi do
  url_for_staging_server_with_current_commit = "https://gcf-pagespeedinsight.herokuapp.com"
  # CI already auto-deploys master to our demo site, so we use it as our comparison
  # You could also compare against staging or (if bold) production
  url_for_server_with_current_master = "https://demo.getcalfresh.org"

  # Hit the API to run a (desktop) speed check, parse the JSON, and yank out our speed
  psi_score_for_this_commit = JSON.parse(HTTParty.get("https://www.googleapis.com/pagespeedonline/v2/runPagespeed?url=#{url_for_staging_server_with_current_commit}&strategy=desktop").body)["ruleGroups"]["SPEED"]["score"]

  # Google lets you use the PSI API once every 5 seconds without authenticating
  puts "sleeping for 6 seconds to let Google REST (get it?)"
  sleep 6

  # Do the same API call but for our demo instance, aka our current master branch
  psi_score_for_master = JSON.parse(HTTParty.get("https://www.googleapis.com/pagespeedonline/v2/runPagespeed?url=#{url_for_server_with_current_master}&strategy=desktop").body)["ruleGroups"]["SPEED"]["score"]

  puts "PSI score for master: #{psi_score_for_master}"
  puts "PSI score for this commit: #{psi_score_for_this_commit}"

  # Fail CI by raising if our PSI went down
  if psi_score_for_this_commit < psi_score_for_master
    raise "Oh noes! PageSpeed Insight score fell from #{psi_score_for_master} to #{psi_score_for_this_commit} with this commit! Please go to https://developers.google.com/speed/pagespeed/insights/?url=#{url_for_staging_server_with_current_commit} to review the problems and fix them"
  end
end
```

I've tried to be really explicit and procedural here so it's clear to anyone using any language. There's also a CLI utility called [psi](https://github.com/addyosmani/psi) that you could use.

You could also modify this to do a bunch more with what the API returns (it has a lot more detail), for example:
- Run a mobile check too (set `strategy=mobile`) in the URL
- Display the different recommendations the API returns
- Use a GitHub webhook to post content back to a pull request or commit

### Step 4: Adding the check into our CI pipeline

Lastly we've got to add this check into our CircleCI config. That'll make our `circle.yml` look like this:

```yaml
# circle.yml

test:
  pre:
    - git push git@heroku.com:gcf-pagespeedinsight.git $CIRCLE_SHA1:master
    - curl https://gcf-pagespeedinsight.herokuapp.com/
    - bundle exec bin/rake checkpsi
  override:
    - RAILS_ENV=test bundle exec rspec -r rspec_junit_formatter --format RspecJunitFormatter -o $CIRCLE_TEST_REPORTS/rspec/junit.xml

deployment:
  demo:
    branch: master
    commands:
      - git push git@heroku.com:gcfdemo.git $CIRCLE_SHA1:master
```

You'll note my `curl` in there — I found that it was helpful to send a request to the Heroku app right after deploying to trigger the first caching and all that jazz and provide a true comparison to our demo deploy, but I didn't dive too much into it.

### SHOW ME WHAT YOU GOT

Okay! Now we're ready to make our app worse intentionally and see what happens! I added a commit that will sleep for 5 seconds before responding to a request to the front page. THAT OUGHTTA DO IT.

<img width="1144" alt="screen shot 2017-01-28 at 3 34 32 pm" src="https://cloud.githubusercontent.com/assets/994938/22408830/d25ff3d6-e634-11e6-921c-041f96ba3cd4.png">

Yeeeep — CI is _not_ stoked. And on GitHub we'll see that CI failed and go yell at whoever did that thing.

(Aside: This is a joke, un chiste, una broma. Having [a psychologically safe, just culture](https://codeascraft.com/2012/05/22/blameless-postmortems/) where we understand the context of someone's mistake that gave them the false confidence in their approach — and then you make a change in that context, like automating test running! — is how you actually fix problems like this.)

### That's it! Go home!

So yeah, that's it! It was fun exploring this, and I was actually pretty surprised that no one had written this up (though many of the pieces are out there, nothing put together) so figured I'd throw it into the world and hope it's helpful for others.

The big improvement I'd make here given more time is actually at the _internal_ user experience level: having CI fail when the score falls is okay, but a more actionable approach towards automated feedback here would be having a GitHub webhook that commented on a pull request with a PSI score drop and diff'd the problems returned in the API so show _just_ the thing that changed that made it fall. Then whoever's working on that knows immediately what they changed.

Also, Google PageSpeed Insights is not The One True Metric™! So failing CI is maybe a bit too aggressive, since there could certainly be contexts where a falling PSI score is a fair tradeoff.

But the bottom line is if you automate your feedback loops, you are more effective at responding to them. Hope this helps some folks do that.

Oh, and if this kind of development philosophy appeals to you, [we are hiring](http://www.codeforamerica.org/jobs) engineers and other roles for both the GetCalFresh project and other excellent ones at Code for America.

[1] ...especially given that I'm writing this on a weekend and this is more fun exploration than an exercise in strict prioritization...
