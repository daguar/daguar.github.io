---
layout: post
title: "Just Dropped In: Using interactive breakpoints in Ruby, Python and Javascript to make life better"
---

<div class="message">
    I pushed my soul in a deep dark hole and then I followed it in<br>
    I watched myself crawlin' out as I was a-crawlin' in<br>
    I got up so tight I couldn't unwind<br>
    I saw so much I broke my mind<br>
  <a href="https://www.youtube.com/watch?v=od_6M8cFdUA">
    I just dropped in to see what condition my condition was in<br>
  </a>
  <br>
  - Kenny Rogers
</div>

Sometimes, when you're early in programming in a new language, **darkness washes over you** — you've written 70% of the code to do some thing, but then you don't know the syntax to do that next step.

In these cases, it's great to use a **breakpoint** — a line of code that stops your code there, and lets to interactively type lines with the full context of the code above, but with immediate feedback (this interactive prompt is called a ["REPL"](http://en.wikipedia.org/wiki/Read–eval–print_loop)).

You've probably used a REPL before — it's what happens when you go to a terminal and type `irb` (Ruby), `python`, or `node` without any arguments. In a REPL, you type code, and immediate see what that code returns.

By adding **breakpoints**, we can enter that almost zen-like immediate feedback loop at any point we want in our code.

Because I love using interactive breakpoints to debug, and often find myself working across languages, here's a quick guide on using them across Python, Ruby, and Javascript (Node).

### Python: IPython embed

In Python, we can use a breakpoint with IPython's embed function.

First, make sure you have IPython installed:

`pip install ipython`

Then we can add a breakpoint by importing embed, and using the embed() function:

```python
from IPython import embed
# Misc code
embed()
```

### Ruby: Pry

In Ruby, we can add a breakpoint with the nifty pry gem.

First, make sure you have pry installed:

`gem install pry`

Now, require pry in your code, and drop in using `binding.pry`:

```ruby
require 'pry'
# Misc code
binding.pry
```

### Node.js: Debugger

In Node.js, we can add a breakpoint with the `debug` feature of Node.js and adding a `debugger` line.

The process is slightly trickier than for Ruby or Python, buuut we don't have to install anything this time!

Let's walk through how to use debugger. In our Javascript, we add `debugger` to create a breakpoint. So say we have simple file called `hello.js` with the following:

```javascript
var x = 'hello';
debugger;
```

We can go to that breakpoint by first running the file with `debug` added in, i.e. `node debug hello.js`:

```bash
$~: node debug hello.js
< debugger listening on port 5858
connecting... ok
break in hello.js:1
  1 var x = 'hello';
  2 debugger;
  3
debug >
```

Now, at the debug prompt, we do two things:

1. Type `c` and hit enter
2. Type `repl` and hit enter

```bash
debug> c
break in hello.js:2
  1 var x = 'hello';
  2 debugger;
  3
  4 });
debug> repl
Press Ctrl + C to leave debug repl
>
```

Congrats! Now you're at an interactive prompt at the breakpoint.

### An example: learning how to deal with GeoJSON in Python

Let's say you're exploring the wild and crazy world of open geo, and you're super stoked to play around with GeoJSON — a JSON data format for storing geographic data — and you want to do it in Python.

You're given a GeoJSON file with a list of bars in Oakland where you might get to hear the Descendents, and you want write a Python script to print out the name of each.

Since you're new to this whole geo game, you don't really know how GeoJSON nests its attributes, and want to be able to play with the JSON data as a Python dictionary to figure that out.

So you've written a script that loads the GeoJSON and turns it into a dictionary. (You can get this script and data from GitHub with `git clone https://gist.github.com/9a6fc83d1a67939c5110.git`)

```python
from IPython import embed
import json

geodata_json_string = open('good_oakland_bars.geojson').read()
geodata_dict = json.loads(geodata_json_string)

embed()
```

Now, when we run `python py_interactive_breakpoint.py`, we'll be dropped to an interactive breakpoint to play around with the `geodata_dict` variable.

By playing around in the REPL — most simply by typing `geodata_dict` and seeing what it looks like — we figure out that `geodata_dict['features']` is an array of the bars, and  that GeoJSON stores the non-geographic attributes in a `properties` dictionary.

So we realize can access the name of the first bar by doing `geodata_dict['features'][0]['properties']['name']` and this opens the door to a nifty `for` loop to print out those names.

We can even write try writing that for loop in the REPL first, and if it works, we can then migrate it over to our file.

Ta-da! REPL-driven-development, yo.

### Parting thought

When coding, always remember the words of The Stranger — sometimes you eat the bar, and sometimes the bar, well, it eats you.

So when you feel like the bar's eating you, try using a breakpoint.

And if that doesn't help, you can always say "fuck it" and write a Lebowski-themed blog post.

_If you found this helpful, spotted a problem, or have additional thoughts, I'd love to hear from you at [@allafarce](https://twitter.com/allafarce) or by [e-mail](mailto:dave@codeforamerica.org)._
