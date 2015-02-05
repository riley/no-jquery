
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
jQuery `$('#purchase').attr('disabled', 'disabled');`
DOM API `document.getElementById('purchase').setAttribute('disabled', 'disabled');`
this code will add and update attributes
#### Removing attributes
jQuery `$('#purchase').removeAttr('disabled');`
DOM API `document.getElementById('purchase').removeAttribute('disabled');`
#### Setting text content
Say that we have an existing node that has some text:
```javascript
<div id="username">Napoleon Dynamite</div>
```
we can update it like so:
jQuery `$('#username').text('Stephen Hawking');`
DOM API `document.getElementById('username').textContent = 'Stephen Hawking';`
as in `$.fn.text()`, any html is escaped by using `textContent`

#### Adding and updating Element Styles
Please, as a rule, don't set styles directly onto DOM elements. It makes things incredibly hard to track down. I will find you, and hurt you. But, you can do if you're setting dynamic positioning like custom x,y coordinates for example.

We have a span or something where we've computed the position (around a circle or something).
```javascript
<span id="bubble">Foo</span>
```
we want to move it to some custom variables x, and y

jQuery
```javascript
$('#bubble').css({
    '-webkit-transform': 'translate(' + x + 'px, ' + y + 'px)',
    '-moz-transform': 'translate(' + x + 'px, ' + y + 'px)',
    '-ms-transform': 'translate(' + x + 'px, ' + y + 'px)',
    '-o-transform': 'translate(' + x + 'px, ' + y + 'px)',
    'transform': 'translate(' + x + 'px, ' + y + 'px)',
})
```
with the DOM API, it might make more sense to do this:
```javascript
var t = 'translate(' + x + 'px, ' + y + 'px)';
var bubble = document.getElementById('bubble');
// yes, uppercase 'Moz'
// opera is webkit now, so you don't have to worry about that anymore.
bubble.style.transform = bubble.style.webkitTransform = bubble.style.MozTransform = bubble.style.msTransform = t;
```