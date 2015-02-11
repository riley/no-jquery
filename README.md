# JavaScript Thursdays Talk
# No more jQuery

notes for Riley's talk on 2.14.15

### Why?

It's easy to lean on jQuery here since the NYT5 includes it in every page. This isn't going to be the case for every site on which you work. jQuery patches over some ugly bits that aren't really an issue anymore now that we've largely moved on from supporting IE8 in the industry. With the updates to js in IE9, you don't really need underscore.js either.

jQuery carries around a (optimized) noticible amount of weight. It's another request to the server, and takes it's own time to parse. Just because you've built all those libraries into a single file and uglified them doesn't mean you're out of the woods. The browser still has to parse the actual javascript, which can easily run into the [hundreds of milliseconds](http://timkadlec.com/2014/09/js-parse-and-execution-time/), which affects how fast your site seems.

if you're doing dom manipulations on lots of objects (i.e. data visualization), you'd be better off sticking to native methods over jQuery because of the [performance cost](http://jsperf.com/getelementbyid-vs-jquery-id/72).

You could solve this with tricks like dynamic script loading. While this has its place if you're building Facebook (which you aren't), you should try to simplify instead.

### I'm so lazy. $ and _ are so easy to type and getElementsByClassName makes me remember that time I tried to learn Java.

Boo. You're a programmer, you can type fast. Besides, you can alias the pertinent functions if you're really that lazy, or you're worried about file size.

### It keeps programming on the front end fresh.

It's always fun to learn, and you'll think of crazier ways to do things if you know what to look for -- you won't have to dig for some jQuery plugin if you can easily write this stuff yourself.

### What we're going to cover

There's a lot, so we'll try to cover the common usages of jQuery, and get into SVG functions (stare into the abyss) if we have time at the end. Unless otherwise noted, examples work in IE9+.

* Element Selection
* DOM Manipulation
* AJAX
* Shims
* Utilities
* Gotchas
* SVGElement