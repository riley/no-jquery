## Utils

There are plenty of convenience functions in jQuery and underscore cover stuff that's was/is missing from vanilla js, and we just stopped thinking about it years ago. Here are a few examples of replicating common tasks in vanilla js.

#### Aliases for long function names

Admittedly, typing document.querySelectorAll is not only arduous, but makes all your code bigger, and ultimately harder to read. Here's my setup:

```javascript
window.$ = document.querySelector.bind(document);
window.$all = document.querySelectorAll.bind(document);
window.byId = document.getElementById.bind(document);
window.byClass = document.getElementsByClassName.bind(document);
```

I think everyone knows this, but in case you've forgotten -- Backbone has a nice alias of `$('selector').find('subselector')` which is `this.$('subselector')`. The aliases above search the _enitre_ DOM. Don't forget that each element can search beneath itself so `myFragment.querySelectorAll('.child-element')` will be much faster than querying the document every time.

#### Type checking

JavaScript is dynamically typed. I'm going to punt on arguing the worth of frameworks and compiled js supersets for now. Here's how to check if a variable holds the different native js types.

**jQuery & _**

```javascript
$.isFunction(foo);
$.isPlainObject(bar); // false for []
$.isArray(baz);

_.isFunction(foo);
_.isObject(bar); // true for []
_.isArray(baz);
```

**vanilla**

determining a function makes sense:
```javascript
typeof foo === 'function'
```

determining an Object is where most people get annoyed and just go back to jQuery. `null` and `[]` are Objects. duh.

```javascript
bar != null && Object.prototype.toString.call(bar) === '[object Object]'
```
still a one liner, and could easily be a utility function. You shouldn't load underscore or jQuery just for type checking

Checking for an `Array` isn't too hard either.

```javascript
Array.isArray(baz);
```

#### is an element in the viewport?

**jQuery**

haha jerk! you have to find a plugin.

**vanilla**

```javascript
var isInViewport = function ( elem ) {
    var distance = elem.getBoundingClientRect();
    return (
        distance.top >= 0 &&
        distance.left >= 0 &&
        distance.bottom <= (window.innerHeight || document.documentElement.clientHeight) &&
        distance.right <= (window.innerWidth || document.documentElement.clientWidth)
    );
};

var elem = document.querySelector('#some-element');
isInViewport(elem); // returns a Boolean
```

#### height of the document

**jQuery**

`$(document).height();` works, with the caveat that if the document is *taller* than the viewport. Otherwise it just returns the height of the viewport.

**vanilla**

```javascript
var getDocumentHeight = function () {
    return Math.max(
        document.body.scrollHeight,
        document.documentElement.scrollHeight,
        document.body.offsetHeight,
        document.documentElement.offsetHeight,
        document.body.clientHeight,
        document.documentElement.clientHeight
    );
};
```

#### Mixing objects together

These are the utility functions that are nice to haves, and should arguably be in the language a couple of versions ago, but I digress...

Say you have two objects

```javascript
var one = {
    a: 'alpha',
    b: {
        b1: 'beta'
    }
};
var two = {
    b: {
        b2: 'bravo'
    },
    c: 'charlie'
};
```

**jQuery & _**

```javascript
// first argument is the 'deep copy' flag
// mutates one with the contents of two
$.extend(true, one, two);

// copies properties from one and two onto a new object
var thirdParty = $.extend(true, {}, one, two);

// underscore
var thirdParty = _.extend({}, one, two);

```

Here's an fairly simple extend function:

**vanilla**
```javascript
// always deep copy, because why not?
var extend = function ( objects ) {
    var extended = {};
    var merge = function (obj) {
        for (var prop in obj) {
            if (Object.prototype.hasOwnProperty.call(obj, prop)) {
                if ( Object.prototype.toString.call(obj[prop]) === '[object Object]' ) {
                    extended[prop] = extend(extended[prop], obj[prop]);
                }
                else {
                    extended[prop] = obj[prop];
                }
            }
        }
    };
    merge(arguments[0]);
    for (var i = 1; i < arguments.length; i++) {
        var obj = arguments[i];
        merge(obj);
    }
    return extended;
};
```

#### Iterating over objects

This is one of those tasks that's surprisingly tricky (read: annoying) and makes you learn more than you cared to about how JavaScript works deeper down.

Given an object:
```javascript
var comedyRelief = {
    first: 'Henry',
    last: 'Jones'
};
```
and then we create another object which inherits the properties of `protagonist`
```javascript
var protagonist = Object.create(comedyRelief);
protagonist.suffix = 'Jr';
protagonist.nickname = 'Indiana';
```

