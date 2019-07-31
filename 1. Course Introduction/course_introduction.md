## Installing Node.js and npm
* For going through this course, we need to install:
    * **Node.js**  
    JavaScript is a **client-side** programming language running on the **browser** of the client. **Node.js** is a tool used to make JavaScript a **server-side** programming language running on a **server**. In this course, we use **Node.js** to run JavaScript using command-line outside a browser. It can be downloaded from **Node.js** official website: https://nodejs.org and then installed.

    * **NPM**(Node package manager)  
    which, as the name implies, is a program that **Node.js** uses to add, manage and keep track of packages or libraries installed and used in the project. It can be installed by hitting: ` npm install -g npm ` on the terminal.
        * to check the version of installed npm: ` npm -v `.
        * if at any point you get an error saying **permission denied**, just run everything as a super user by preceding your command by ` sudo `(**super user do**).


## Installing Lodash and running code
* This symbol **~** is known as **tilde** and pronounced **tilda**.
* To use **NPM** to manage your project packages you need to hit ` npm init -y ` which will create a file for you called **package.json** which keeps track of installed packages.
    * **Hint**: If we used ` npm init ` only it will ask some questions about your project and it will work. ` npm init -y ` means generate the file without asking about anything.
* After initiating **NPM** in our exercise files directory, we need to install a package called **Lodash** which contains a lot of useful methods for functional programming. To install it hit: ` npm install lodash --save `.   
* Now we have all the things needed to use **Node.js** to run our JavaScript files outside a browser. Just ` cd ` to where your file exists and hit ` node file-name.js `[or you can remove the extension ` .js `] and replace **file-name** with actual file name.


## What is functional programming?
* Both **functional programming** and **object-oriented programming** are **programming paradigms** so it is useful to understand what is a programming paradigm before understanding its examples.
* The word **paradigm** means a **model**, a **pattern** of something. A **paradigm** means a **model** in the sense that it's used to identify and generate a wide range of examples that follow a bunch of concepts.  
* Technically, I can define a **programming paradigm** as a **model** of programming defined by a **set of concepts** that when followed **correctly** achieve **certain goals** that facilitate and organize software development as well as produce efficient software products.
* Now we understand what a **programming paradigm** is let's see what is **functional programming**?
* As we said, a **programming paradigm** is defined by a **set of concepts** so let's see the concepts of functional programming:
    * Keeping functions and data separate.
    * Avoiding state change and mutable data.
    * Treating functions as first-class citizens.
* Now let's dig into each concept to get a basic understanding of it.

#### Keeping functions and data separate
* Consider this **object-oriented** approach:
```js
class Student {
    constructor( name, age ) {
        this.name = name;
        this.age = age;
    }

    getOlder() {
        this.age++;
    }
}

var s = new Student( "Hos", 22 );
s.getOlder();
```
Here the class ` Student ` combines both the **data**(` age `) and the **functions**(` getOlder `) that operate on that data.
* In **functional programming** we separate data from functions by storing data in simple constructs like Arrays or Hashes and define functions that take data as arguments and returns a transformed version of that data.
```js
var s = {
    name: "Hos",
    age: 22;
}

function increaseAge( obj ) {
    let coppiedObj = _.cloneDeep( obj );  
    /* copy object. We could use: let coppiedObj = { ...obj }; */
    
    coppiedObj.age++;
    return coppiedObj;
}
var olderS = increaseAge( s );
```
Forget about copping the object and returning a new version of it now. It will be clear by the next concept.
* The most useful thing here is the immediate **polymorphism** you get since the ` increaseAge ` **function** can operate on **any type** of object that has ` age ` **property**. Its usage is not restricted to a specific type of object.

#### Avoiding state change and mutable data
* **Mutable** means can be **changed**.
* An example of using mutable data:
```js
var greeting = "Hello";
// ...
greeting += ", Welcome on Board";
// ...
greeting = greeting.toUpperCase();
// ...
```
In this case a single variable can hold many different values throughout its life-cycle depending on where and when you reference this variable. In a real project this introduces a problem since changing the variable affecting many other operations depending on the state of that variable.
* Let's follow the concept of avoiding state change and mutable data.
```js
const greeting = "Hello";
// ...
const longGreeting = greeting + ", Welcome on Board";
// ...
const longShoutedGreeting = longGreeting.toUpperCase();
// ...
```
* The way we achieve this concept is either by enforcing immutability like what we did using immutable data like ` const ` or use the data in an immutable way. This enforces us to make a new variable for each state change.
* By following this we can infer just by looking to the variable name what it contains. This concept helps a lot in **debugging**.
* You may think that this process of coping each object, modifying it and assigning a new version of it may be costly. Actually it is relatively inexpensive since we use simple constructs to hold data.       

