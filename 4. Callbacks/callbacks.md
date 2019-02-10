## Chapter introduction
* We are familiar with **Asynchronous operations**. They are operations that take some time to be completed such as:
    * Reading and writing files.
    * Accessing databases.
    * Performing network requests.
* These operations if done in the normal way will result in delay in system response which is a bad user experience. Alternatively, they are done on another thread working in the background and keeping the system interactive.
* The way we do that is by specifying a function to be executed once these long operations are completed. These functions are called **callbacks**.
* The ability to pass functions as arguments in JavaScript(Treating functions as first-class citizens) has a very useful implications in performing these asynchronous operations by passing these callbacks.


## Simple callbacks
* An example of using **callbacks** is the function ` setTimeout ` which we encountered in the course **Up and running with ES6** in chapter 5(Asynchronous features).
* ` setTimeout ` is used to delay the execution of a certain function(**callback**) a certain amount of time.
* Let's see some example:
```
var x = 1;
console.log( `x is originally ${x}`);
setTimeout( () => {
    x = 5;
    console.log( `setTimeout changed x to ${x}` )
    }, 3000 );
console.log( `x after setTimeout(in the script) is ${x}` );
```
Output:  
x is originally 1  
x after setTimeout(in the script) is 1  
setTimeout changed x to 5  
This behaviour is because ` setTimeout ` is an **asynchronous function** that lets the script continues executing and calling its callback whenever the certain time passes.


## Callbacks with arguments
* **Callbacks** are rarely used with no arguments. Most of the cases **callbacks** accept arguments such as the result or the error encountered from the asynchronous operation being executed.
* For an example of callbacks with arguments head to the course ` Up and running with ES6` chapter5(Asynchronous features) and see the callback passed to ` Promise ` constructor.
* For another example let's see how we read a file asynchronously in **node.js**. We use a method in a library included with **node.js** called ` fs `. This method is named ` readFile `. ` readFile ` accepts as arguments:
    * the name of the file to be read.
    * the character encoding.
    * the callback function to be executed when the file is read. This callback accepts two arguments: the first is any encountered error while reading the file if exists, and the second is the result of reading the file.
```
var fs = require( "fs" );
fs.readFile( 'message.txt', "utf8", ( error, result ) {
    console.log( result );
    } );
```   
Output:  
This message was read asynchronously from a file!  
