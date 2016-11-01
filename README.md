# Shawn Bullock's NodeJS Study Notes
##### (Version 11.1.2016:1)

## Introduction

The following is a rendition of my own personal study notes about Node.JS that I have decided to share with you. While I attempt to cover all the basics I make no attempt to be thorough.  I have removed some of the best parts and, in true mentorship spirit, reframed them in the form of a question or exercise that you can followup on.

NOTE: Words in **Bold** are defined at the bottom of this document beneath the Dictionary header.  Some words are in _italic_.  These words are defined at their first use.

## Overview

**NodeJS** or (**Node.js**) is an **asynchronous** **event-driven** javascript platform based on Google's **V8** engine.  **NodeJS** is not a **Web Server** or anything else, specifically.  It is just a Javascript environment that can do anything you want it to do.  It is often used to create a **Web Server** or **REST** server via **Express.js**, but it can also be used to create system utilities, file processors, and more.  Any of which may or may not have anything to do with the Internet.

All **Web Browsers** support some form of Javascript.  Google's Chrome also uses the **V8** engine.  When executing Javascript in a **Web Browser** the code has access to the **HTML DOM**.  When executing code within **NodeJS** there is no **HTML DOM**.  Not all Javascript code can execute without modification in both environments (even though a proclaimed benefit of using **NodeJS** is the ability to share code between the **NodeJS** on the **server** and the **Web Browser**).

The code that runs within a **NodeJS** instance is called a **NodeJS Application** (I call it a _Node App_).  **NodeJS** is single-threaded.  This means that each _Node app_ can only process one **event** simultaneously, even though it appears to be servicing potentially 100's of 1000's of **events** at a time.

A **NodeJS** _project_ is the collection of files that comprise the _Node app_.

**NodeJS** is very efficient when processing non-blocking (**asynchronous**) (doing something else while waiting) IO (tasks).  IO: file operations, database operations, _HTTP_ operations, network communications (sockets), etc., can be very time consuming.  Rather than blocking (waiting) for such operations to complete, **NodeJS** offloads such tasks to the background until completed.  Once completed, via a callback, processing resumes within the **NodeJS** environment.  There are other kinds of tasks that are very intensive, such as image processing, AI, sound compression, etc., that **NodeJS** is not good at processing.  Such tasks should be handled by another type of server more specialized to the purpose.

##### Exercises

* Does jQuery work on **NodeJS**?  Why or why not?


## Launching a _Node app_

Many **NodeJS** applications use file names such as `app.js`, `server.js`, or even `basic-server.js` as the main file and starting point.  It can be named anything.  I choose to use `server.js` for all my projects, and others tend to use something else such as `basic-server.js`, `app.js`, etc. for theirs.

To start a _Node app_, simply type:

`node <filename.js>`

Or

`node <path/to/<filename.js>` at the command line.

You will, of course, have to start the _Node app_ after every change.  There is a utility called `nodemon` that can launch your _Node app_ that will automatically restart after each save.


##### Exercises

* What does `npm start` do?


## NodeJS Packages

It is not productive to create everything from scratch.  To increase productivity, **NodeJS** provides a mechanism to re-use previously written code.  Such code is called a **package**.  **Packages** can be installed via a commandline utility called **NPM**.

**Packages** can be installed either _Global_ or _local_.  _Global_ **packages** are installed on the machine and can be used from the commandline or by all _Node apps_ from that single installation.  _Local_ **packages** are installed with each _Node app_ and cannot be used by other _Node apps_.

When a _Node app_ usees a **package**, it is called a _dependency_.  This means that the _Node app_ _depends_ on the **package** in order to function properly.  **Packages** can also use other **packages** as _dependencies_.  **Packages** have _version numbers_.  A _version number_ indicates a specific iteration of code or features that the **package** supports.  When a _Node app_ _depends_ on a specific _version_ of a **package**, it avoids potentional problems and _conflicts_ that might arise if there was no _versioning_ in place.  A _conflict_ is when the _Node app_ expects a certain feature or behavior that does not exist, or behaves differently than expected.


##### To install a package:

**NodeJS** provides a utility called `npm` that manages **packages**.  With it you can install and remove **packages** easily.

Each application should have a `package.json` file in its root directory.  This file does not exist by default and must be created.  To create it, simply type:

