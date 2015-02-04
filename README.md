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
* Eventing
* Shims
* Considerations / Gotchas
* SVGElement

## Element Selection

#### By id
jQuery
`$('#some-element')`
DOM API `document.getElementById('some-element')`
or `document.querySelector('#some-element')`

#### By class
jQuery `$('.some-element')`

DOM API `document.getElementsByClassName('some-element')`
or `document.querySelectorAll('.some-element')`

#### By tag name
jQuery `$('div')`

DOM API `document.getElementsByTagName('div')` or  `document.querySelectorAll('div')`

#### By pseudo-class
jQuery `$('#some-widget :first-child')` or

DOM API `document.querySelectorAll('#some-widget :first-child')`

#### Children
jQuery `$('#parent-el').children()`

DOM API
```javascript
// this will select text nodes, which is almost never what you want
document.getElementById('parent-el').childNodes
```
or
```javascript
// returns elements in a way that makes sense without all the random blank text nodes
document.getElementById('parent-el').children
```

#### Decendants
jQuery `$('#parent a')` or

DOM API `document.querySelectorAll('#parent a')`

#### Excluding from a set
jQuery
```javascript
$('button').not('.disabled')
$('button:not(.disabled)')
```
DOM API
```javascript
// what!
document.querySelectorAll('button:not(.disabled)')
```
#### Multiple selectors
jQuery `$('div, a, li')`

DOM API `document.querySelectorAll('div, a, li')`

#### Things to keep in mind
* `getElementsByTagName()` and `querySelectorAll()` return an HTMLCollection and a NodeList respectively, which are not the same thing
* something else probably

## DOM Manipulation

jQuery isn't doing anything magical or inventing functionality (for the most part). We can do almost everything with the vanilla DOM API.

#### New Elements
jQuery `$('<div></div>')`

DOM API `document.createElement('div')`

#### Inserting Elements Before & After
let's say we start with a list, and we want to add an element in the middle
```javascript
<div id="beta"></div>
<div id="launch"></div>
<div id="patch"></div>
```
and we want to insert some content between the first and second divs, giving this:
```javascript
<div id="beta"></div>
<div id="rewrite"></div>
<div id="launch"></div>
<div id="patch"></div>
```
jQuery
```javascript
$('#beta').after('<div id="rewrite"></div>');
```
DOM API
```javascript
// ooh.
document.getElementById('beta').insertAdjacentHTML('afterend', '<div id="rewrite"></div>');
```

or maybe we want to insert something at the beginning, before all the selected elements, creating this:
```javascript
<div id="alpha"></div>
<div id="beta"></div>
<div id="launch"></div>
<div id="patch"></div>
```
jQuery
```javascript
$('#beta').before('<div id="alpha"></div>');
```
DOM API
```javascript
// does this look familiar?
document.getElementById('beta').insertAdjacentHTML('beforestart', '<div id="alpha"></div>');
```
#### Inserting elements as children
Given:
```javascript
<div id="diana">
    <div id="william"></div>
</div>
```
and we want to create a new element and make it the first child of the parent
```javascript
<div id="diana">
    <div id="harry"></div>
    <div id="william"></div>
</div>
```
jQuery
```javascript
$('#diana').prepend('<div id="harry"></div>');
```

DOM API
```javascript
document.getElementById('diana').insertAdjacentHTML('afterbegin', '<div id="harry"></div>');
```
or create the element and put it at the end:
```javascript
<div id="diana">
    <div id="william"></div>
    <div id="harry"></div>
</div>
```
jQuery
```javascript
$('#diana').append('<div id="harry"></div>');
```
DOM API
```javascript
document.getElementById('diana').insertAdjacentHTML('beforeend', '<div id="harry"></div>');
```
#### Moving elements
Given:
```javascript
<div id="donald">
    <div id="huey"></div>
    <div id="louie"></div>
    <div id="dewey"></div>
</div>
<div id="tom-riddle"></div>
```
and we just want to adopt this child as the last child like so
```javascript
<div id="donald">
    <div id="huey"></div>
    <div id="louie"></div>
    <div id="dewey"></div>
    <div id="tom-riddle"></div>
</div>
```
jQuery
```javascript
$('#donald').append($('#tom-riddle'));
```
DOM API
```javascript
document.getElementById('donald').appendChild(document.getElementById('tom-riddle'));
```
Or we could make Tom the first child with
jQuery
```javascript
$('#donald').prepend($('#tom-riddle'));
```
DOM API
```javascript
document.getElementById('donald').insertBefore(document.getElementById('tom-riddle'), document.getElementById('huey'));
```
this is a one liner, but it's getting kind of long. We will solve this problem later. Stay vigilant!
#### Removing Elements
jQuery `$('#booger').remove();`
DOM API `document.getElementById('booger').parentNode.removeChild(document.getElementById('booger'))`
#### Adding and Removing classes
jQuery `$('#freshman').addClass('econ');`
DOM API `document.getElementById('freshman').classList.add('econ')`
or to remove a class:
jQuery `$('#senior').removeClass('apathy');`
this does not work in IE9. You'll have to have a shim, which is available on MDN. More on that later.
DOM API `document.getElementById('senior').classList.remove('apathy')`
#### Adding, Removing and updating attributes