`protagonist` has 3 properties, only one of which belong to itself. Almost all of the time, if you iterate over the properties you want the _only_ the properties that belong to itself.

**jQuery & _**
```javascript
$.each(protagonist, function (key, value) {
    // drive plot
});

// or underscore
_.each(protagonist, function (value, key) {
    // notice arguments are reversed.
});
```
well, that was nice. jQuery assumes we don't want to go up the prototype chain.

**vanilla**
```javascript
for (var key in protagonist) {
    // drive plot
    if (protagonist.hasOwnProperty(key)) {
        // exposition
    }
}
```
we have to make sure we only check the `protagonist` for its own properties with `hasOwnProperties`, another long JavaScript function name. No one accused js of being terse.

```javascript
// Ah ha! you were expecting the code above! Didn't see this one coming!
Object.keys(protagonist).forEach(function (key) {
    // drive plot
});
```

#### Iterating over arrays
Say we have an array:
```javascript
var infinityGems = ['space', 'mind', 'soul', 'reality', 'time', 'power'];
```

and you were way ahead of the curve and you liked to chain functions together in jQuery

**jQuery & _**
```javascript
$.each(infinityGems, function (index, value) {
    // control some aspect of the universe
});

// reversed arguments here too
_.each(infinityGems, function (value, index) {
    // expose your tragic flaw
});
```

You could write a for loop, but it adds mental overhead to you and future you. Do this easily with vanilla js. I would argue that this is **better** than `_` since you dont have to use the silly `_.chain` setup. It Just Works™

```javascript
infinityGems.forEach(function (val, index) {
    // better call Adam Warlock (again)
});
```

It's worth noting that almost all of the array functions that underscore.js popularized are in IE9 natively. This is a big deal. I can't tell you how great it was to finally unshackle myself from IE8 and keeping track of all the things I *couldn't* do. We can complain about the lack of support for all sorts of CSS3 things another day, but IE9 is solid on ES5 (strict mode the obvious exception).

#### Finding single & multiple objects in an array

Given:
```javascript
var evilExes = ['Matthew Patel', 'Lucas Lee', 'Todd Ingram', 'Roxy Richter', 'Kyle Katayanagi', 'Ken Katayanagi', 'Gideon Graves'];
```

**jQuery**

Back in the day Internet Explorer didn't support `indexOf` on arrays, but confusingly, it did on strings. This was an incredible oversight, akin to God not giving us a third arm or x-ray vision. Therefore:

```javascript
// where -1 means the item is not in the array
var finalBossIndex = $.inArray('Gideon Graves', evilExes);
```

or if we want to find all the objects in the array that satisfy a condition:
```javascript
var matchingElements = $.grep(evilExes, function (value) {
    return someCondition === true;
});
```

**vanilla**
IE9+ rejoice!
```javascript
var evilExes.indexOf('Gideon Graves');
```

and awesomely
```javascript
evilExes.filter(function (element) {
   return someCondition === true;
});
```

this is better than `$.grep` in my opinion since it's part of the array already

#### Min and Max values in an array

Given an array `incomes` find the minimum and maximum values

**jQuery & _**

stop trying to do math things with jQuery and underscore. A plague on both your houses!

**vanilla**
```javascript
// boom goes the dynamite
Math.min.apply(Math, incomes);
Math.max.apply(Math, incomes);
```

#### Modifying function context

This is another big one for me. Once you finally learned what `this` does in JavaScript, then you find out it was a mess to control.

Say you have a widget that keeps track of its own `el` (or `$el` for a jQuery widget). The widget has a method `buyNow` which takes users' money.

**jQuery & _**
```javacript
this.$el.find('.purchase').on('click', $.proxy(this.buyNow));
```
**vanilla**
```javascript
// finally. Function.prototype.bind is in IE9+
this.el.querySelector('.purchase').addEventListener('click', this.buyNow.bind(this));
```

#### Trimming strings

For a long time I assumed that nothing would work in ie8 and 9, so I automatically used jQuery for everything. Luckily, this isn't true. I thought the same thing about trimming strings:

**jQuery**

```javascript
var firstName = $.trim($('input[name="first-name"]').val());
```

**vanilla**

the `trim` method now exists on `String.prototype`
```javascript
var firstName = document.querySelector('input[name="first-name"]').value.trim();
```