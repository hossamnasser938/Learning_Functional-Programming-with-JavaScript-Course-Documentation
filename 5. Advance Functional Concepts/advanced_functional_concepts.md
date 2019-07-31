## introduction
* In this chapter we present 2 advanced concepts:
    * Partial application.
    * Recursion.


## Partial application
* **Partial application** is used to generate more specialized functions from general ones.

* Let's jump in to an example to understand the idea:
```js
const add = function( a, b ) {
    return a + b;
}

console.log( add( 1, 2 ) );
console.log( add( 1, 5 ) );
console.log( add( 1, 7 ) );
console.log( add( 1, 10 ) );
console.log( add( 1, 15 ) );
```

I know it is a trivial silly example but it illustrates the idea. You noticed that we use the function ` add ` with a given argument(which is 1 in this case) most frequently. Why not we create a function(**partial application**) that accept as arguments: the function that we wanna decrease its arguments and the arguments that we use most frequently.
```js
function partialApply( func, arg1 ) {
    return function( arg2 ) {
        fun( arg1, arg2 );
    }
}
```
Now we can use this function to generate a more specialized function from the general one ` add `.
```js
const add1o1 = partialApply( add, 1 );
```
Now we can use this function ` addTo1 `.
```js
console.log( addTo1( 2 ) );
console.log( addTo1( 5 ) );
console.log( addTo1( 7 ) );
console.log( addTo1( 10 ) );
console.log( addTo1( 15 ) );
```

* While this seems trivial and silly, this concept allows us to simplify most complicated things.

* One example that makes this concept clear is the network request process. In this process if you use ` XMLHttpRequest ` you have to specify many things. However, we have specialized cases like when we wanna just get data from api. The ` fetch ` method in the other hand accepts only the URL from where you wanna get data. Cases like this can simplify most complicated operations.
* References:
    * http://benalman.com/news/2012/09/partial-application-in-javascript/.


## Recursion
* We are familiar with **recursion**. It simply means defining a function that calls itself somewhere in its definition.

* A famous example is the **factorial**:
```js
function factorial( number ) {
    if ( number > 1 ) {
        return number * factorial( number - 1 )
    }
    else if ( number == 1 || number == 0 ) {
        return 1;
    }
    else {
        throw new Error("Error negative or NAN");
    }
}

console.log( `factorial of 5 = ${factorial( 4 )}` );
```
Generally you define cases to repeatedly call the same function and cases to stop calling.
