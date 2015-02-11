## Utils

There are plenty of convenience functions in jQuery and underscore cover stuff that's was/is missing from vanilla js, and we just stopped thinking about it years ago. Here are a few examples of replicating common tasks in vanilla js.

#### Type checking

Flamewars aside, js is dynamically typed. Without arguing the worth of frameworks and compiled js supersets, here's how to check if a variable holds the different native js types.

**jQuery & _**

```javascript
$.isFunction(foo);
$.isPlainObject(bar);
$.isArray(baz);

_.isFunction(foo);
_.isObject(bar);
_.isArray(baz);
```

**vanilla**

determining a function makes sense:
```javascript
typeof foo === 'function'
```

determining an Object is where most people get annoyed and just go back to jQuery. `null` and `[]` are all Objects.

```javascript
bar != null && Object.prototype.toString.call(bar) === '[object Object]'
```
still a one liner, and could easily be a utility function. You shouldn't load underscore or jQuery just for type checking

Checking for an `Array` isn't too hard either.

```javascript
Array.isArray(baz);
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
we have to make sure we only check the `protagonist` for it's own properties with `hasOwnProperties`, another long JavaScript function name. No one accused js of being terse.

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

#### Getting single & multiple objects in an array

Given:
```javascript
var
```