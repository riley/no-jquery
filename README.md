# No more jQuery

this was originally used for a talk at my job, but I'm keeping it here as a brain dump and personal reference. It's all the resources I've gathered in my quest to write (mostly) vanilla javascript for work. Props to [You don't need jQuery](http://blog.garstasio.com/you-dont-need-jquery/) and many, many stackoverflow posts and of course [MDN](https://developer.mozilla.org/en-US/docs/Web/Reference/API)

### Why?

It's easy to lean on jQuery at the Times since NYT5 includes it in every page. This isn't going to be the case for every site on which you work.

jQuery patches over some ugly bits which largely aren't an issue anymore now that we've moved on from supporting IE8 in the industry. With the updates to js in IE9, you don't really need underscore.js either.

An incredible amount of work goes into jQuery to make it as light as possible, but it still carries around a noticible amount of weight. It's another request to the server, and takes its own time to parse. Just because you've built all those libraries into a single file and uglified them doesn't mean you're out of the woods. The browser still has to parse the actual javascript, which can easily run into the [hundreds of milliseconds](http://timkadlec.com/2014/09/js-parse-and-execution-time/), which affects how fast your site seems.

if you're doing dom manipulations on lots of objects (i.e. data visualization), you'd be better off sticking to native methods over jQuery because of the [performance cost](http://jsperf.com/getelementbyid-vs-jquery-id/72).

### I'm so lazy. $ and _ are so easy to type and getElementsByClassName() makes me remember that time I tried to learn Java.

Boo. You're a programmer, you can type fast. Besides, you can alias the pertinent functions if you're really lazy, or you're worried about file size.

### It keeps programming on the front end fresh.

It's always fun to learn, and you'll think of crazier ways to do things if you know what to look for -- you won't have to dig for some jQuery plugin if you can quickly write something yourself. You might actually _save_ time since you don't have to find a plugin if you know how to produce the functionality quickly in vanilla js.

### What we're going to cover

There's a lot, so we'll try to cover the common usages of jQuery, and get into SVG functions (stare into the abyss) if we have time at the end. Unless otherwise noted, examples work in IE9+.

* Easy wins
* Element Selection
* DOM Manipulation
* AJAX
* Shims
* Utilities
* Gotchas
* SVGElement

The examples will be formatted like this:

**jQuery**
```javascript
$(document).ready(function () {
    // code
});
```

**DOM API**
```javascript
document.addEventListener('DOMContentLoaded', function () {
    // srs bzns
});
```

### How to use what you've learned:

**jQuery**
```javascript
<script src="//code.jquery.com/jquery-migrate-1.2.1.min.js"></script>
```

**vanilla**
```javascript
<!-- lol -->
```