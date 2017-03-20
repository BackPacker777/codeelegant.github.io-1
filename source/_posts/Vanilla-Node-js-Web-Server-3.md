---
title: Vanilla Node.js Web Server - 3
date: 2017-03-20 06:54:45
tags: node.js, web server, es6, http, mime
categories: Crafting vanilla Node.js web server
---

After completing [lesson 1](https://codeelegant.github.io/2017/03/16/Vanilla-Node-js-Webserver/), & [lesson 2](https://codeelegant.github.io/2017/03/17/Vanilla-Node-js-Webserver-2/), you have a web server that can serve static web pages on par with the very _excellent_ [http server](https://www.npmjs.com/package/http-server). WHAT!?!? You could have just used a library and had a canned web server without having to write all that code? Yes, it's true. That's not really why you're here though, is it? You are here to learn how to write code & to understand JavaScript & Node.js better.  

In this, the last lesson in the 'Vanilla Node.js web server' series, we are going to tackle more dynamic functionality: Databases with [NeDB](https://github.com/louischatriot/nedb), csv files, and html templating with [EJS](http://www.embeddedjs.com/).  
![](/stuff/nedb_ejs.png)  
Why would you want to read/write data to a server? File I/O on the server using **[Ajax](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)** or the new **[fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)** is the primary way (the _only_ way I know of...) to send data from the DOM to the server to permanently store the data. Then you can use your server-side JavaScript to do what you normally do with data: Query it, modify it, print it, sell it to spammers, etc. (please don't do that last one!).

Let's begin by making two (2) new directories in the root of our project called data & node. We'll keep our data files in the data folder and any of our Node.js scripts in the node folder - I know, 'duh'....! When we are finished with this lesson we'll have a file named **people.csv** and a file named **games.json** in our data folder. In our node folder we'll have a file named **Data_Handler.js**.  
![Like this](/stuff/capture3.png)
