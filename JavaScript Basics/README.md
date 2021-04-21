# JavaScript Basics

Learn JavaScript, the high-level, multi-paradigm language of the web.

[JavaScript Basics](https://tryhackme.com/room/javascriptbasics)

## Topic's

* JavaScript Fundamentals
* Cross-Site Scripting

## Task 1 Intro to JavaScript

**JavaScript Basics**

![](https://cdn.uconnectlabs.com/wp-content/uploads/sites/25/2020/04/J.png)

During this room, we will be covering the basics of the programming language JavaScript.

The main purpose of this language is to implement interactiveness into webpages and web applications, but thanks to the millions of individuals in the community, we've been able to control more than just the interactiveness of web pages.
Thanks to the community, we can now use JavaScript to control servers, create video games, code mobile applications, and can even be used for cybersecurity in some scenarios.

Here are a few things we will be covering during this room:

* Variables
* Conditionals
* Functions
* Objects
* Loops
* DOM (Document Object Model)
* XSS (Cross-Site Scripting)

Here is what you'll need before beginning this room:

* IDE or Text Editor of your choice (I recommend VS Code or Atom)

One final note. This room will not cover everything JavaScript can do, not even close. This room will be targeted towards beginners who are looking to learn basic programming logic and an easy to use, versatile language for obtaining a better understanding of how the web works. Remember to take your time and read carefully. Enjoy the room and have fun learning about JavaScript!

1. Let's Begin!

`No answer needed`

## Task 2 Variables & Operators

Before diving into JavaScript, here are a few things to remember as we traverse through the room:

1. Using "//" within your IDE or text editor are considered comments, use this to take notes directly in your JS file. Always take notes!
2. We will be using Chrome dev tools for this room, so for this, we'll need to link our JavaScript file to an HTML file. We will not be covering much HTML in this room, but you can easily link your JavaScript file by using a Script element. You can see an example here:

```html
<body>

  <script src="script.js"></script>

</body>
```

Remember to put the script tag right before the closing body tag. This way our HTML code will execute before our external JavaScript document. There are some instances where it's better to have the script in the head tag or a certain order when working with multiple documents, but for this room, we will only be focusing on the basics. You can see an example of how this is used in an HTML document [here](https://raw.githubusercontent.com/GhostlyPy/javascriptbasics/master/script_tag.png).

**Variables**

There are 3 types of variables in JavaScript: var, let, and const.

The var (variable) keyword was originally the only variable available, but thanks to the upgrade to ECMAScript 6 back in 2015, which is the specification that JavaScript conforms too, we now have multiple ways of declaring a variable or data types.

**Quick Overview:**

* let: If a variable is going to be reassigned later within the application, this is the ideal variable type to use.
* var: It's better to use either let or const for variables, but this variable type will still work and is still used in applications to this day. This variable can be updated and re-declared.
* const: If the variable will never change or won't be reassigned anywhere else in the application, this keyword is the best option.

**Good things to remember:**

* The var variable is globally scoped and can be updated and re-declared.
* The let variable is block-scoped and can be updated but not re-declared.
* The const variable is block-scoped and cannot be updated or re-declared.

Global Scope: A variable declared outside a function. This means all scripts and functions on a web application or webpage can access this variable.

Block Scope: A variable declared inside a block. This means we can use these variables inside of loops, if statements, or other declarations within curly brackets and have them be only used for that declaration instead of the entire application having access to it.

**Examples:**

```js
var variableOne = 'Linus Torvalds';

let variableTwo = 50;

const variableThree = 'Creator of the Linux Kernel';
```

Variables are very easy to use and understand in JavaScript, they can store a lot of information with very little code.

Main data types that you can store within a variable:

| Data Type | Example |
|---|---|
| Number | 200 |
| String | 'Neo' |
| Boolean | True or False |

There are also arrays, objects, and many many more data types that we will touch upon during this room. Variables are wonderful, but they aren't very powerful on their own, this is where arithmetic operators, comparison operators, and boolean operators come into play. It may sound complicated, but these tables should make everything a little easier to understand!
 
Arithmetic Operators
| Operator | Type | What It Does | Example |
|---|---|---|---|
| + | Addition | Adds numbers or strings together | 25 + 5 = 30 |
| ++ | Increment | Increases the variable's number by 1 | let x = 20; x++; x = 21 |
| - | Subtraction | Subtracts the variable's numbers | 15 - 5 = 10 |
| -- | Decrement | Decreases the variable's number by 1 | let x = 20; x--; x = 19 |
| * | Multiplication | Multiplies one number by another | 5 * 10 = 50 |
| / | Division | Divides one number by another | 100 / 10 = 10 |
| % | Modulus | Returns remainder of divided operation | 100 % 8 = 4 |

Comparison Operators

| Operator | What It Does | Example |
|---|---|---|
| == | Equal to | 100 == 100 |
| === | Equal to & identical | 500 === 500 |
| != | Not equal to | 100 != 50 |
| !== | Not identical | 35 !== 75 |
| < | Less than | 5 < 85 |
| <= | Less than or equal to | 60 <= 90 |
| > | Greater than | 30 > 5 |
| >= | Greater than or equal to | 1,000 >= |

These two tables cover the majority of what we will be going over moving forward, but there are still so many more things that we won't be able to cover. Once you finish learning about the basics of JavaScript, I recommend researching more about operators as there are still boolean operators (true or false) and logical operators (AND, OR, NOT).

Let's move on to our final lesson for variables: extra data types. It's time to see the available types of data that we can store within our variables. This will cover some of the basics you'll be using for this room.

* Strings: 'Morpheus'
* Arrays: [1, 2, 3]
* Objects: {Name: 'John', Occupation: 'Master Hacker'}
* Booleans: true (or false)
* Numbers: 455
* Floating-Point Numbers: 10.5

There may be a lot here, but you'll notice as we progress through the room, it'll all be worth it and valuable information for later.

1. What type of data type is this: 'Neo'?

`string`

2. What data type is true/false?

`boolean`

3. What is John's occupation?

`Master Hacker`

4. What tag is used for linking a JavaScript file to HTML?

`script`

## Task 3 Conditionals

Conditionals

Welcome to Conditionals!

Example:

```js
if (5 === 5) {

    console.log('Hello World!'); // Prints Hello World! to the console

};
```

In plain English, if a condition is met, then the code will run. Remember all those operators we learned about in the first task? We can use those to define these conditions and plan our approach.

There are also else if statements, which look like this:

```js
if (5 === 10) {

    console.log('Hello World!'); // Skips this code

} else if (10 === 10) {

    console.log('Hello World!'); // Prints Hello World! to the console

};
```

This way we can have multiple conditions before our code executes. Normally, we'd end our code with an else statement like this:

```js
if (5 === 10) {

    console.log('Hello World!'); // Skips this code

} else if (10 === 10) {

    console.log('Hello World!'); // Prints Hello World! to the console

} else {

    console.log('ERROR ERROR ERROR');

};
```

Basically, the else keyword concludes our if conditional. We'll learn a better way of writing conditionals later, but for now, this is a clear way of using a condition to return a result.

**Switch Cases**

If you need to test multiple conditions, then most of the time switch cases are best for optimization and readability within your code. If, else if, else statements and switch cases can both do similar tasks, but switch cases are better for performing multiple different conditions. Here's how a switch case looks:

```js
const animal = 3;

switch (animal) {

    case 1:

        document.write('Cow');

        break;

    case 2:

        document.write('Chicken');

        break;

    case 2:

        document.write('Monkey');

        break:

    default:

        document.write('Animal?');

} // Outputs Monkey
```

Cool, huh? We use each case to define a number, have our variable hold the number we're pulling from, and the break keyword is used for ending that specific case or switch block. The default keyword is there just in case our variable is equal to something that isn't present in our switch...case code.

![](https://raw.githubusercontent.com/GhostlyPy/javascriptbasics/master/funny-if.png)

1. Congratulations! You can now write conditionals!

`No answer needed`

## Task 4 Functions

**Functions**

Functions are one of the most vital parts of programming. Due to how important functions are, there will be a lot of things we don't cover, but remember that functions are going to be vital to any applications you write.

*ECMAScript 6 is the most popular version of JavaScript. There are a lot of important differences between ES5 and ES6, we focus mainly on ES6 in this room, but knowing past versions can help improve your knowledge of the language and logic of JavaScript.

This is a function in **ES6** (ECMAScript 6):

```js

const func = (a, b) => {

    let nums = a * b;

    console.log(nums); // Outputs 250

}

func(25, 10);

ES5:

function func(a, b) // Everything inside of the parenthesis defines our parameter(s)

{

    let nums = a * b;

    console.log(nums); // Outputs 250

}

func(25, 10);
```

A few things that make up a function are the parameter(s), variables, and our executed code. Functions can contain if conditionals, defined variables, multiple parameters, and return statements. Functions are usually the same across programming languages with minor syntax changes.

1. Finished with Functions!

`No answer needed`

## Task 5 Objects & Arrays

Learning about **Objects** and **Arrays** are heavy subjects, but let's try to break them into easy to understand sections.

Let's start with Objects.

**Objects**

The most important thing about objects is to remember that they're just another variation of variables. Here is a quick example of an object:

```js
var choosePill = {
    pillOne: 'Red',
    pillTwo: 'Blue'
}
```

I know we've already used millions of tables, but they're the best way to learn! So, here's a table displaying the code object we just wrote:

| Property | Stored Value |
|---|---|
| pillOne | Red |
| pillTwo | Blue |

Even though we only stored strings (you can remember strings based on the quotations), we can also store numbers. Here is our same code, updated:

```js
var choosePill = {
    pillOne: 'Red',
    pillTwo: 'Blue',
    numberOfPills: 2
}
var choice = choosePill.pillOne; // This will access the Objects property
```

| Property | Stored Value |
|---|---|
| pillOne | Red |
| pillTwo | Blue |
| numberOfPills | 2 |

**Arrays**

Arrays are fairly similar to objects, they have different stored values and syntax, but can be used for almost anything.

Here is the same code from before, but in an array:

```js
var choosePill = ['Red', 'Blue', 2];

var choice = choosePill[0];

console.log(choice); // Outputs 'Red'
```

* Reminder: Most programming languages start from 0, not 1, so when we access the **choosePill** variable, we grab the value from the 1st position.

Arrays are extremely important when storing multiple values within a variable. This way we don't have to write 100 variables and instead can store them all in a single variable, this is an array. There are multiple ways to write an array, but the simplest and most basic version is the example above.


**Quick Challenge**

What is the output of this code (Question #3)?

```js
var mrRobot = ['Elliot', 'Angela', 'Tyrell', 'Darlene'];

let character = mrRobot[2];

console.log(character); // What is the output?
```

1. What type of brackets are used for arrays?

`[]`

2. What color pill did we choose?

`Red Pill`

3. What is the output of this code?

`Tyrell`

## Task 6 Loops

**Loops**

Loops can be complicated, there are **for loops**, **while loops**, and **do...while loops**. Due to the complex nature of looping, I will be explaining the basic logic behind them and then give multiple examples of how they look and work. They are important in every programming language and every aspect of programming in general.

Let's begin with for loops!

**For Loop**

We will be creating a few mini-projects that will loop through a set of numbers.

Here is how a for loop appears in a code editor:

![](https://raw.githubusercontent.com/GhostlyPy/javascriptbasics/master/for_loop.png)

Let's break this code down line by line:

```js
for (a = 1; a <= 10; a++) {
    console.log(`Number: ${a}`); // Outputs 1-10 in our console
}
```

What does this code do? This code takes a, our variable, and loops through our specified range (1-10) and prints this to the console. By running this loop, we save our counter, or our range of numbers, to the variable a.

You'll notice that we have three statements (or components). Here is a quick list of what each one does:

1. The first component (a = 1): Executes before the loop starts.
2. The second component (a <= 10): Defines the condition for our loop.
3. The third component (a++): Executes each time after the loop starts.

* IMPORTANT: Each statement must be separated by a semi-colon. We can have more than one value within our first statement, but they must be separated by commas: for (a = 1, b = 2; component-2; component-3) {}

**While Loop**

The while loop is similar to the for loop, with a few minor differences:

```js
let x = 0;

while (x <= 3) {

console.log(x++); // Prints 0-3

}
```

This code will loop through x as long as it is less than or equal to three. This loop will continuing running until the desired outcome is met. Loops can be dangerous if the syntax is incorrectly written, then you can easily start an infinite loop, which isn't good for anyone in most cases. To avoid this, make sure that the loop eventually becomes false.

**Do...While Loop**

The basics of the do...while loop is the code will execute the loop before checking if the condition is true.

Example:

```js
let c = 10;

do {

console.log(c++); // Outputs 10-50

} while (c <= 50);
```

This code will ALWAYS execute at least once. It does this because loops normally require the conditions to be true, but a do...while loop doesn't require this as it executes before checking if the condition is truthy.

Loops are complicated but fairly simple once you understand the basics. There are still some things we haven't covered yet, but for the most part, loops have similar logic across programming languages. The thing about loops is that they have numerous variations that can do the same thing, but that's pretty much all of programming.

1. Loops repeat until the written code is finished running (true/false)

`true`

2. What loop doesn't require the condition to be true for it execute at least once?

`do...while`

## Task 7 Document Object Model (DOM)

**DOM**

Here is what we will be covering in the DOM section (keep in mind that these are just a few lines of code, DOM manipulation is a vast subject):

```js
document.getElementByID('Name_of_ID'); // Grabs the element with the ID name from the connected HTML file
document.getElementByClassName('Name_of_Class'); // Grabs the element with the class name from the connected HTML file
document.getElementByTagName('Name_of_Tag'); // Grabs a specific tag name from the connected HTML file
```

There are also methods we can use to access different things within our HTML files such as addEventListener, removeEventListener, and many more. Most of what the DOM does is change, replace, edit, or in some form, manipulate the HTML file or webpage that you're working on. For us to successfully manipulate the DOM, we use events. These events are added to HTML tags to work with our JavaScript file. Some of the more important events that are used a lot, you can find here:

* **onclick**: Activates when a user clicks on the specific element
* **onmouseover**: Activates when a user hovers over a specific element
* **onload**: Activates when the element has loaded
* and many more that are used. You can find a complete list here: https://www.w3schools.com/js/js_htmldom_events.asp

Code example:

![](https://raw.githubusercontent.com/GhostlyPy/javascriptbasics/master/DOM_POP.png)

Now when a user clicks on the **Click Me** button, an alert pops up that says **POP!!!**

The Document Object Model (DOM) has a ton of resources, and combining that with CSS will help your webpage **POP**. Eventually, if you take on web development or front-end programming, then you'll not only gain knowledge around the DOM, but you'll be able to manipulate a virtual DOM using React, cut your code in half using jQuery, and even combine your skills with PHP or Nodejs files for server manipulation. Good luck with your continued journey to learn JavaScript, and remember it can do so much more than just create webpages and add interactiveness.

1. What is the DOM?

`Document Object Model`

## Task 8 XSS

In this section, we'll be covering a few things that JavaScript can be used for in the information security industry.

Here is what we'll be covering:

* XSS (Cross-Site Scripting) & Web Application Penetration

**XSS**

Cross-Site Scripting is a security vulnerability that's typically found in web applications which can be used to execute a malicious script on the target's machine. We will only be covering some of the basics, so I recommend taking some time to visit the [XSS room](https://tryhackme.com/room/xss) provided by TryHackMe to get a more in-depth understanding of how to exploit a web application.

There are multiple types of attack when talking about XSS, here are some of my favorites:

* Keylogging
* Stealing Cookies
* Phishing
* and many more

A **keylogger** is used by setting up an event listener on the target's keyboard, which will track their keystrokes and save them on the attacker's server.

When an attacker steals a target's [cookies](https://tryhackme.com/room/xss), they can use that information to log in as the user without needing advanced authentication or even just find information stored in the cookies that could lead to devastating effects on the target's online saved accounts. This is why so many websites use SSL or some other form of protection against these attacks.

[Phishing](https://tryhackme.com/room/xss) is an interesting type of exploitation, an attacker can clone the website you're logging into and steal your credentials without you ever knowing. Another form of phishing is an attacker can insert code directly onto the webpage to change forms or input fields to steal the target's information.

![](https://raw.githubusercontent.com/GhostlyPy/javascriptbasics/master/reflected-cross-site-scripting-xss-attacks.png)


The three most common types that I've seen of XSS are DOM-Based XSS (type-0 XSS), Reflected XSS (Non-Persistent XSS), and Stored XSS (Persistent XSS):

* **DOM-Based XSS**: This is when an attack payload is executed by manipulating the DOM (Document Object Model) in the target's browser. This type uses the client-side code instead of server-side code.
* **Reflected XSS**: This is when a malicious script bounces off another website onto the target's web application or website. Normally, these are passed in the URL as a query, and it's easy as making the target click a link. This type originates from the target's request.
* **Stored XSS**: This is when a malicious script is directly injected into the webpage or web application. This type originates from the website's database.


As a final note, learning JavaScript will help you manipulate the DOM to gain the target's information, creating a keylogger script will log the keystrokes so that you can access the user's information with those keys pressed, and you can use web development knowledge to completely mirror a web application and trick the user into inputting their information. JavaScript is just a language, but when combined with certain techniques and tools, you can do some unbelievably devastating things to a target machine.


JavaScript is really important in the world of Cybersecurity, especially when it comes to web application exploitation, cross-site scripting, JavaScript injection, and many more methods.

Extra Resources:

Here are a few interesting, more in-depth, resources used for JavaScript in Cybersecurity:

* STÖK's "Hacker101 - JavaScript for Hackers": [https://www.youtube.com/watch?v=FTeE3OrTNoA](https://www.youtube.com/watch?v=FTeE3OrTNoA)
* TryHackMe's "Cross-site Scripting" Room: [https://tryhackme.com/room/xss](https://tryhackme.com/room/xss)
* Cross-Site Scripting (XSS) OWASP Website: [https://owasp.org/www-community/attacks/xss/](https://owasp.org/www-community/attacks/xss/)

1. What is it called when XSS is used to record keystrokes?

`Keylogging`

## Task 9 Final Notes

JavaScript is a versatile and impressive language. I did my best to put all of the important aspects in this room, but there are still hours of content I didn't cover. JavaScript has unlimited resources across the web, and if you're serious about web development, programming, or anything in between, I recommend looking into the logic behind the code. Learning how a programming language works will help you 100x more than just memorizing the syntax. You may not always have to explain the logic behind an algorithm, but being able to write the code and explain it can set you apart from the average enthusiast or web developer.

Since we only touched on the very basics, here are some free resources to continue learning JavaScript:

1. [https://developer.mozilla.org/en-US/docs/Web/JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
2. [https://www.w3schools.com/js/default.asp](https://www.w3schools.com/js/default.asp)

What wasn't covered in this room:

* Libraries or Frameworks
* ES6 Destructuring
* Advanced Methods
* And so much more!

If you'd like to learn more about programming logic or gain basic knowledge of another programming language, you can find these three rooms on TryHackMe:

1. optional's "Intro to Python": https://tryhackme.com/room/introtopython
2. bee's "Learn Rust": https://tryhackme.com/room/rust
3. TryHackMe's "Intro to x86-64": https://tryhackme.com/room/introtox8664

Thank you so much for taking the time to complete the JavaScript Basics room. If you have any questions or just wanna have a conversation about programming or computer "stuff", you can find me in the TryHackMe Discord: Ghostly#2568 or on YouTube where I upload THM rooms and other tech-related content here.

1. JavaScript Basics Master!

`No answer needed`

## Task 10 JavaScript Challenge

**Challenge Time!**

Get ready for a JavaScript code challenge!

For our challenge, we will be sorting an array of numbers using a JavaScript method.

*The challenge is available on my Github [here](https://github.com/GhostlyPy/javascriptbasics/blob/master/code_challenges.js), but I would recommend trying to figure out the solution first and researching the best method before viewing.

1. Sort the array [1,10,5,15,2,7,28,900,45,18,27]

```js
var numArray = [1,10,5,15,2,7,28,900,45,18,27]
numArray.sort(function(a, b) { return a - b });
console.log(numArray);
```

`1,2,5,7,10,15,18,27,28,45,900`