`npm init` at the application root directory and follow the prompts.  To add a **NodeJS** package, simply type:

`npm install <name> --save` at the application root directory.

You can also remove a package via:

`npm remove <name>` at the application root directory.

When you get a _project_ from github the first time, it will usually contain the `package.json` file already.  But the **packages** will usually not be included with the git repository.  The **packages** will be listed in the `package.json` file.  To install all the **package** dependencies, simply type:

`npm intall` at the application root directory.

Some **packages** will behave more like applications and others more like functionality for YOUR _Node app_.  Those that behave more like applications (or utilities) must be installed _globally_ and those that provide functionality as a part of YOUR _Node app_ must be installed _locally_.  To install a _global_ **package**, simply type:

`npm install -g <name>` anywhere.


##### Exercises:

* Create a new _Node app_.  Add a `package.json` file
* What is the purpose of the `main` key/value in the `package.json` file?
* Install at least one **package** to the _project_
* What happens if you omit `--save` when installing a **package**?
* Install at least one **package** that is specific to the dev dependencies (HINT: testing usually will be dev specific (as in development, vs deployed to the end users and ready to use))
* Remove at least one **package** from the _project_
    * Validate that `package.json` reflects the change.  If not, figure out why and try again
* Install a specific _version_ of a package to the _project_
* What directory does NPM install _local_ **packages** to?
* What happens if you install a **package** at a different directory level than the application root directory?
* How can you avoid uploading **package** files to github?  (HINT: git.ignore... but how, specifically?)
* Install a _global_ package (`Nodemon`??)
* Can a _global_ **package** be a _dependency_ to your _Node app_?  Explain why or why not?
* Launch your **NodeJS** app using the dev configuration.


## Modules

A **module** is a code file in **NodeJS**.  Sometimes when people refer to a **package** they call it a **module**.  A .js file in **NodeJS** has a one-to-one correspondence to a **module**.  A **package**, on the other hand, is comprised of one ore more **modules**.  A _Node app_ is also comprised of one or more **modules** and may or may not depend on one or more **packages**.  However, when `require(...)`ing a **package** we may simply refer to it as a **module**.

To define a **module** just add a new .js file to the _project_.  If you are familiar with the **Web Browser** you might recall using something like `<SCRIPT SRC="file.js"></SCRIPT>` to load a Javascript file into the Web page (where `"file.js"` is any valid URL path).  That `file.js` is not technically the same thing as a **module** but the larger idea is similar.  In **NodeJS**, to use a **module**, the file that wishes to use it must _reference_ it thus:

`var file = require("./file.js");` (where `"./file.js"` is any valid file system path)

To declare a **module**, simply write any valid Javascript code in a .js file within the **NodeJS** _project_.  All functions and variables will be private (not visiable to other **modules**) by default.  To make anything visible to other parts of the _Node app_, you must _export_ it thus:

###### [file `"circle.js"`]
```javascript
var PI = Math.PI;
exports.area = function(r) {
  return PI * r * r;
};
exports.circumference = function(r) {
  return 2 * PI * r;
};
```

In the above example, `var PI = Math.PI;` will not be visible to any other parts of the _Node app_.  However, the functions `area(...)` and `circumference(...)` wil be.

To use them elsewhere, simply type:

##### [file `"foo.js"`]
```javascript
var circle = require("./circle.js");
console.log("The area of a circle of radius 4 is " + circle.area(4));
console.log("The circumference of a circle of radius 4 is " + circle.circumference(4));
```

You can also write:

##### [file `"foo.js"`]
```javascript
var area = require("./circle.js").area;
var circumference = require(".circle.js").circumference;
console.log("The area of a circle of radius 4 is " + area(4));
console.log("The circumference of a circle of radius 4 is " + circumference(4));
```

Why the two different ways?  In the first example `circle` can be used to access _any_ member of the `circle.js` **module**.  The second example shows a way to reference and use only specific members of the `circle.js` module.  Both are valid use cases and it is up to you which style to prefer.

There are actually two ways to export a module:

`exports.<member> = ...`

and

`module.exports.<member> = ...`

Both will achieve the same thing but should be used consistently throughout your _Node app_.


##### Exercises

