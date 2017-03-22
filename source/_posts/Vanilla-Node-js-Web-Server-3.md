---
title: Vanilla Node.js Web Server - 3
date: 2017-03-20 06:54:45
tags: node.js, web server, es6, http, mime
categories: Crafting vanilla Node.js web server
---
## ! ! THIS POST IS NOT COMPLETE ! !
I will be finishing during spring break 2017  

#### JavaScript Templating & File I/O
After completing [lesson 1](https://codeelegant.github.io/2017/03/16/Vanilla-Node-js-Webserver/), & [lesson 2](https://codeelegant.github.io/2017/03/17/Vanilla-Node-js-Webserver-2/), you have a web server that can serve static web pages on par with the very _excellent_ [http server](https://www.npmjs.com/package/http-server). WHAT!?!? You could have just used a library and had a canned web server without having to write all that code? Yes, it's true. That's not really why you're here though, is it? You are here to learn how to write code & to understand JavaScript & Node.js better.  

In this, the last lesson in the 'Vanilla Node.js web server' series, we are going to tackle more dynamic functionality: Databases with [NeDB](https://github.com/louischatriot/nedb), csv files, and html templating with [EJS](http://www.embeddedjs.com/).  
![](/stuff/nedb_ejs.png)  
Why would you want to read/write data to a server? File I/O on the server using **[Ajax](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)** or the new **[fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)** API is the primary way (the _only_ way I know of...) to send data from the DOM to the server to permanently store the data. Then you can use your server-side JavaScript to do what you normally do with data: Query it, modify it, print it, sell it to spammers, etc. (please don't do that last one!).

Let's begin by making two (2) new directories in the root of our project called data & node. We'll keep our data files in the data folder and any of our Node.js scripts in the node folder - I know, 'duh'....! When we are finished with this lesson we'll have a file named **people.csv** and a file named **games.json** in our data folder. In our node folder we'll have a file named **Data_Handler.js**.  
![Like this](/stuff/capture3.png)

In the views folder we'll want to have some ejs files, in the css folder throw in some styling, and in the javascripts folder, chuck in some client-side form handling capability. We'll also have some client-side Ajax code in there so we can send the data over to the server code for processing. Feel free to look at my source code over on my [GitHub Repository](https://github.com/CodeElegant/NodeWebServerLesson).

We will need to update our **package.json** file so that npm can install some library dependencies that we'll need for our project. Add dependencies for **ejs**, **formidable**, & **nedb**. Your **package.json** should look like this:  
```javascript  
{
  "name": "nodewebserverlesson",
  "version": "0.0.1",
  "description": "Demo Code",
  "main": "bin/server",
  "scripts": {
    "start": "node bin/server"
  },
  "author": "Howard Bates",
  "license": "MIT",
  "dependencies": {
    "ejs": "latest",
    "formidable": "latest",
    "nedb": "latest"
  }
}
```

Check out that sweet, sweet JSON goodness with all the quotes, commas, and curly braces. Man, programming is weird. It must be universal in the galaxy though if that guy in _Independence Day_ was able to upload a virus to the alien mothership. ;-P  (Yes, I know it's explained [HERE](https://www.yahoo.com/movies/independence-day-producer-explains-hacking-scene-104676447332.html).)
![](/stuff/virus.gif)  

Run the **npm install** command in your terminal. A folder called node_modules will be created and all the libraries/dependencies will be installed into it. The node_modules directory has a figgin' TON of files in it, wayyy more than we'd want to push/pull from GitHub. What's a Codesmith to do to stop this? Guess what, all you have to do is have the **.ignore** IntelliJ plugin installed and create a file in the root of your project named **.gitignore**. Each line of this file will contain all the stuff you don't want **git** to give a heck about. Just put **node_modules** in the top of your **.gitignore** file and viola!

First up with the extensions we want to add to our web app is Embedded JavaScript (EJS), which is a JavaScript templating engine. Why would you want to use a template engine anyway? See **[this article](https://www.smashingmagazine.com/2012/12/client-side-templating/)**, and watch **[this video](https://css-tricks.com/video-screencasts/127-basics-of-javascript-templating/)**. I won't be showing you how to use the templating logical constructs in your HTML, just how to use templating for bolting on extra pages of data such as a header, footer, and pages of divs for hiding & showing on demand.  
At the top of the **loadServer()** method in your **app.js** file, create a constant that imports the **EJS** library like this:

```javascript
const EJS = require('ejs');
```
Change the way we handle some content types like this:
```javascript
else if (contentType.indexOf('css') >= 0 || contentType.indexOf('js') >= 0) {
	 response.writeHead(200, {'Content-Type': contentType});
	 response.end(string, 'utf-8');
} else if (contentType.indexOf('html') >= 0) {
	 console.log(`Rendering EJS`);
	 response.writeHead(200, {'Content-Type': contentType});
	 response.end(EJS.render(string, {
		  data: this.ejsData,
		  filename: 'index.ejs'
	 }));
}
```

Finally, change your root route to this:

```javascript
else if (request.url.indexOf('/') >= 0) {
	this.render('public/views/index.ejs', 'text/html', httpHandler, 'utf-8');
}
```

Your entire **app.js** should look like this:  
```javascript  
// todo:

"use strict";

class app {
     constructor() {
          this.ejsData = null;
          this.user = null;
          this.loadServer();
     }

     loadServer() {
          const HTTP = require('http');
          const PORT = 8005;
          const EJS = require('ejs');

          HTTP.createServer((request, response) => {

               let httpHandler = (error, string, contentType) => {
                    if (error) {
                         response.writeHead(500, {'Content-Type': 'text/plain'});
                         response.end('An error has occurred: ' + error.message);
                    } else if (contentType.indexOf('css') >= 0 || contentType.indexOf('js') >= 0) {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(string, 'utf-8');
                    } else if (contentType.indexOf('html') >= 0) {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(EJS.render(string, {
                              data: this.ejsData,
                              filename: 'index.ejs'
                         }));
                    } else {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(string, 'binary');
                    }
               };

               if (request.url.indexOf('.css') >= 0) {
                    this.render(request.url.slice(1), 'text/css', httpHandler, 'utf-8');
               } else if (request.url.indexOf('.js') >= 0) {
                    this.render(request.url.slice(1), 'application/javascript', httpHandler, 'utf-8');
               } else if (request.url.indexOf('.png') >= 0) {
                    this.render(request.url.slice(1), 'image/png', httpHandler, 'binary');
               } else if (request.url.indexOf('/') >= 0) {
                    this.render('public/views/index.ejs', 'text/html', httpHandler, 'utf-8');
               } else {
                    this.render(`HEY! What you're looking for: It's not here!`, 'text/html', httpHandler, 'utf-8');
               }

          }).listen(PORT);
     }

     render(path, contentType, callback, encoding) {
          const FS = require('fs');
          FS.readFile(path, encoding ? encoding : 'utf-8', (error, string) => {
               callback(error, string, contentType);
          });
     }
}

module.exports = app;
```

What does this extra code do? We really only added a library call, changed the name of the file we render from index.html to index.ejs, and called the **render** static method from the **ejs** library, which 'turns on' EJS capability. Read about it [HERE](https://www.npmjs.com/package/ejs).

To do the page-bolt on technique I like so much, I simply created seperate header.ejs & footer.ejs files with the top & bottom parts of a normal html page. I then use this code in my index.ejs file where I want their contents to be displayed. (Remember to look at the GitHub repositry code). The line looks loke this:

```html
<% include public/views/header.ejs %>
```

To learn more about EJS, watch [THIS](https://www.youtube.com/watch?v=BmmJujNQvXw) video and read the documentation [HERE](http://www.embeddedjs.com/getting_started.html). No, **_REALLY_** do it! I chose EJS to teach with because, although venerable, it is VERY easy to grok. It will make it much easier for you to jump into React, Vue, or Angular by learning this trivial templating engine.

##### Exercise time!
>Create a seperate html page with _only_ a div of content. Use EJS tags in the index.ejs file to include the page you just made.

#### On to file I/O

Now that we have some folders, let's modify our **app.js** file to handle POST requests. What the heck is a POST request? Well, if you look at the [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) you'll notice that there are a whole bunch of http request types. You will probably only work with GET & POST 99.999% of the time. GET to get stuff from the server, and PUT to send stuff to the server, like filled-in form data.  
I am going to cover two methods of file I/O. Reading/writing to .csv files using c-style for loops, and reading/writing to databases using NeDB.

We need to modify our **app.js** file to look like this:

```javascript
// todo:

"use strict";

const DATA_HANDLER = require('./node/DataHandler');

class app {
     constructor() {
          this.ejsData = null;
          this.user = null;
          this.loadServer();
     }

     loadServer() {
          const HTTP = require('http');
          const PORT = 8005;
          const EJS = require('ejs');

          HTTP.createServer((request, response) => {

               let httpHandler = (error, string, contentType) => {
                    if (error) {
                         response.writeHead(500, {'Content-Type': 'text/plain'});
                         response.end('An error has occurred: ' + error.message);
                    } else if (contentType.indexOf('css') >= 0 || contentType.indexOf('js') >= 0) {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(string, 'utf-8');
                    } else if (contentType.indexOf('html') >= 0) {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(EJS.render(string, {
                              data: this.ejsData,
                              filename: 'index.ejs'
                         }));
                    } else {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(string, 'binary');
                    }
               };

               if (request.method === 'POST') {
                    if (request.headers['x-requested-with'] === 'XMLHttpRequest0') {
                         request.on('data', (data) => {
                              this.user = DATA_HANDLER.handleUserData(data.toString('utf8'));
                              if (this.user !== 'false') {
                                   response.writeHead(200, {'content-type': 'application/json'});
                                   response.end(this.user);
                              } else {
                                   response.writeHead(200, {'content-type': 'text/plain'});
                                   response.end('false');
                              }
                         });
                    } else {
                         response.writeHead(405, "Method not supported", {'Content-Type': 'text/html'});
                         response.end('<html><head><title>405 - Method not supported</title></head><body><h1>Method not supported.</h1></body></html>');
                    }
               } else if (request.url.indexOf('.css') >= 0) {
                    this.render(request.url.slice(1), 'text/css', httpHandler, 'utf-8');
               } else if (request.url.indexOf('.js') >= 0) {
                    this.render(request.url.slice(1), 'application/javascript', httpHandler, 'utf-8');
               } else if (request.url.indexOf('.png') >= 0) {
                    this.render(request.url.slice(1), 'image/png', httpHandler, 'binary');
               } else if (request.url.indexOf('/') >= 0) {
                    this.render('public/views/index.ejs', 'text/html', httpHandler, 'utf-8');
               } else {
                    this.render(`HEY! What you're looking for: It's not here!`, 'text/html', httpHandler, 'utf-8');
               }

          }).listen(PORT);
     }

     render(path, contentType, callback, encoding) {
          const FS = require('fs');
          FS.readFile(path, encoding ? encoding : 'utf-8', (error, string) => {
               callback(error, string, contentType);
          });
     }
}

module.exports = app;
```
Please note that we've added a library import for our DataHandler.js class we will write. We also now have a POST handler section. We will use the client-side JavaScript to send the appropriate **request.headers** info so our app knows how to handle the client requests.

Now, write a separate class to handle the actual file I/0. Something like this:
```javascript
//   todo:

"use strict";

const FS = require('fs');

class DataHandler {

     static handleUserData(data) {
          data = JSON.parse(data);
          const FILE_PATH = 'data/users.csv';
          let users = FS.readFileSync(FILE_PATH, 'utf8');
          let user = {};
          const COLUMNS = 4;
          let tempArray, finalData = [];
          tempArray = users.split(/\r?\n/); //remove newlines
          for (let i = 0; i < tempArray.length; i++) {
               finalData[i] = tempArray[i].split(/,/).slice(0, COLUMNS);
          }
          for (let i = 0; i < finalData.length; i++) {
               if (data === finalData[i][0]) {
                    user = JSON.stringify({
                         'email': finalData[i][0],
                         'position': finalData[i][1],
                         'lastName': finalData[i][2],
                         'firstName': finalData[i][3]
                    });
                    break;
               } else {
                    user = 'false';
               }
          }
          return user;
     }
}

module.exports = DataHandler;
```
You should see **[JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)** & **[JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)** in the above code. If you peruse [lesson 1](https://codeelegant.github.io/2017/03/16/Vanilla-Node-js-Webserver/), you'll remember that all communication between the client DOM & the server has to be string. Stringify takes object data & turns it into a string to send, and parse unpacks the string back into its object form for use by the code. [Here](http://softwareengineering.stackexchange.com/questions/164094/why-does-javascript-use-json-stringify-instead-of-json-serialize) is a very good (but turbo-nerdy) explanation. All I am doing in the **DataHandler.js** class is accepting a user's email address, loading a user file from the hard drive into a multi-dimensional array, and comparing the email address submitted to finalData[i][0] column in the MD array loaded from the .csv file. If I find one, I return the entirety of the user data from the MD array to the DOM. If I don't have a match, I alert a message in the DOM stating they need to provide a valid email address. Easy-peasy, lemon-squeezy.  

I feel like we need an intermission here. How about a [fantastic documentary](https://www.youtube.com/watch?v=wbM2_-JeDuY) (Beware: some strong language in parts) about the Pink Floyd album _Wish you were here_? Yeah, I thought you'd like that.  

Almost done with the csv part.

here are the contents of my **users.csv** file in my data folder:

```
bates4e@gmail.com,3rd Base,Bates,Howard
meow@kitty.com,Left Field,Kitty,Hello
```

The .csv file is the source of the hard drive data that we are iterating over to see if a user exists.

Finally, we have to have some client-side Ajax code so the DOM can yak at the server properly. Mine looks like this:

```javascript
performAjax(requestNum, sendToNode, callback) {
	let bustCache = '?' + new Date().getTime();
	const XHR = new XMLHttpRequest();
	XHR.open('POST', document.url + bustCache, true);
	XHR.setRequestHeader('X-Requested-with', requestNum);
	XHR.send(sendToNode);
	XHR.onload = () => {
		if (XHR.readyState == 4 && XHR.status == 200 && callback) {
			return callback(XHR.responseText);
		}
	};
}
```

The magic happens with the **[XMLHttpRequest()](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)**. _THIS_ is Ajax. Or Francis (just kidding).  
![](/stuff/deadpool.png)
Remember, to see the whole code, look at the **main.js** file in the [GitHub repository](https://github.com/CodeElegant/NodeWebServerLesson).  

The Ajax code allows us to open a POST request to the server, set the **request.headers**, send the data, then handle the response back from the server with the **XHR.onload** thingy. The **app.js** file receives & handles the request with the ```javascript if (request.method === 'POST') {} ```section.  

It feels to me like I've gotten us into a 'How to draw an owl' situation....  
![](/stuff/drawowl.jpg)

Please make sure you REALLY understand how the Ajax method talks to the server, how the server responds, and how the Ajax method handles the response. Also, make sure you grok **stringify** & **parse**, why we need them, & how to use them. Use these resources:

1. [Vanilla Ajax](https://www.sitepoint.com/guide-vanilla-ajax-without-jquery/)
2. [Ajax guide on MDN](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started)
3. A [nice video](https://www.youtube.com/watch?v=qqRiDlm-SnY)
4. TutorialsPoint [tutorial](http://www.tutorialspoint.com/ajax/index.htm)
5. [Ajax Reference Sheet](http://help.dottoro.com/ljspgwvf.php)

You go through all of THAT material and you will be the **Sensei of Ajax** (feel free to put that on your résumé. Tell 'em I said so.)

###### NeDB
There is a lot of discussion about which database you should use for a web app. See [here](https://blog.daftcode.pl/hype-driven-development-3469fc2e9b22#.ba7omm7fq) & [here](https://blog.qmo.io/common-problems-when-developing-a-node-js-web-application/) for discussions. SQL or NoSQL. Meh, for our purposes it doesn't matter, but for production it does matter, so get frosty on which database you'll need.