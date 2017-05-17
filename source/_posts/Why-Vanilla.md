---
title: Why Vanilla?
date: 2017-05-17 11:56:47
tags: JavaScript, Introductory Programming
---

![](/stuff/vanilla.jpg)

I have been sharpening my programming skills for 16 years now. I try to stick to the community of programming's curated best-practice content and methods. I only recently (2009-ish) decided that web development is probably the best programming environment to teach my students. The biggest challenge I keep running into is the lack of best-practice teaching resources for ultra-modern vanilla JavaScript. Most resources want to jump right into jQuery or some framework. There are a lot of opinions and websites, but nothing stands out as 'best-practice'. For example, I recently published some JavaScript Ref Cards and posted the link to r/learnjavascript. Aside form the typo that a commenter discovered, they had pointed out some 'mistakes' in my examples. They went on to say that the modern IIFE (immediately invoked function expression - introduced in 2010) is not yet handled by the new ES6 block-scope. From *my* research and JavaScript usage over the years, IIFEs went from this:
```javascript
    (function () {
        // logic here
    })();
```
 To this:
```javascript
    (() => {
        // logic here
    })();
```
To finally [this](http://wesbos.com/es6-block-scope-iife/):
```javascript
    {
        // logic here
    }
```

Very few resources mentioned the last example and still teach the first one. I chose to put the last one on the ref card as an example of IIFEs because it is super easy, and is valid for 95% of beginning programmer's use-cases. As the commenter correctly pointed out, my example is *NOT* valid when used with [module patterns](http://stackoverflow.com/questions/33534485/will-const-and-let-make-the-iife-pattern-unnecessary). While correct, the differences between function scope and the new block scope, hoisting, and module patterns is more of an [advanced topic](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md). The target audience of the content I am providing are beginner-intermediate programmers. So please keep that in mind as you evaluate the materials. My goal is to provide JavaScript training content that teaches ES6 capabilities inline with the standard sequence, selection, & looping material normally taught to new students. I try to take the best programming patterns from other languages like modern Perl and Java 8 to come up with a methodical, best-practice programming style that is modular & cohesive. I am not claiming to be any kind of authority, I'm just someone wanting to give back to the community that helped me. I am always open to suggestions and feedback, but please be kind as my motives are simply to help folks.

### So what is my definition of ultra-modern vanilla JavaScript? At a beginners level:
* Use of ```let``` and ```const``` instead of ```var```
* Using block-scope for IIFEs
* Arrow functions where appropriate
* An appropriate mix of OOP (classes, modules, static, instance, and constructors) and functional programming (IIFE, function expression, pure functions, callbacks, and closures)

### Why vanilla and not a bunch of frameworks?
Frameworks are great for their purposed business use-cases, but I want to know the ecosystem at a fundamental level. Programming is a hobby of mine - something I do because it's fun and cathartic. I understand the reasons for wanting to just jump right in and learn the methodologies that will make an employee the most productive and hire-able, but that is not my path. I want to keep hammering, firing, and honing my *fundamental* programming/language skills so that the code I produce is elegant, readable, maintainable, and interesting. Like a shiny, sharp [Katana](https://en.wikipedia.org/wiki/Japanese_sword).
![](/stuff/katana.jpg)

### What do I provide?
I provide four (4) resources for students:
* [This blog](https://codeelegant.github.io/archives/)
* My JavaScript [Ref Cards](https://github.com/CodeElegant/CheatSheets)
* A [youtube channel](https://www.youtube.com/watch?v=oLys9ev1tfc&t=24s&index=1&list=PLU7QZceI0qqdTctWgF8cD-AH1-vsvg8Jp)
* The [code repository](https://github.com/IntroductoryProgrammingLogic) that has all of the example code

I hope you find all of the content beneficial and if you have questions, concerns, or a better idea, please feel free to drop me a line!

Thanks!

~Howard