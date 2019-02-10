## Assigning functions to variables
* There is only one difference between using:
    * **normal functions**
    ```
    function sayHello() {
        console.log( "Hello" );
    }
    ```
    * **functions assigned to a variable**
    ```
    const sayHello = function() {
        console.log( "Hello" );
    }
    ```
    * This **difference** is that:
        * **normal functions** can be called before they are declared since functions in JavaScript are **hoisted**(As we said in the course **Up and Running with ES6**).
        ```
        sayHello();  // Hello
        function sayHello() {
            console.log( "Hello" );
        }
        sayHello();  // Hello
        ```  
        * **functions assigned to a variable** can not be called before they are declared. Although, variables are also **hoisted** in JavaScript, their declaration only is **hoisted** not their initialization(we said that as well in the course **Up and Running with ES6**).
        ```
        sayHello();  // undefined
        const sayHello = function() {
            console.log( "Hello" );
        }
        sayHello();  // Hello
        ```
* Some **use cases** where **functions assigned to a variable** help
    * Renaming built-in functions to something shorter.
    ```
    const print = console.log;
    print( "Hello" );  // Hello
    ```
    Here we renamed the function ` console.log ` to ` print ` by assigning ` console.log ` function to ` print ` variable. Remember that if you forgot and added the parenthesis to ` console.log ` like this.
    ```
    const print = console.log();
    print( "Hello" );  // Error: print is not a function
    ```
    Since ` console.log() ` actually calls ` console.log ` and assigning whatever is returned from it(which in this case is ` undefined `) in ` print ` variable, now ` print ` is ` undefined ` rather than a function.
    * Changing the behavior of a function based on environment state.  
    The ability to assign a function to a variable gives you the flexibility to assign a single variable different functions dynamically based on the environment state which is something you can not achieve using normal functions.


## Passing functions as arguments
* We are familiar with passing functions as arguments. Nothing is new. For a typical use case for that concept go to chapter 1(Course Introduction) under the video(What is functional programming?) under the section (Treating functions as first-class citizens).


## Closure and returning functions
* We are familiar with returning functions as well as returning object wrapping multiple functions. In addition we are familiar with the concept of closures. For info about closures go to the course(JavaScript Essential Training) under chapter 4(Functions and Objects) under the video(Closures). For a use case of Closures go to the course(Up and Running with ECMAScript 6) under chapter 3(ES6 Syntax) under the video(Let keyword) under the section(Block -> for loop).
* Another use case of **Closures** is making variables **private**.
    * Consider this object-oriented approach to create a Counter object.
    ```
    const myCounter = {
        count: 0,
        increment: function() {
            count++;
        },
        currentValue: function() {
            return count;
        }
    }
    ```
    This object works well as an object-oriented model. However, any one can assign the property ` count ` any value he wants without following our predefined way which is the two methods we defined since in JavaScript we do not have **private** variables.
    ```
    myCounter.count = 256;
    ```
    * **Closures** allow us to make variables **private**.
    ```
    function createCounter() {
        var count = 0;
        return {
            increment: function() {
                count++;
            },
            currentValue: function() {
                return count;
            }
        };
    }
    ```
    Now we can use the **function** ` createCounter ` to create a counter for us with ` count ` only accessible through the methods ` increment ` and ` currentValue `.
    ```
    const myCounter = createCounter();
    console.log( myCounter.currentValue() );  // 0
    myCounter.increment();
    console.log( myCounter.currentValue() );  // 1
    ```
    However if we tried to access the ` count ` property without neither ` increment ` nor ` currentValue ` we get ` undefined `.
    ```
    console.log( myCounter.count );  // undefined   
    ```
    What is worse is that:
    ```
    myCounter.count = 55;
    console.log( myCounter.count );  // 55
    console.log( myCounter.currentValue() );  // 1
    ```   
    What happens here is that on assigning a value to a property called ` count ` JavaScript will not find that property so it will add a new property with that name to the ` myCounter ` object while still our counter has its value before that assignment.
        * Also we can pass an initial value to the counter.
        ```
        function createCounter( initial ) {
            var count = initial;
            return {
                increment: function() {
                    count++;
                },
                currentValue: function() {
                    return count;
                }
            };
        }
        ```
        This is not strange. However, we can eliminate this line ` var count = initial `.
        ```
        function createCounter( count ) {
            return {
                increment: function() {
                    count++;
                },
                currentValue: function() {
                    return count;
                }
            };
        }
        ```
        We can do that since values given to the function is one of its lexical environment and it is wrapped with it(closure).
        * However, this way of using **Closures** to make variables **private** does not follow the **functional programming** paradigm since it **combines data and objects** together.


## Higher-Order Function
* A **higher-order function** is a function that takes as argument a function, returns a function, or both.
* Let's see some use cases:
```
function printMessageNTimes( message, n ) {
    if ( message != null && typeof message === "string", message != "" ) {
        if ( n != null && typeof n === "number" && n > 0 ) {
            for ( let i = 0; i < n; i++ ) {
                console.log( message );
            }
        }
    }
}
function getNthLetter( message, n ) {
    if ( message != null && typeof message === "string", message != "" ) {
        if ( n != null && typeof n === "number" && n > 0 ) {
            return message.charAt( n - 1 );
        }
    }
}
printMessageNTimes( "Hos", 10 );
getNthLetter( "Hossam", 3 );
```  
Repeating this validation code does not seem to be a good practice.
```
function validateandDo( message, n, fun ) {
    if ( message != null && typeof message === "string", message != "" ) {
        if ( n != null && typeof n === "number" && n > 0 ) {
            return fun( message, n );
        }
    }
}
function printMessageNTimes( message, n ) {
    for ( let i = 0; i < n; i++ ) {
        console.log( message );
    }
}
function getNthLetter( message, n ) {
    return message.charAt( n - 1 );
}
validateandDo( "Hos", 10, printMessageNTimes );
validateandDo( "Hossam", 3, getNthLetter );
```        
Less code and more efficient. We used the concept of passing functions as arguments. We can use the concept of returning a function to achieve the same goal with a different approach.
```
function getSafeVersion( fun ) {
    return function( message, n ) {
        if ( message != null && typeof message === "string", message != "" ) {
            if ( n != null && typeof n === "number" && n > 0 ) {
                return fun( message, n );
            }
        }
    }
}
function printMessageNTimes( message, n ) {
    for ( let i = 0; i < n; i++ ) {
        console.log( message );
    }
}
function getNthLetter( message, n ) {
    return message.charAt( n - 1 );
}
const safePrintMessageNTimes = getSafeVersion( printMessageNTimes );
const safeGetNthLetter = getSafeVersion( getNthLetter );
safePrintMessageNTimes( "Hos", 10 );
safeGetNthLetter( "Hossam", 3 );
```


## Chapter quiz
1. Which line of code would rename console.log to line?  
var line = console.log
2. A(n) _____ function does not have a name?  
anonymous
