## Mapping
* We are familiar with the ` map ` function used with **arrays** to apply a specific **function** on each element and return an array of the result.
```
var numbers = [1, 2, 3, 4];
var squares = numbers.map( (element) => element * element );
console.log( squares );  // [1, 4, 9, 16]
console.log( numbers );  // [1, 2, 3, 4]
```
The point here is that ` map `, and many other functions as we will see, does not affect the original array. It returns a new one after applying the function without affecting the original. This allows the concept of ` Avoiding state change and mutable data `. We treat the original array in an immutable way.
* There is a strange syntax(for me) to do the same job which is using the ` underscore.js ` like that:
```
var squares = _.map( numbers, (element) => element * element );
```
In order to use this syntax you need a library. The instructor used ` lodash ` which we installed in the beginning of the course.  
```
var _ = require( "lodash" );
```
I went to their official (website)[https://underscorejs.org/] trying to figure out what it is. It seems that it serves two purposes:
    * It **simplifies** the code a little bit. However it is not clear that it does in our example but in advanced use cases it does.
    * It helps applying **functional programming**. How? have no idea yet.


## Filtering
* Now let's see a new function which is ` filter `. ` filter ` is also applied on **arrays** and is used, as the name implies, to **filter** arrays based on a given **criteria** or simply extracting some elements that evaluates a given condition.
* ` filter ` function is given as argument the **array** to be filtered and the **function** to be applied on the elements of the array to filter it(that is the case when using the underscore library. However if we used the normal syntax we would pass only the function). This function should return a condition that when met the element is added to the resulting array.
* Let's see some examples:
    * Suppose that we wanna extract the even number from an array of numbers.
    ```
    var numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    ```
    We have two ways:
        * The **procedural** way.
        ```
        var evenNumbers = [];
        for ( let number of numbers ) {
            if ( number % 2 === 0 ) {
                evenNumbers.push( number );
            }
        }
        console.log( evenNumbers );  // [2, 4, 6, 8]
        ```
        * The **functional** way
        ```
        var evenNumbers = _.filter( numbers, (number) => number % 2 === 0 );
        console.log( evenNumbers );  // The same output
        ```
        Recall that if used this condition ` !(number % 2) ` it works.
    * Suppose that we wanna extract all the employees that have salary larger than 10000.
    ```
    var employees = [ { name: "Hossam", salary: 7000 },
        { name: "Ismail", salary: 11000 },
        { name: "Ayman", salary: 8000 },
        { name: "Mossab", salary: 12000},
        { name: "Hassan", salary: 15000 }
    ];
    ```
    We also have two ways:
        * The **procedural** way.
        ```
        var highEncomeEmployees = [];
        for ( let employee of employees ) {
            if ( employee.salary > 10000 ) {
                highEncomeEmployees.push( employee );
            }
        }
        console.log( highEncomeEmployees );
        ```
        Output:  
        [ { name: "Ismail", salary: 11000 },  
        { name: "Mossab", salary: 12000},  
        { name: "Hassan", salary: 15000 } ]
        * The **functional** way.
        ```
        var highEncomeEmployees = _.filter( employees, (employee) => employee.salary > 10000 );
        console.log( highEncomeEmployees );
        ```
        The same output.


## Every/Some
* Now we introduce 2 methods that are similar to ` filter `:
    * ` every ` is used to check if **all** of the array elements evaluate a given condition.
    * ` some ` is used to check if **at least one** of the array elements evaluates a given condition.
* Both of them works the same way as ` filter `. They are given as arguments a function that returns a condition and both of them return a single ` boolean ` value indicating that:
    * ` every ` **all** elements evaluate the condition or non of them does.
    * ` some ` **there is** some(at least one) elements that evaluate this condition or no one evaluates this condition.
* Let's see some examples:  
We have this array of numbers:
```
var numbers = [2, 4, 8, 16, 32];
```
    * Let's check whether **all** the elements of the array are even numbers.
        * The **procedural** way.
        ```
        var allEven = true;
        for ( number of numbers ) {
            if ( number % 2 !== 0 ) {
                allEven = false;
                break;
            }
        }
        console.log( `All elements are even: ${allEven}` );  // All elements are even: true
        ```
        * The **functional** way.
        ```
        var allEven = _.every( numbers, (number) => number % 2 === 0 );
        console.log( `All elements are even: ${allEven}` );  // The same output
        ```
        Now if we added the number 5 or any odd number the result will be: ` allEven = false`.
    * Let's check whether **there are** numbers that can be divided by 5 or not.
        * The **procedural** way.
        ```
        var thereIs5Dividend = false;
        for ( number of numbers ) {
            if ( number % 5 === 0 ) {
                thereIs5Dividend = true;
                break;
            }
        }
        console.log( `There is at least a number that can be divided by 5: ${thereIs5Dividend}` );
        // There is at least a number that can be divided by 5: false
        ```  
        * The **functional** way.
        ```
        var thereIs5Dividend = _.some( numbers, (number) => number % 5 === 0 );
        console.log( `There is at least a number that can be divided by 5: ${thereIs5Dividend}` );
        // The same output.
        ```
        Now if we added the number 5 or any number that can be divided by 5 the result will be
        ` thereIs5Dividend = true `.


## Reducing
* Now we introduce a little different function from what we have seen so far. ` reduce ` function is used, as the name implies, to **reduce** a number of items to only one item such as taking an array of numbers and returning the summation or the product of them.
* the ` reduce ` function accepts as arguments:
    * the array of numbers to reduce.
    * the function to be applied on the numbers to be reduced.
    * the initial value of the accumulator which holds the reduced value.
* The function which is passed as argument to the ` reduce ` function accepts two arguments not one as we expect:
    * the accumulator which hold the reduced item by each iteration over the array.
    * the element itself of the array.
* Let's see some examples:
    * Reducing an array of numbers to their summation.
    ```
    var numbers = [1, 2, 3, 4, 5, 6];
    ```
        * The **procedural** way.
        ```
        var summation = 0;
        for ( number of numbers ) {
            summation += number;
        }
        console.log( summation );  // 21
        ```
        * The **functional** way.
        ```
        var summation = _.reduce( numbers, ( accumulator, number ) => accumulator + number, 0 );
        // The same output
        ```
    * Reducing an array of numbers to their product.
    ```
    var numbers = [10, 20, 30];
    ```
        * The **procedural** way.
        ```
        var product = 1;
        for ( number of numbers ) {
            product *= number;
        }
        console.log( product );  // 6000
        ```
        * The **functional** way.
        ```
        var product = _.reduce( numbers, ( accumulator, number ) => accumulator * number, 1 );
          console.log( product );  // The same output
        ```


## Combining functions
* Let's see through an example how to combine these functions to get to your goal.
* Suppose that we have an array of employees(males and females) and we wanna calculate the average of males salaries as well as the average of females salaries.
```
var employees = [
    { name: "Hossam", salary: 10000, gender: 'M' },
    { name: "Ayman", salary: 9000, gender: 'M' },
    { name: "Mona", salary: 12000, gender: 'F' },
    { name: "Hassan", salary: 20000, gender: 'M' },
    { name: "Mossab", salary: 15000, gender: 'M' },
    { name: "Yassmin", salary: 6000, gender: 'F' },
    { name: "Samah", salary: 8000, gender: 'F' }
];
```
* Here we are gonna demonstrate the process of calculating the average of males salaries and the females salaries is very similar.
    1. filter the employees to only males.
    ```
    var maleEmps = _.filter( employees, (emp) => emp.gender === 'M' );
    ```
    2. map all employees to numbers representing salaries
    ```
    var maleEmpsSalaries = _.map( maleEmps, ( emp ) => emp.salary );
    ```
    2. sum their salaries.
    ```
    var maleEmpsSalarySum = _.reduce( maleEmpsSalaries, ( accumulator, sal ) => accumulator + sal, 0 );
    ```
    3. divide by their number   
    ```
    var maleEmpsSalaryAvg = maleEmpsSalarySum / maleEmps.length;
    console.log( maleEmpsSalaryAvg );  // 13500
    ```


## Chapter quiz
1. What would you use to double all elements in an array?  
mapping.
