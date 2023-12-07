# HTTP API

## Introduction

HTTP stands for Hypertext Transfer Protocol and is the foundational communication protocol for the entire Internet. Even if this is your first experience with HTTP it's likely that you probably understand more than you realize. At its most basic level, HTTP works like this:

- "Hello server XYZ, may I please have file abc.html"
- "Well hello there little client, yes you may, here it is"

There are many different methods to send HTTP requests in PHP. The purpose of the WordPress HTTP API is to support as many of those methods as possible and use the one that is the most suitable for the particular request.

The WordPress HTTP API can also be used to communicate and interact with other APIs like the Twitter API or the Google Maps API.

## HTTP methods

HTTP has several methods, or verbs, that describe particular types of actions. Though a couple more exist, WordPress has pre-built functions for three of the most common. Whenever an HTTP request is made a method is also passed with it to help the server determine what kind of action the client is requesting.

### GET

GET is used to retrieve data. This is by far the most commonly used verb. Every time you view a website or pull data from an API you are seeing the result of a GET request. In fact your browser sent a GET request to the server you are reading this on and requested the data used to build this very article.

### POST

POST is used to send data to the server for the server to act upon in some way. For example, a contact form. When you enter data into the form fields and click the submit button the browser takes the data and sends a POST request to the server with the text you entered into the form. From there the server will process the contact request.

### HEAD

HEAD is much less well known than the other two. HEAD is essentially the same as a GET request except that it does not retrieve the data, only information about the data. This data describes such things as when the data was last updated, whether the client should cache the data, what type the data is, etc. Modern browsers often send HEAD requests to pages you have previously visited to determine if there are any updates. If not, you may actually be seeing a previously downloaded copy of the page instead of using bandwidth needlessly pulling in the same copy.

All good API clients utilize HEAD before performing a GET request to potentially save on bandwidth. Though it will require two separate HTTP requests if HEAD says there is new data, the data size with a GET request can be very large. Only using GET when HEAD says the data is new or should not be cached will help save on expensive bandwidth and load times.

### Custom Methods

There are other HTTP methods, such as PUT, DELETE, TRACE, and CONNECT. These methods will not be covered in this article as there aren't pre-built methods to utilize them in WordPress, nor is it yet common for APIs to implement them.

Depending upon how your server is configured you can also implement additional HTTP methods of your own. It is always a gamble to go outside of the standard methods, and places huge potential limitations on other developers creating clients to consume your site or API, however it is possible to utilize any method you wish with WordPress. We will briefly touch on how to do that in this article.

## Response codes

HTTP utilizes both numeric and string response codes. Rather than go into a lengthy explanation of each, here are the standard response codes. You can define your own response codes when creating APIs, however unless you need to support specific types of responses it may be best to stick to the standard codes. Custom codes are usually in the 1xx ranges.

### Code Classes

The type of response can quickly be seen by the leftmost digit of the three digit codes.

| Status Code | Description |
| --- | --- |
| 2xx | Request was successful |
| 3xx | Request was redirected to another URL |
| 4xx | Request failed due to client error. Usually invalid authentication or missing data |
| 5xx | Request failed due to a server error. Commonly missing or misconfigured configuration files |

**Common Codes**

These are the most common codes you will encounter.

| Status Code | Description |
| --- | --- |
| 200 | OK – Request was successful |
| 301 | Resource was moved permanently |
| 302 | Resource was moved temporarily |
| 403 | Forbidden – Usually due to an invalid authentication |
| 404 | Resource not found |
| 500 | Internal server error |
| 503 | Service unavailable |

## GETting data from an API

