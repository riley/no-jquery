## AJAX

Browser support for AJAX has been around forever, but since it was designed by Microsoft back in 1999, it doesn't look like a lot of other JavaScript stuff

#### GET Request

Say that we want to get a record of twitter data. We'll need an api key as a parameter.

```javascript
$.ajax({
    url: 'http://api.tumblr.com/search',
    data: {
        term: 'cat gifs',
        api_key: 1234567
    },
    success: function (data) {
        console.log('cat gifs!', data);
    },
    error: function (status) {
        console.log('something blew up!', status);
    }
});
```

XMLHttpRequest Object

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', encodeURI('http://api.tumblr.com/search?term=cat gifs&api_key=1234567'));
xhr.onload = function () {
    if (xhr.status === 200) {
        console.log(xhr.responseText);
    } else {
        conosle.log('something blew up', xhr.status);
    }
}
xhr.send(null);
```

this code is less lines, but it's a bit more difficult to read, since there's some extra logic, and you're querying the state of something, and not just reading arguments like modern js convention (jQuery promises aside for now).

#### POST Request

Maybe we want to POST something, like you made a poem about your own cat, and you want to share it with the world.

```javascript
$.ajax({
    url: 'http://api.tumblr.com/v2/blog/my-cat-blog/post',
    method: 'POST',
    data: {
        post_data: 'Cats. Are. The. BEST',
        api_key: 1234567
    },
    success: function (data) {
        console.log('posted to tumblr!', data);
    },
    error: function (status) {
        console.log('something blew up!', status);
    }
});
```

XMLHttpRequest Object

```javascript
var xhr = new XMLHttpRequest();
var postData = 'Cats. Are. The. BEST';
xhr.open('POST', encodeURI('http://api.tumblr.com/v2/blog/my-cat-blog/post'));
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.onload = function () {
    if (xhr.status === 200) {
        console.log('posted to tumblr!', xhr.responseText);
    } else {
        console.log('something blew up!', xhr.status);
    }
}
xhr.send(null);
```
Using jQuery keeps you from having to manually set headers, which is just one more point of failure and potential typos. If you're typing this over and over, maybe consider a utility function, but a whole library? Probably not.

#### JSON

Say that we want to update something one the server, and we want to send it via JSON. That's a pretty normal use case, right?

```javascript
$.ajax({
    url: 'his-majestys-service/user/1611'
    // say that we're updating a record
    method: 'PUT',
    contentType: 'application/json',
    // make sure data object is not url-encoded
    processData: false,
    data: JSON.stringify({
        name: "Charles de Batz-Castelmore d'Artagnan"
    })
});
```
the default way of sending data in jQuery is for form data, which is why you need configuration to do much else. That's fine, but the internets are changing.

DOM API
```javascript
var xhr = new XMLHttpRequest();
xhr.open('PUT', 'his-majestys-service/user/1611');
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.onload = function () {
    if (xhr.status === 200) {
        var userInfo = JSON.parse(xhr.responseText);
    }
};
xhr.send(JSON.stringify({
    name: "Charles de Batz-Castelmore d'Artagnan"
}));

```

#### Uploading

There is a way to upload files in IE9. You have to post a `<form>` that contains an `<input type="file">`. So, godspeed. In modern browsers you'll have to do something more like this with the [File API](https://developer.mozilla.org/en-US/docs/Web/API/File).

Say that we have this markup, and the user has selected a file.

```javascript
<input type="file" id="input-file">
```

jQuery
We're going to upload the file as a multi-part encoded request.
```javascript
// the 0th element of a jQuery array is of course the native DOM element
var file = $('#witty-banter')[0].files[0];
var formData = new FormData(); // also IE10+

// FormData is like a hashmap, and is an abstraction for sending key/value pairs
formData.append('file', file);

$.ajax({
    url: 'his-majestys-service/dialogue',
    method: 'POST',
    contentType: false,
    processData: false,
    data: formData
});
```

This shows some of the problems I've run into while working with `$.ajax()`. While jQuery's apis have kept up with the growth of browser features, it does so by shoe-horning configuration into existing methods. I know that I want to upload a file asynchronously, but I have to dig through the docs to find a flag that's not immediately intuitive to get things to work. `contentType: false`? What? This is inserted so jQuery doesn't insert it's own headers, since the browser is required to set them so the multi-part request is sent properly by the browser.

If we want to send the file as a single payload (really only good for small files).

var file = $('#witty-banter/dialogue')[0].files[0]

$.ajax({
    url: 'his-majestys-service/dialogue',
    method: 'POST',
    contentType: file.type,
    processData: false, // this is another trip to stackoverflow.
    data: file
});
```

XMLHttpRequest

multi-part encoded

```javascript
var formData = new FormData();
var file = document.getElementById('witty-banter').files[0];
var xhr = new XMLHttpRequest();

formData.append('file', file);
```

#### CORS

#### JSONP

#### glossing over ugly bits