* What is the difference between URL and a file system path? Why use one for `<SCRIPT>` tags and another for `require(...)` statements?
* What is the technical difference between the `exports.<member>` pattern and the `module.exports.<member>` pattern? (HINT: [A good explanation](http://www.sitepoint.com/understanding-module-exports-exports-node-js/)).  Why would you choose one over the other?
* Create a **module** and export a few functions or variables.  Use them from another part of your _Node app_.
  * Create another **module** using a different export pattern and use it from another part of your _Node app_.  Are there any differences?
* What happens if you `require("<url>");` instead of `require(<file system path>);`?


## HTTP

NOTE: This section is important to understand before continuing on to creating HTTP servers with **NodeJS**.  Be familiar with the Postman Chrome extension before continuing, it will make your life easier.


Read the following sections from [HTTP Basics](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html) at a minimum before continuing (skip any other section for now):

1. The Web
2. HyperText Transfer Protocol (HTTP)
3. Browser
4. Uniform Resource Locator (URL)
5. HTTP Protocol
6. HTTP Specifications
7. HTTP Request and Response Messages
8. HTTP Request Method
9. HTTP Response Method
10. HTTP Request Methods
11. Submitting HTML Form Data and Questy String
12. URL and URI

The other parts of the page are useful for your general understanding but not critical for understanding the rest of this study guide.


## Processing HTTP Requests with NodeJS

**NodeJS** has many **modules** built in.  Among them is the `"HTTP"` module.  This allows you to start an instance of an HTTP server and handle requests.  To do so is simple, simply type:

```javascript
var http = require("http");
var server = http.createServer(function(request, response) {
    // ...
});
var port = 8080;
server.listen(port);
```

All HTTP requests have at least a _request_ and a _response_.  The code above aptly names the arguments such, but sometimes you'll see `function(req, res)`.  It doesn't matter what they're called.  I prefer to be very precise.  A _request_ contains information from the client and hints about how to process that _request_.  A _response_ contains information from the _Node app_ that the client will need to continue properly.  There are two fundamental pieces of data contained within each _request_:

1. `request.method`: specifies what _HTTP_ method to use (GET, POST, UPDATE, DELETE, OPTIONS, etc.).
2. `request.url`: the HTTP URI path that the _client_ desires.

There are two other important pieces of data that you can use:

3. `request.headers`: lists all HTTP headers included as part of the request
4. BODY: this isn't a property per se, but a request may include data.  This data is called a BODY.

All HTTP responses should end in a consistent state.  This means, at the very least, if nothing else, with a suitable status code.  `response.end();` will end the response stream with a default status code.

```javascript
var http = require("http");
var server = http.createServer(function(request, response) {
    response.end();
});
var port = 8080;
server.listen(port);
```

To be more useful than the above, we will need handle the _HTTP_ method accordingly:

```javascript
var http = require("http");
var server = http.createServer(function(request, response) {
    if (request.method === "GET") {
        // process GET request...
    }

    else if (request.method === "POST") {
        // process POST request...
    }

    else {
        // not prepared to handle the request.method, respond with appropriate error
    }
});
var port = 8080;
server.listen(port);
```

To be even more useful yet, we can handle individual URLs thus:

```javascript
var http = require("http");
var server = http.createServer(function(request, response) {
    if (request.method === "GET") {
        if (request.url === "<url1>") {
            // process <url1>...
            // status OK (unless its not)
        }
        else if (request.url === "<url2>") {
            // process <url2>...
            // status OK (unless its not)
        }
        else {
            /// respond with appropriate error
        }
    }

    else if (request.method === "POST") {
        if (request.url === "<url1>") {
            // process <url1>...
            // status OK (unless its not)
        }
        else if (request.url === "<url2>") {
            // process <url2>...
            // status OK (unless its not)
        }
        else {
            /// respond with appropriate error
        }
    }

    else {
        // not prepared to handle the request.method, respond with appropriate error
    }
});
var port = 8080;
server.listen(port);
```

Please note all the different places where we respond with an appropriate error above.

The `request.url` property returns the URL.  But sometimes we might want specific information about it.  **NodeJS** includes the `url` **module**.  For more information, visit the [official documentation](https://nodejs.org/docs/latest/api/url.html).  To use:

```javascript
var http = require("http");
var url = require("url");
var server = http.createServer(function(request, response) {
    var uri = url.parse(request.url).pathname;
    // ...
});
var port = 8080;
server.listen(port);
```

It is often useful for the _client_ to send data with a _request_.  One way is via the SUBMIT button on a form.  This will submit the form data with the _request_ (see the serversideconcepts.demo project to see this in action).  Another way is to attach raw data.

Some programming languages, like Java or C#, provide a property that automatically parses and returns the _request_ BODY.  **NodeJS** does not.  To do so, we have to parse it ourselves, thus:

``` javascript
//...
function collectData(request, callback) {
    var data = "";

    request.on("data", function(chunk) {
        data += chunk;
    });

    request.on("end", function() {
        callback(data);
    })
};
//...
```

As data fragments arrive, called `chunk` in the above example, we keep adding it to another variable called `data` until complete.  The format of data can be anything the _client_ sends (XML, form data, JSON, TEXT, BINARY, etc.).

Suppose a user submits a form.  The _client_ will POST a _request_ using form data.  HTML form data resembles a querystring (visit [Wikipedia](https://en.wikipedia.org/wiki/Query_string) for more information about the Querystring).  The following example demonstrates parsing form data:

```javascript
var querystring = require("querystring");
var http = require("http");
var url = require("url");
var server = http.createServer(function(request, response) {
    if (request.method === "POST") {
        var uri = url.parse(request.url).pathname;
        if (uri === "/<url1>") {
            collectData(request, function(data) {
                var formData = querystring.parse(data); // parse the form variables posted with the request

                console.log("firstName is " + formData.firstname);
                console.log("lastname is " + formData.lastname);
                console.log("email is " + formData.email);
            });
        }
        // ...
    }
});
var port = 8080;
server.listen(port);
```

We may want to respond with different types of data.  Perhaps you want to return JSON?  Maybe TEXT?  Or an image?  The _HTTP_ standard defines a `"content-type"` header that can be set to tell the _client_ what kind of data was returned.  You can see a complete [list of MIME types here](http://www.freeformatter.com/mime-types-list.html).  You can set this thus:

```
response.writeHead(status, { "Content-Type": "text/json" });
```

Sometimes when a client _requests_ one URL, the _server_ will respond by redirecting to another.  You can observe this readily.  Assuming curl is installed, type in the following command:

`curl -i http://google.com`

Observe the output.  It returns a status code indicating a redirect, and a URL to redirect to.  Now type in the the new URL:

`curl -i http://www.google.com` and you'll receive the desired output.

Open the Chrome Web Browser, and type into the address bar: `http://google.com` and notice that the URL changes to something else before showing the web page.  In effect, the Web Browser detected the redirect and followed it automatically.

We can write our own _server_ redirect using the following code:

``` javascript
// ...
sendRedirect(response, location, null);
// ...
function sendRedirect(response, location, status){
  status = status || 302;
  response.writeHead(status, {Location: location});
  response.end();
};
```

We often want to write content to the _response_ that the client can use.  This can be _HTML_, JSON, image data, or anything else.  To write content to the _response_, simply write:

`response.write(content);`

You can write as many times as you wish.


##### Exercises

* Create a basic _HTTP_ server.  Try to recall it from memory, if possible.
* When using `response.end();`, what default status code will be used?
* Does `response.end();` exit the current function?
* Create a response that returns status 404 when an invalid URL is requested
* Visit [Wikipedia HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).  What status code should be returned when:
  * Everything is successful/OK
  * A method is used that the _Node app_ is not prepared to process
  * An invalid URL is requested
  * Something bad happened
  * Something was created
  * _Node app_ encountered an internal error
* Whey should we respond with an appropriate error for each of the GET, POST, unknown, etc. methods and also each URL within it?
* Sometimes `request.url` and `url.parse(request.url).pathName` will return the same value.  Will this always be the case?  Why or why not?
* using the _RESTful_ architecture, should we submit a _request_ BODY with a GET method?  Why or why not?
* When receiving data, will it always arrive in a single `chunk`?
  * What is the default chunk size?  How can it be changed?
* Run the serversideconcepts.demo project as-is, observe the console log and form variables.  Note that those are the parsed key/value pairs.  Modify the example to show the raw form data.  Execute again.  Observe the output.
  * [optional]  Create an AJAX request on the client that POSTs JSON data instead, observe the difference and correctly parse it into a JSON object.
* What `Content-type` indicates an image file (JPEG, or PNG, or GIF)?
* Write code that uses `response.write(content);` before writing a custom _response_ header (such as a content-type).  What happens?
  * Visit [response.write documentation](https://nodejs.org/api/http.html#http_response_write_chunk_encoding_callback) to understand why.


## Processing Files with NodeJS

**NodeJS** includes another useful **module** called `"fs"` (which is short for File System).  Using it we can read and write files.  Reading and writing files is very slow.  **NodeJS** wants to appear to be very fast and responsive.  It is possible to read and write files both **asynchrounously** and **synchronously**.  As much as possible, we want to read and write files **asynchronously**.

We can write a file **asynchronously** thus:

```javascript
var fs = require("fs");
var fileName = "/desktop/file.txt";
var content = "99 glasses of margaritas on the wall.";
fs.writeFile(fileName, content, function(err) {
    // only when there's an error ...
});
```

We can read a file **asynchronously** thus:

```javascript
var fs = require("fs");
var fileName = "/desktop/file.txt";
fs.readFile(fileName, function(err, data) {
    if (err) {
        // only when there's an error ...
    }
    else {
        var content = data.toString();
        // ...
    }
});
```

_HTTP_ URLs are different than file system paths.  The way you locate a resource via _HTTP_ is not the same as you would on the local file system.  Sometimes we may need to convert a URL path to something equivalent on the local file system.  Suppose we have the following directory structure:

```
/root
    /server
        /web
            index.html
            about.html
            contact.html
            ...
        ...
    ...

```

When the _client_ requests the _HTTP_ path (URL): `http://example.com/about.html` then the _server_ has to somehow map the URL to the actual file.  The built in `path` **package** serves this purpose.

```javascript
var http = require("http");
var path = request("path");
var url = require("url");
var server = http.createServer(function(request, response) {
    var uri = url.parse(request.url).pathname;
    var filename = path.join(__dirname, "./web/" + uri);
    // ...
});
var port = 8080;
server.listen(port);
```

You'll notice the `__dirname` variable above.  It is a built-in global variable that **NodeJS** provides to gives the directory name of the currently executing script (*.js) file.  We can use that as a basis with which to form a valid file system path.  The above example assumes the script is running beneath the `/root/server/` directory listed before.  Any valid file path can be used, such as `../../../<file>` to change to a higher level directory, and so on.


##### Exercises

* When reading a file, why does `data` have to be converted to a string?  Under what circumstances do you NOT want to convert it to a string?
* Create a response that returns an image file when the user browses to it.
* Write code that determines whether a directory exists and creates one if it does not.



## Dictionary

**Asynchronous**: When an event happens in the background without blocking code until the event has completed, it is asynchronous.  This is said to be "non-blocking".

**Event**: Something that happened.  For example, a file opened, a request was received, a user clicked a mouse.

**Event-Driven**: An architecture that is based on responding to **events**.

**Express.js**: A **NodeJS** **package** that makes it very easy to build _HTTP_ servers.

**HTML DOM**: The structure of HTML content in a Web page.

**Module**: A single *.js file within **NodeJS**.

**NodeJS**: A Javascript runtime based on Google **V8** that you can use to build different kinds of applications.

**NPM**: Node Package Manager.  The utility that manages **NodeJS** **packages** by creating, installing, and removing them.

**Package**: A collection of one or more reusable **modules** that a _Node app_ can use to enhance its functionality.

**REST**: REpresentational State Transfer.  A set of guidelines based on a pure interpretation of the _HTTP_ standard to make it easier to build server APIs that are consumable by any client library that understand how to work with _HTTP_.

**Synchronous**: When an event must complete before the next line of code can continue.  This is said to be "blocking".

**V8**: The Javascript interpreter that Chrome and **NodeJS** uses.  It is very fast becasue it compiles Javascript code before executing it.

**Web Brower**: The softare that a user uses to visit different Web pages.  From a programming perspective, it can also be the set of fuctions that make _HTTP_ _requests_ (such as AJAX calls) and _responses_.  While know one would confuse an AJAX call with a Web Browser, its effect is the same.

**Web Server**: the software that handles _HTTP_ _requests_ and _responses_.





