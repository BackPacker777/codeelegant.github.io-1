---
title: Beginning With JavaScript
date: 2017-03-15 08:43:17
tags:
---

First things first. I **_HIGHLY_** recommend that you purchase this book: [Learning JavaScript](https://www.amazon.com/Learning-JavaScript-Essentials-Application-Development/dp/1491914912/ref=sr_1_1?s=books&ie=UTF8&qid=1489582493&sr=1-1&keywords=learning+javascript) & its companion site with errata [HERE](http://shop.oreilly.com/product/0636920035534.do) (Clears up a lot of the negative review complaints). Additionally, I will recommend the ["You don't know JS"](https://github.com/getify/You-Dont-Know-JS) free online books as great resources to help you learn JavaScript. I will link to specific chapters for various topics below.

I teach an Introduction to Programming Logic class for a community college as well as for a semester at our high school. I use JavaScript for _this_ class only insofar as to allow the students to test their algorithms and understand the coding process. I don't teach any language features other than what are strictly needed to perform the coding projects, for example the library ```readline-sync``` so they can read input from the command line. This is the order I use for my students, over a 16-week period:

* Code layout - Simply how to format each program file they will turn in for grading. I teach four sections: 1. Comment header, pragmas, & library imports; 2. Global variable declarations & constant declarations/initializations; 3. dispatching section using a main function; 4. All methods - Mutators & workers. Every project my students turn in follow that code layout pattern.
* Syntax and style - How we will format the code and what the language reserved words are.
* Comments - Single-line, multi-line, and JSDoc.
* Variables, constants, & scope - Data type, declaring, & initializing.
* Operators - <, >, ===, !==, <=, >=, arithmetic: ** + - \* / % \*\* **
* Synchronous functions - How to modularize their code into smaller, cohesive chunks. This includes how to use dispatching with a main function.
* Parameters - How to pass & receive values into functions.
* Selection logic - If, else if, else.
* Boolean logic - AND (&&), OR (||) , NOT (!).
* Recursion - How to do it, why to limit it, and tail recursion.
* Looping - While, c-style for, 'for of', nesting loops, break & continue.
* Regular expressions - Primarily used for the next step.
* Input validation, sanitization, & distillation.
* Basic collections - Single & multi-dimensional arrays, maps, & sorting.
* File I/O - How to read & write data to permanent storage.

I have them download & use Node.js to do all their projects so that we aren't dealing with web browsers. It's trivial for my students to use IntelliJ to write their code, then run it directly from the toolbar to see their output! Good stuff. For my web dev classes, we go WAYYY more in depth with actual JavaScript on bot the front & back ends and I will cover that in the next posts.

Here are the list of learning resources I use (You don't have to do every one, but it wouldn't hurt to at least skim what you don't go in depth with):

1. Code layout & syntax/style - I provide this [VIDEO](https://youtu.be/K-RwRTeB8Qs) for my students.
2. [JavaScript Course](https://openclassrooms.com/courses/learn-the-basics-of-javascript)
3. [My video on selection logic](https://youtu.be/6Xp8pkkRt20)
4. [A nice JS Basics course](https://www.udacity.com/course/javascript-basics--ud804)
5. [An alternative take on learning JS](https://watchandcode.com/)
6. Finally, Microsoft's [take on it](https://mva.microsoft.com/en-us/training-courses/javascript-fundamentals-for-absolute-beginners-14194?l=DmF3TY1eB_9500115888), which is actually very good! (I personally loathe jQuery, so I can't recommend those parts at the end.)
7. Regular expressions - For your library, I would recommend these two books: [_Mastering Regular Expressions_](https://www.amazon.com/Mastering-Regular-Expressions-Jeffrey-Friedl/dp/0596528124) + [Errata](http://shop.oreilly.com/product/9780596528126.do) & [_Regular Expressions Cookbook_](https://www.amazon.com/Regular-Expressions-Cookbook-Solutions-Programming/dp/1449319432) + [Errata](http://shop.oreilly.com/product/0636920023630.do). [THIS](https://code.tutsplus.com/tutorials/you-dont-know-anything-about-regular-expressions-a-complete-guide--net-7869) is a great tutorial, [THIS](https://regex101.com/) is a great regex tester, & [THIS](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/) is a great cheat sheet.
8. File I/O [demo code](/stuff/fileIO.txt). Also notice the input validation.
9. Remember to utilize the [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript).
10. If you get bored, try [Codecademy!](https://www.codecademy.com/courses/learn-javascript)

That is a TREMENDOUS amount of learning material to get you up & coding with JavaScript. There is probably **200** hours of stuff on this page. Sorry about that!