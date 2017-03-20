---
title: Vanilla Node.js Web Server - 2
date: 2017-03-17 05:52:12
tags: node.js, web server, es6, http, mime
categories: Crafting vanilla Node.js web server
---

In the [1st lesson](https://codeelegant.github.io/2017/03/16/Vanilla-Node-js-Webserver/), we made a very basic web server. In fact all it did was respond to a client request with a simple string of text. When you think about it, that's still pretty neat because the client could have been anywhere in the world and transacted with the web server: The one **YOU** wrote, with your hand fingers!  
As awesome as that is, we _probably_ want a little more functionality. In this lesson, we'll add some routes. That means when a client requests a specific resource from our server like a page a photos of our dog, we can respond with the correct data. We'll also add the ability to handle mime types whereby, if the client requests a non-plain text item like a photo, we can tell the browser how to display it to the user. If we didn't, the browser would display EVERYONES favorite picture:  
![](/stuff/noimage.png)  
Right now, your web server code should look like this:  
```javascript
//  todo:

"use strict";

class app {
     constructor() {
          this.loadServer();
     }

     loadServer() {
          const HTTP = require('http');
          const PORT = 8000;

          HTTP.createServer((request, response) => {
               response.writeHead(200, {'Content-Type': 'text/plain'});
               response.write(`Look at my website!`);
               response.end();
          }).listen(PORT);
     }
}

module.exports = app;
```

To make our web presence all fancy, we'll use some CSS to style it and some pictures to snazz it up. Let's tell our server to ask questions about the client request:
  
```javascript
// todo:

"use strict";

class app {
	constructor() {
		this.loadServer();
	}
	
	loadServer() {
		const HTTP = require('http');
		const PORT = 8000;
		
		HTTP.createServer((request, response) => {
						
			if (request.url.indexOf('.css') >= 0) {

			} else if (request.url.indexOf('.js') >= 0) {

			} else if (request.url.indexOf('.html') >= 0) {

			} else if (request.url.indexOf('.png') >= 0) {

			} else {

			}
			
		}).listen(PORT);
	}
}

module.exports = app;
```

Notice that the guts of the loadServer() function's logic has changed. Some folks prefer to use a **[switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)** statement instead of cascading if statements. Whatever floats your boat. I like my way. The magic of what's happening here are the **request.url.indexOf** statements. If we have a look-see at the 'ol JavaScript API for [indexOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf) we see that this technique is a way to search a text string for the occurrence of what we're looking for. In the case of the first **if** test, we're looking for the character sequence **.css** in the request **url**. Hmmmm, any idea what files may have the extension .css? Yes! Our css stylesheets! We have to put some commands inside the if blocks to handle the request once we've decided what it is. I like this technique:  
```javascript  
// todo:

"use strict";

class app {
	constructor() {
		this.loadServer();
	}
	
	loadServer() {
		const HTTP = require('http');
		const PORT = 8000;
		
		HTTP.createServer((request, response) => {
		
			let httpHandler = (error, string, contentType) => {
				if (error) {
					 response.writeHead(500, {'Content-Type': 'text/plain'});
					 response.end('An error has occurred: ' + error.message);
				} else if (contentType.indexOf('css') >= 0 || contentType.indexOf('html') >= 0 || contentType.indexOf('js') >= 0) {
					response.writeHead(200, {'Content-Type': contentType});
					response.end(string, 'utf-8');
				} else {
					response.writeHead(200, {'Content-Type': contentType});
					response.end(string, 'binary');
				}
			};
						
			if (request.url.indexOf('.css') >= 0) {

			} else if (request.url.indexOf('.js') >= 0) {

			} else if (request.url.indexOf('.html') >= 0) {

			} else if (request.url.indexOf('.png') >= 0) {

			} else {

			}
			
		}).listen(PORT);
	}
}

module.exports = app;
```  
The first step is to create a **function expression** that accepts an error, the string content from the render function (we haven't made this yet), and the content type of the request from the client. Hold up. You're probably wondering how we are going to send our _images_ as string. Great question! First, see [this answer](http://stackoverflow.com/a/1892044/466246) on why we don't use binary. Here is what your pictures look like before we tell the browser to translate them to binary:  
![Pic of Grandma!](/stuff/stringbinary.png)
Awww, grandma...!  

This is the reason we have to handle (encode) the mime types and tell the browser to render the response from our web server as either **binary** for all non-plain text data, and **utf-8** for all plain text data. What's **utf-8**? see [THIS](http://stackoverflow.com/a/33358306/466246).  
Ok, now let's add the ability to read the data off of the hard drive of your server so you can give it to the client.  
```javascript  
// todo:

"use strict";

class app {
	constructor() {
		this.loadServer();
	}
	
	loadServer() {
		const HTTP = require('http');
		const PORT = 8000;
		
		HTTP.createServer((request, response) => {
		
			let httpHandler = (error, string, contentType) => {
				if (error) {
					 response.writeHead(500, {'Content-Type': 'text/plain'});
					 response.end('An error has occurred: ' + error.message);
				} else if (contentType.indexOf('css') >= 0 || contentType.indexOf('html') >= 0 || contentType.indexOf('js') >= 0) {
					response.writeHead(200, {'Content-Type': contentType});
					response.end(string, 'utf-8');
				} else {
					response.writeHead(200, {'Content-Type': contentType});
					response.end(string, 'binary');
				}
			};
						
			if (request.url.indexOf('.css') >= 0) {

			} else if (request.url.indexOf('.js') >= 0) {

			} else if (request.url.indexOf('.html') >= 0) {

			} else if (request.url.indexOf('.png') >= 0) {

			} else {

			}
			
		}).listen(PORT);
		
		render(path, contentType, callback, encoding) {
			const FS = require('fs');
			FS.readFile(path, encoding ? encoding : 'utf-8', (error, string) => {
				callback(error, string, contentType);
			});
		}
	}
}

module.exports = app;
```  
You'll note that there is an instance method at the bottom of our code that's responsible for actually _reading_ the data off of the server hard drive by using the Node.js **[fs](https://nodejs.org/api/fs.html)** library and, specifically, its **[readFile()](https://nodejs.org/api/fs.html#fs_fs_readfile_file_options_callback)** method. The render function takes in a pad-ton of parameters: The path on our hard drive of the client request, the mime content type requested, a callback function (it will the the **httpHandler** function expression), and the encoding (**binary**, or **utf-8**) we'll tell the client browser. The **readFile()** function's parameters are a little more complicated. The first parameter is the path from above, then the next parameter is a **[ternary](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)** that says "if no encoding is passed to me, use **utf-8** as my default", and finally, we use an anonymous arrow callback function per the **fs.readFile()** spec. The callback allows the server to read the files from the hard drive asynchronously and give the results to our callback (**httpHandler**) that was passed in as a parameter to the render function.  
Now we're ready to finally fill in those if blocks with some action!  
```javascript  
// todo:

"use strict";

class app {
	constructor() {
		this.loadServer();
	}
	
	loadServer() {
		const HTTP = require('http');
		const PORT = 8000;
		
		HTTP.createServer((request, response) => {

               let httpHandler = (error, string, contentType) => {
                    if (error) {
                         response.writeHead(500, {'Content-Type': 'text/plain'});
                         response.end('An error has occurred: ' + error.message);
                    } else if (contentType.indexOf('css') >= 0 || contentType.indexOf('html') >= 0 || contentType.indexOf('js') >= 0) {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(string, 'utf-8');
                    } else {
                         response.writeHead(200, {'Content-Type': contentType});
                         response.end(string, 'binary');
                    }
               };
						
			if (request.url.indexOf('.css') >= 0) {
				this.render(request.url.slice(1), 'text/css', httpHandler, 'utf-8');
			} else if (request.url.indexOf('.js') >= 0) {
				this.render(request.url.slice(1), 'application/javascript', httpHandler, 'utf-8');
			} else if (request.url.indexOf('/') >= 0) {
				this.render('public/views/index.html', 'text/html', httpHandler, 'utf-8');
			} else if (request.url.indexOf('.png') >= 0) {
				this.render(request.url.slice(1), 'image/png', httpHandler, 'binary');
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
Alrighty then! This code simply calls the **render()** method and uses the JavaScript **[String.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)** function to send the path & filename requested from the client browser (index.html, etc.) as the first parameter, then it passes **render()** the mime content type, the callback function (httpHandler, finally!), and the encoding we need to used based on the file requested. That's it! You now have a fully operational battle station, er, web server! (Sorry, my inner Star Wars was coming through there).  
We still have one more step to do before we can respond to requests. You have NO content on the hard drive to serve up! Please create a **css**, **html**, & **js** file and put them in your web server app root directory. NOW you can fire it up and test it out!
If it works:
![Great Job!](/stuff/awesome.jpg)

If it does NOT work:
![You're trying hard!](/stuff/betterjob.jpg)

Heh, just kidding! Try hard to figure out what is broken. Remember to use **console.log()** to diagnose problems. Utilize the Chrome developer tools, and the Node.js output in the terminal window. 

Exercise, yay!
>Add support for jpeg images and a web page called about.html.

Time for a philosophical discussion. By now you've heard of the terms _[express.js](https://expressjs.com/)_, or _[koa.js](http://koajs.com/)_, or other Frameworks. I'm sure there is a ratio of 1 billion:1 of people in favor of frameworks vs. coding it out in Node.js by hand. I'm not one of them. Look, I get that there are libraries out there to abstract away all of the hard stuff, but I'd rather get really good at the hard stuff. For example, I am on the Ski Patrol, and we have patrollers that swear by the latest and greatest skis, poles, boots, gloves, helmets, etc. But we also have patrollers that can ski in the glades on a pair of 2x4s duct-taped to their feet and look good doing it. They can do this because they are _Masters_ of skiing, not the equipment. Please consider coding for the enjoyment of it and to move from apprentice to master. By all means, as you do A LOT more of this, start to add libraries like **[url](https://nodejs.org/docs/latest/api/url.html)** & **[httpdispatcher](https://www.npmjs.com/package/httpdispatcher)** to make your life easier, but I encourage you to master vanilla Node.js first.  

Well, that's it for now. In the [next & and final installment](https://codeelegant.github.io/2017/03/20/Vanilla-Node-js-Web-Server-3/), we'll cover the [EJS](http://www.embeddedjs.com/) templating engine, and data handling with databases ([NeDB](http://stackabuse.com/nedb-a-lightweight-javascript-database/)) & csv files.