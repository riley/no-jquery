## AJAX

Browser support for AJAX has been around forever, but since it was designed by Microsoft back in 1999, it doesn't look like a lot of other JavaScript stuff

#### GET Request

Say that we want to get a record of twitter data. We'll need an api key as a parameter.

**jQuery**
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

**XMLHttpRequest Object**

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

**jQuery**
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

**XMLHttpRequest Object**

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
xhr.send(postData);
```
Using jQuery keeps you from having to manually set headers, which is just one more point of failure and potential typos. If you're typing this over and over, maybe consider a utility function, but a whole library? Probably not.

#### JSON

Say that we want to update something on the server, and we want to send it via JSON. That's a pretty normal use case, right?

**jQuery**
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

**XMLHttpRequest**
```javascript
var xhr = new XMLHttpRequest();
var postData = JSON.stringify({name: "Charles de Batz-Castelmore d'Artagnan"});
xhr.open('PUT', 'his-majestys-service/user/1611');
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.onload = function () {
    if (xhr.status === 200) {
        var userInfo = JSON.parse(xhr.responseText);
    }
};
xhr.send(postData);

```

#### Uploading

There is a way to upload files in IE9. You have to post a `<form>` that contains an `<input type="file">`. So, godspeed. In modern browsers you'll have to do something more like this with the [File API](https://developer.mozilla.org/en-US/docs/Web/API/File).

Say that we have this markup, and the user has selected a file.

```javascript
<input type="file" id="input-file">
```

**jQuery**

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

This shows some of the problems I've run into while working with `$.ajax()`. While jQuery's apis have kept up with the growth of browser features, it does so by shoehorning configuration into existing methods. I know that I want to upload a file asynchronously, but I have to dig through the docs to find a flag that's not immediately intuitive to get things to work. `contentType: false`? What? This is inserted so jQuery doesn't insert its own headers, since the browser is required to set them so the multi-part request is sent properly by the browser.

If we want to send the file as a single payload (really only good for small files).

```javascript
var file = $('#witty-banter/dialogue')[0].files[0]

$.ajax({
    url: 'his-majestys-service/dialogue',
    method: 'POST',
    contentType: file.type,
    processData: false, // this is another trip to stackoverflow.
    data: file
});
```

**XMLHttpRequest**

multi-part encoded

```javascript
var formData = new FormData();
var file = document.getElementById('witty-banter').files[0];
var xhr = new XMLHttpRequest();

formData.append('file', file);
formData.open('POST', 'his-majestys-service');
formData.send(formData);
// oh snap.
```
Now the file as the payload of the request:
```javascript
var file = document.getElementById('witty-banter').files[0];
var xhr = new XMLHttpRequest();

formData.open('POST', 'his-majestys-service');
formData.setRequestHeader('Content-Type', file.type);
formData.send(file);
```
The File API is quite powerful, and will make life a lot easier once we don't have to worry about IE9 anymore.

#### CORS

**C**ross **O**rigin **R**esource **S**haring is not going away, and there are lots of use cases -- loading external fonts, images that will be used in a canvas or as webgl textures. This is going to come up in even medium-sized projects, so it's something to be aware of.

So, assuming the server you're requesting an asset from is set up to respond with the correct headers --

**jQuery**

```javascript
$.ajax({
    url: 'http://elsewhere.com',
    contentType: 'text/plain',
    data: 'When we look to the individuals of the same variety or sub-variety of our older cultivated plants and animals...',
    beforeSend: function (xhr) {
        // PRO TIP: cookies are not sent by default on cross-origin requests
        xhr.withCredentials = true;
    }
})
```

**XMLHttpRequest**
```javascript
var xhr = new XMLHttpRequest();
xhr.open('POST', 'http://elsewhere.com');
xhr.withCredentials = true;
xhr.setRequestHeader('Content-Type', 'text/plain');
xhr.send('When we look to the individuals of the same variety or sub-variety of our older cultivated plants and animals...');
```

This is the exact same functionality as jQuery, only we had to configure the jQuery object differently. There was no benefit to loading the library just for `$.ajax`.

Notes:

- To send cross-domain requests in IE9, you have to use Microsoft's [XDomainRequest](https://msdn.microsoft.com/en-us/library/ie/cc288060%28v=vs.85%29.aspx) so...sigh. It's really just XMLHttpRequest, only you have to read more docs.
- This doesn't work with jQuery either. It depends on XMLHttpRequest internally, so you'll have to use a plugin to get it to work. Now your dependencies have dependencies. (Yo dawg, I herd you like dependencies).
- OR you could wirte a little shim yourself:

```javascript
// check to see if we need the shim
if (typeof new XMLHttpRequest().withCredentials === 'undefined') {
    var xdr = new XDomainRequest();
    xdr.open('POST', 'http://elsewhere.com');
    xdr.send('blah');
}
```
that was pretty easy.


#### JSONP

I'm assuming that everyone knows what JSONP is. CORS is better, but in a pinch, use JSONP.

**jQuery**
```javascript
$.ajax({
    url: "http://configured-jsonp-api/uniqueid",
    jsonp: 'callback', // not strictly necessary
    dataType: 'jsonp',
    data: { name: 'your mom' },
    success: function (data) {

    }
})
```
not too bad. just for code clarity, jQuery has some advantages here. You might want to write a helper function for the XMLHttpRequest version.

**XMLHttpRequest**
```javascript
// global callbacks, gross.
window.handleJSONP = function (data) {
    // handle requested
}

var scriptElement = document.createElement('script');
scriptElement.setAttribute('src', 'http://configured-jsonp-api/uniqueid?callback=handleJSONP&foo=' + Date.now());
document.body.appendChild(scriptElement);
```
