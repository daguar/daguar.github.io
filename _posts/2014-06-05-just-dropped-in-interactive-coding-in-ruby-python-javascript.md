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

```python
from IPython import embed
import json

geodata_json_string = open('good_oakland_bars.geojson').read()
geodata_dict = json.loads(geodata_json_string)

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


_If you liked this or have additional thoughts, I'd love to hear from you at [@allafarce](https://twitter.com/allafarce) or by [e-mail](mailto:dave@codeforamerica.org)._
