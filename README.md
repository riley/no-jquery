# JavaScript Thursdays Talk
# No more jQuery

notes for Riley's talk on 2.14.15

### Why?

It's easy to lean on jQuery here since the NYT5 includes it in every page. This isn't going to be the case for every site on which you work. jQuery patches over some ugly bits that aren't really an issue anymore now that we've largely moved on from supporting IE8 in the industry. With the updates to js in IE9, you don't really need underscore.js either.

### I'm so lazy. $ and _ are so easy to type.

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

## Considerations / Gotchas
## SVGElement