[GitHub](https://github.com/) provides an excellent API that does not require app registration for many public aspects, so to demonstrate some of these methods, examples will target the GitHub API.

GETting data is made incredibly simple in WordPress through the [`wp_remote_get()`](https://developer.wordpress.org/reference/functions/wp_remote_get/) function. This function takes the following two arguments:

1. `$url` – Resource to retrieve data from. This must be in a standard HTTP format.
2. `$args` – OPTIONAL – You may pass an array of arguments in here to alter behavior and headers, such as cookies, follow redirects, etc.

The following defaults are assumed, though they can be changed via the $args parameter:

- `method` – GET
- `timeout` – 5 – How long to wait before giving up
- `redirection` – 5 – How many times to follow redirects.
- `httpversion` – 1.0
- `blocking` – true – Should the rest of the page wait to finish loading until this operation is complete?
- `headers` – array()
- `body` – null
- `cookies` – array()

Let's use the URL to a GitHub user account and see what sort of information we can get.

```
$response = wp_remote_get( 'https://api.github.com/users/blobaugh' );
```

`$response` will contain all the headers, content, and other meta data about our request.

```
Array(
  [headers] => Array(
    [server] => nginx
    [date] => Fri, 05 Oct 2012 04:43:50 GMT
    [content-type] => application/json; charset=utf-8
    [connection] => close
    [status] => 200 OK
    [vary] => Accept
    [x-ratelimit-remaining] => 4988
    [content-length] => 594
    [last-modified] => Fri, 05 Oct 2012 04:39:58 GMT
    [etag] => "5d5e6f7a09462d6a2b473fb616a26d2a"
    [x-github-media-type] => github.beta
    [cache-control] => public, s-maxage=60, max-age=60
    [x-content-type-options] => nosniff
    [x-ratelimit-limit] => 5000
  )
  [body] => {"type":"User","login":"blobaugh","gravatar_id":"f25f324a47a1efdf7a745e0b2e3c878f","public_gists":1,"followers":22,"created_at":"2011-05-23T21:38:50Z","public_repos":31,"email":"ben@lobaugh.net","hireable":true,"blog":"http://ben.lobaugh.net","bio":null,"following":30,"name":"Ben Lobaugh","company":null,"avatar_url":"https://secure.gravatar.com/avatar/f25f324a47a1efdf7a745e0b2e3c878f?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png","id":806179,"html_url":"https://github.com/blobaugh","location":null,"url":"https://api.github.com/users/blobaugh"}
  [response] => Array(
    [preserved_text 5237511b45884ac6db1ff9d7e407f225 /] => 200
    [message] => OK
  )
  [cookies] => Array()
  [filename] =>
)
```

All of the same helper functions can be used on this function as with the previous two. The exception here being that HEAD never returns a body, so that element will always be empty.

### GET the body you always wanted

Just the body can be retrieved using [`wp_remote_retrieve_body()`](https://developer.wordpress.org/reference/functions/wp_remote_retrieve_body/). This function takes just one parameter, the response from any of the other [`wp_remote_X`](https://developer.wordpress.org/?s=wp_remote_&post_type%5B%5D=wp-parser-function) functions where retrieve is not the next value.

```
$response = wp_remote_get( 'https://api.github.com/users/blobaugh' );
$body     = wp_remote_retrieve_body( $response );
```

Still using the GitHub resource from the previous example, `$body` will be.

```
{"type":"User","login":"blobaugh","public_repos":31,"gravatar_id":"f25f324a47a1efdf7a745e0b2e3c878f","followers":22,"avatar_url":"https://secure.gravatar.com/avatar/f25f324a47a1efdf7a745e0b2e3c878f?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png","public_gists":1,"created_at":"2011-05-23T21:38:50Z","email":"ben@lobaugh.net","following":30,"name":"Ben Lobaugh","company":null,"hireable":true,"id":806179,"html_url":"https://github.com/blobaugh","blog":"http://ben.lobaugh.net","location":null,"bio":null,"url":"https://api.github.com/users/blobaugh"}
```

If you do not have any other operations to perform on the response other than getting the body you can reduce the code to one line with.

```
$body = wp_remote_retrieve_body( wp_remote_get( 'https://api.github.com/users/blobaugh' ) );
```

Many of these helper functions can be used on one line similarly.

### GET the response code

You may want to check the response code to ensure your retrieval was successful. This can be done via the [`wp_remote_retrieve_response_code()`](https://developer.wordpress.org/reference/functions/wp_remote_retrieve_response_code/) function:

```
$response = wp_remote_get( 'https://api.github.com/users/blobaugh' );
$http_code = wp_remote_retrieve_response_code( $response );
```

If successful `$http_code` will contain `200`.

### GET a specific header

If your desire is to retrieve a specific header, say last-modified, you can do so with [`wp_remote_retrieve_header()`](https://developer.wordpress.org/reference/functions/wp_remote_retrieve_header). This function takes two parameters.

1. `$response` – The response from the get call.
2. `$header` – Name of the header to retrieve.

To retrieve the last-modified header.

```
$response      = wp_remote_get( 'https://api.github.com/users/blobaugh' );
$last_modified = wp_remote_retrieve_header( $response, 'last-modified' );
```

`$last_modified` will contain `[last-modified] => Fri, 05 Oct 2012 04:39:58 GMT`

You can also retrieve all of the headers in an array with `wp_remote_retrieve_headers( $response )`.

### GET using basic authentication

APIs that are secured more provide one or more of many different types of authentication. A common, though not highly secure, authentication method is HTTP Basic Authentication. It can be used in WordPress by passing ‘Authorization' to the second parameter of the [`wp_remote_get()`](https://developer.wordpress.org/reference/functions/wp_remote_get/) function, as well as the other HTTP method functions.

```
$args = array(
  'headers' => array(
    'Authorization' => 'Basic ' . base64_encode( YOUR_USERNAME . ':' . YOUR_PASSWORD )
  )
);
wp_remote_get( $url, $args );
```

## POSTing data to an API

The same helper methods ([`wp_remote_retrieve_body()`](https://developer.wordpress.org/reference/functions/wp_remote_retrieve_body/), etc.) are available for all of the HTTP method calls, and utilized in the same fashion.

POSTing data is done using the [`wp_remote_post()`](https://developer.wordpress.org/reference/functions/wp_remote_post/) function, and takes exactly the same parameters as [`wp_remote_get()`](https://developer.wordpress.org/reference/functions/wp_remote_get/). It should be noted here that you are required to pass in ALL of the elements in the array for the second parameter. The Codex provides the default acceptable values. You only need to care right now about the data you are sending so the other values will be defaulted.

To send data to the server you will need to build an associative array of data. This data will be assigned to the `'body'` value. From the server side of things the value will appear in the `$_POST` variable as you would expect. i.e. if `body => array( 'myvar' => 5 )` on the server `$_POST['myvar'] = 5`.

Because GitHub does not allow POSTing to the API used in the previous example, this example will pretend that it does. Typically if you want to POST data to an API you will need to contact the maintainers of the API and get an API key or some other form of authentication token. This simply proves that your application is allowed to manipulate data on the API the same way logging into a website as a user does to the website.

Lets assume we are submitting a contact form with the following fields: name, email, subject, comment. To setup the body we do the following:

```
$body = array(
  'name'    => 'Jane Smith',
  'email'   => 'some@email.com',
  'subject' => 'Checkout this API stuff',
  'comment' => 'I just read a great tutorial. You gotta check it out!',
);
```

Now we need to set up the rest of the values that will be passed to the second parameter of [`wp_remote_post()`](https://developer.wordpress.org/reference/functions/wp_remote_post/).

```
$args = array(
  'body'        => $body,
  'timeout'     => '5',
  'redirection' => '5',
  'httpversion' => '1.0',
  'blocking'    => true,
  'headers'     => array(),
  'cookies'     => array(),
);
```

Then of course to make the call.

```
$response = wp_remote_post( 'https://example.com', $args );
```

## HEADing off bandwidth usage

It can be pretty important, and sometimes required by the API, to check a resource status using HEAD before retrieving it. On high traffic APIs, GET is often limited to a number of requests per minute or hour. There is no need to even attempt a GET request unless the HEAD request shows that the data on the API has been updated.

As mentioned previously, HEAD contains data on whether or not the data has been updated, if the data should be cached, when to expire the cached copy, and sometimes a rate limit on requests to the API.

Going back to the GitHub example, here are few headers to watch out for. Most of these headers are standard, but you should always check the API docs to ensure you understand which headers are named what, and their purpose.

- `x-ratelimit-limit` – Number of requests allowed in a time period.
- `x-ratelimit-remaining` – Number of remaining available requests in time period.
- `content-length` – How large the content is in bytes. Can be useful to warn the user if the content is fairly large.
- `last-modified` – When the resource was last modified. Highly useful to caching tools.
- `cache-control` – How should the client handle caching.

The following will check the HEAD value of my GitHub user account:

```
$response = wp_remote_head( 'https://api.github.com/users/blobaugh' );
```

`$response` should look similar to

```
Array(
  [headers] => Array(
    [server] => nginx
    [date] => Fri, 05 Oct 2012 05:21:26 GMT
    [content-type] => application/json; charset=utf-8
    [connection] => close
    [status] => 200 OK
    [vary] => Accept
    [x-ratelimit-remaining] => 4982
    [content-length] => 594
    [last-modified] => Fri, 05 Oct 2012 04:39:58 GMT
    [etag] => "5d5e6f7a09462d6a2b473fb616a26d2a"
    [x-github-media-type] => github.beta
    [cache-control] => public, s-maxage=60, max-age=60
    [x-content-type-options] => nosniff
    [x-ratelimit-limit] => 5000
  )
  [body] =>
  [response] => Array(
    [preserved_text 39a8515bd2dce2aa06ee8a2a6656b1de /] => 200
    [message] => OK
  )
  [cookies] => Array(
  )
  [filename] =>
)
```

All of the same helper functions can be used on this function as with the previous two. The exception here being that HEAD never returns a body, so that element will always be empty.

## Make any sort of request

If you need to make a request using an HTTP method that is not supported by any of the above functions do not panic. The great people developing WordPress already thought of that and lovingly provided [`wp_remote_request()`](https://developer.wordpress.org/reference/functions/wp_remote_request/). This function takes the same two parameters as [`wp_remote_get()`](https://developer.wordpress.org/reference/functions/wp_remote_get/), and allows you to specify the HTTP method as well. What data you need to pass along is up to your method.

To send a DELETE method example you may have something similar to the following:

```
$args     = array(
  'method' => 'DELETE',
);
$response = wp_remote_request( 'http://some-api.com/object/to/delete', $args );
```

## Introduction to caching

Caching is a practice whereby commonly used objects or objects requiring significant time to build are saved into a fast object store for quick retrieval on later requests. This prevents the need to spend the time fetching and building the object again. Caching is a vast subject that is part of website optimization and could go into an entire series of articles by itself. What follows is just an introduction to caching and a simple yet effective way to quickly setup a cache for API responses.

Why should you cache API responses? Well, the big elephant in the room is because external APIs slow down your site. Many consultants will tell you tapping into external APIs will improve the performance of your website by reducing the amount of connections and processing it performs, as well as costly bandwidth, but sometimes this is simply not true.

It is a fine balancing act between the speed your server can send data and the amount of time it takes for the remote server to process a request, build the data, and send it back. The second glaring aspect is that many APIs have a limited number of requests in a time period, and possibly a limit to the number of connections by an application at once. Caching helps solve these dilemmas by placing a copy of the data on your server until it needs to be refreshed.

## When should you cache?

The snap answer to this is **always**, but again there are times when you should not. If you are dealing with real time data or the API specifically says not to cache in the headers you may not want to cache, but for all other situations it is generally a good idea to cache any resources retrieved from an API.

## WordPress Transients

WordPress Transients provide a convenient way to store and use cached objects. Transients live for a specified amount of time, or until you need them to expire when a resource from the API has been updated. Using the transient functionality in WordPress may be the easiest to use caching system you ever encounter. There are only three functions to do all the heavy lifting for you.

### Cache an object ( Set a transient )

Caching an object is done with the [`set_transient()`](https://developer.wordpress.org/reference/functions/set_transient/) function. This function takes the following three parameters:

1. `$transient` – Name of the transient for future reference.
2. `$value` – Value of the transient.
3. `$expiration` – How many seconds from saving the transient until it expires.

An example of caching the GitHub user information response from above for one hour would be

```
$response = wp_remote_get( 'https://api.github.com/users/blobaugh' );
set_transient( 'prefix_github_userinfo', $response, 60 * 60 );
```

### Get a cached object ( Get a transient )

Getting a cached object is quite a bit more complex than setting a transient. You need to request the transient, but then you also need to check to see if that transient has expired and if so fetch updated data. Usually the `set_transient()` call is made inside of the `get_transient()` call. Here is an example of getting the transient data for the GitHub user profile:

```
$github_userinfo = get_transient( 'prefix_github_userinfo' );
if ( false === $github_userinfo ) {
  // Transient expired, refresh the data
  $response = wp_remote_get( 'https://api.github.com/users/blobaugh' );
  set_transient( 'prefix_github_userinfo', $response, HOUR_IN_SECONDS );
}
// Use $github_userinfo as you will
```

## Delete a cached object (Delete a transient)

Deleting a cached object is the easiest of all the transient functions, simply pass it a parameter of the name of the transient and you are done.

To remove the Github user info:

```
delete_transient( 'blobaugh_github_userinfo' );
```

More information on transients can be found [here](https://developer.wordpress.org/apis/transients/).