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
```
Using jQuery keeps you from having to manually set headers, which is just one more point of failure and potential typos. If you're typing this over and over, maybe consider a utility function, but a whole library? Probably not.

#### JSON

Say that we want to update something one the server, and we want to send it via JSON. That's a pretty normal use case, right?

```javascript
$.ajax({
    url: ''
});
```

#### Uploading

#### CORS

#### JSONP

#### glossing over ugly bits