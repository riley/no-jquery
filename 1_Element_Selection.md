
## Element Selection

#### By id
**jQuery**
`$('#some-element')`

**DOM API**
`document.getElementById('some-element')`
or `document.querySelector('#some-element')`

#### By class
**jQuery** `$('.some-element')`

**DOM API** `document.getElementsByClassName('some-element')`
or `document.querySelectorAll('.some-element')`

#### By tag name
**jQuery** `$('div')`

**DOM API** `document.getElementsByTagName('div')` or  `document.querySelectorAll('div')`

#### By pseudo-class
**jQuery**`$('#some-widget :first-child')`

**DOM API** `document.querySelectorAll('#some-widget :first-child')`

#### Parent

**jQuery**
```javascript
var darthVader = $('#luke-skywalker').parent();
```

**DOM API**
```javascript
var darthVader = document.getElementById('luke-skywalker').parentNode;
```

#### Children
**jQuery**
```javascript
$('#parent-el').children()
```

**DOM API**
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
**jQuery**
```javascript
$('#parent a')
```

**DOM API**
```javascript
document.querySelectorAll('#parent a')
```

### Next

**jQuery**
```javascript
var interestingChannel = $('.boring-channel').next();
```

**DOM API**
```javascript
var interestingChannel = document.querySelector('.boring-channel').nextSibling;
```

#### Is an element empty?

jquery | DOM API
--- | ---
`if ($('#outer').is(':empty'))` | `if(!document.getElementById('outer').hasChildNodes())`

#### Excluding from a set
**jQuery**
```javascript
$('button').not('.disabled')
$('button:not(.disabled)')
```
**DOM API**
```javascript
// what!
document.querySelectorAll('button:not(.disabled)')
```
#### Multiple selectors
**jQuery** `$('div, a, li')`

**DOM API** `document.querySelectorAll('div, a, li')`

#### Things to keep in mind
* `getElementsByTagName()` and `querySelectorAll()` return an HTMLCollection and a NodeList respectively, which are not the same thing