#### Treating functions as first-class citizens
* **First-class** citizens are citizens that receive **fair treatment**(**having all their rights**).
* **Second-class** citizens are citizens that receive **inferior treatment**(**lacking some of their rights**).
* In programming:
    * A **first-class citizen**(value, object, function, or type) is an entity that **supports all** the **operations** available by other entities.
    * These **operations** include:
        * being passed as argument.
        * returned from a function.
        * assigned to a variable.
        * modified.
    * A **second-class citizen** is an entity that does not support some of these operations.
* In functional programming functions are treated as first-class citizens which means they support all operations.
```js
function acceptFun( fun ) {  // Passing a function as argument
    return fun(3);
}

function f( x ) {
    return x * 2;
}

console.log( acceptFun( f ) );  // 6
```
```js
function returnFun() {  // Returning a function from a function
    return function() { return 1; };
}

console.log( returnFun()() );  // 1
```
```js
var assignedFun = function() { return 1; };  // Assigning function to a variables
console.log( assignedFun() );  // 1
```

* This feature of allowing functions these operations provides very useful **flexibility** and **reusability**.
    * A use case in my mind now: I have built a Login/Register process in Android using Java with Fire-base as the back-end service. In both processes(Login and Register) you make common operations(such as checking email and password format and checking the internet connection) except for the last one which calling a different function related to Fire-base. It would be very useful to make a function that wrap all the common operations and accept as argument a function either login or register. It would significantly reduce code.


## Functional vs. object-oriented programming (OOP)
* ` concat ` is a  **function** that operates on Arrays. It returns a new ` Array ` which is the result of concatenating the original array and whatever given as arguments(values or Array). It does not mutate(modify) the original array.
```js
var menoufFriends = new Array( "Ayman", "Mossab", "Ismail" );
var allFriends = menoufFriends.concat( ["Gamal", "Adham"] );
var completeFriends = allFriends.concat( "Abobakr" );

console.log( menoufFriends ); //["Ayman", "Mossab", "Ismail"]
console.log( allFriends ); //["Ayman", "Mossab", "Ismail", "Gamal", "Adham"]
console.log( completeFriends );  // ["Ayman", "Mossab", "Ismail", "Gamal", "Adham", "Abobakr"]
```
* Let's compare between both paradigms:
    * The **main units of computer programs** in *OOP* is **classes and objects**. However, in *FP* they are **functions**. Let's see how we can implement a basic to-do list using both approaches:
        * In *OOP*
        ```js
        class ToDoList {
            constructor( list ) {
                this.list = list;
            }

            addItem( item ) {
                this.list.push( item );
            }
        }

        class ToDoItem {
            constructor( task, time ) {
                this.task = task;
                this.time = time;
            }
        }
        ```
        
        * In *FP*
        ```js
        function addToDoItems( list, items ) {
            return list.concat( items );
        }
        ```
        In the next point you will understand why we keep it so simple in *FP*.
    * On creating objects, with *OOP* we must decide what sort of object each one represents(**which class**). However, in *FP* we are not so concerned with (defining what everything is and what it can do). Instead we are concerned with **what the raw data** itself is and **what operations** and transformations we can perform on it. This gives us the flexibility to define objects as key-value pairs(what's called hashes). Let's initialize a To-Do list using both approaches:
        * In *OOP*
        ```js
        var myToDoList = new ToDoList( [
            new ToDoItem( "Fajr", "5 - 6" ),
            new ToDoItem( "Azkar", "6 - 7" ),
            new ToDoItem( "Study JS", "7 - 12")
            ] );
        ```
        * In *FP*
        ```js
        const morningToDoList = [
            { task: "Fajr", time: "5 - 6" },
            { task: "Azkar", time: "6 - 7"},
            { task: "Study JS", time: "7 - 12"}
        ];
        ```
        Notice the difference in creating objects.
    * Executing methods on objects in *OOP* **mutates** the original data(by changing its state). However, in *FP* on calling functions with data as arguments we create a new variable to store the result for each state change and keep the **original data intact**(as it is). This difference makes *OOP* does not give attention to why we change data. However in *FP* you must come up with a name for each state change which enforces giving attention to these changes. This(if done correctly) improves our code readability and reduces bugs. Now let's add more ToDo items to our list using both approaches:
        * In *OOP*
        ```js
        myToDoList.addItem( new ToDoItem( "Duhr", "12 - 1" ) );
        myToDoList.addItem( new ToDoItem( "Study JS", "1 - 3" ) );
        ```
        * In *FP*
        ```js
        const morning_afternoonToDoList = addToDoItems( morningToDoList, [
            { task: "Duhr", time: "12 - 1" },
            { task: "Study JS", time: "1 - 3" }
            ] );
        ```
        Notice that in *FP* the names of variables refer to their content which makes the code more readable and makes it easy to debug.


## Chapter quiz
1. --- is a package manager for Node?  
npm.
2. Which command will allow you to install Lodash successfully?  
npm install lodash --save.
3. One of the key benefits of functional programming is that debugging is much easier?  
TRUE.
4. One key benefit of Functional programming is that it does not --- the original data?  
mutate.
