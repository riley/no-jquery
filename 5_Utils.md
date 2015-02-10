## Utils

There are plenty of convenience functions in jQuery and underscore cover stuff that's was/is missing from vanilla js, and we just stopped thinking about it years ago. Here are a few examples of replicating common tasks in vanilla js.

#### Type checking

Flamewars aside, js is dynamically typed. Without arguing the worth of frameworks and compiled js supersets, here's how to check if a variable holds the different native js types.

**jQuery**

```javascript
$.isFunction(foo);
$.isPlainObject(bar);
$.isArray(baz);
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

jQuery

```javascript
// first argument is the 'deep copy' flag
// mutates one with the contents of two
$.extend(true, one, two);

// copies properties from one and two onto a new object
var thirdParty = $.extend(true, {}, one, two);
```

Here's an fairly simple extend function:

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