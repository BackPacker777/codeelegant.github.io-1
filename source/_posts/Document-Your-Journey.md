---
title: Document Your Journey
date: 2017-03-14 09:22:10
tags:
---

As you navigate your learning it is recommended that you write a web log (blog) record of it all. Why? How about these two guy's reasoning:

* [Dan Luu from Microsoft](https://twitter.com/danluu/status/806223533181468672), [About](https://www.linkedin.com/in/danluu/)
* [Steve Yegge from Google](https://sites.google.com/site/steveyegge2/you-should-write-blogs), [About](https://en.wikipedia.org/wiki/Steve_Yegge)

There is real value in writing your thoughts down and articulating your learnings.

You have a huge array of blogging methods & platforms available to you such as WordPress & Moodle. My recommendation is the following combo:

* [GitHub Pages](https://pages.github.com/) - You already have access to this resource because you have a GitHub account!  
![Github](/stuff/githubpages.jpg)
* [Markdown](https://en.wikipedia.org/wiki/Markdown) - A dead-simple set of extra characters that you add to plain text. The final product is easily converted to web pages (html). [Learn](http://www.markdowntutorial.com/), [Reference](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf).  
![Markdown](/stuff/markdown.png)
* [Hexo](https://hexo.io) - This one was bit of a challenge for me. I had all kinds of trouble setting it up with a vanilla Windows 10 computer. [HERE](https://malekbenz.com/blog/2016/09/10/Create-Host-Blog-for-free-with-Hexo-Github) is a good tutorial but with some caveats (see below). Hexo works because you have already installed Node.js on your computer.
![Hexo](/stuff/hexo.png)

#### Hexo Caveats

I had to update my _config.yml file with the following:
* I downloaded a new theme from Hexo's [theme site](https://hexo.io/themes/), put it in the themes directory and set the **theme:** to its name.
* I changed the public path to:  ```./``` so that my blog would be available from the root url.
* I had trouble getting the server to run, so I ran the ```hexo generate``` command first then the ```hexo server -s``` command.

This should get you up & running! These tasks should be fairly complex if you are new to all of this, but it is a good introduction to workflow for this vocation. Writing Markdown is good place to begin before you start writing code. Please email me your blog address:  **_bates4e@gmail.com_** so I can read it!