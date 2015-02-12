## Easy wins.

You're listening to this, and thinking, I'm not going to refactor all my code. That's not what I'm saying. At first, just go native when you need the performance, like in loops.

Here are a few examples of when jQuery actually makes the code [more verbose](http://www.doxdesk.com/img/updates/20091116-so-large.gif) than it would otherwise be because of the almighty `$`.

* `this.id` vs `$(this).attr('id')`. yes, always.
* `this.value` vs `$(this).val()`
* `this.selectedIndex` on a `<select>` to get the selected index
* `this.options` on a `<select>` to get a list of options
* `this.rows` on a `<table>` to get a collection of `<tr>` elements
* `this.cells` on a `<tr>` to get a collection of cells
* `this.parentNode` to get a direct parent
* `this.checked` vs `$(this).is(':checked')` to get the state of a `checkbox`
* `this.selected` to get the state of an `option`
* `this.disabled` to read the state of an `input` or `button`
* `this.readOnly` to get the readOnly state of an `input`
* `this.href` on an `<a>, <link>` to get its `href`
* `this.hostname` on an `<a>` to get the domain of its `href`
* `this.pathname` on an `<a>` to get the path of its `href`
* `this.search` on an `<a>` to get the querystring of its `href`
* `this.src` on any element where you can set a `src`