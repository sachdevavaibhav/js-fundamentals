# JavaScript Fundamentals 

## How does JS works?
- **Everything in JS happens inside the execution context**. It consists of memory (variable environment) and code (thread of execution). Memory is a key value pair environment where all variables (including function names etc) are stored. In the thread of execution, code is executed line by line.
- JS is a synchronous single-threaded language. That means the code is executed synchronously line by line in a single thread.
    -  **Process**: A process is an executing program. It consists of a code segment
        (which contains the program instructions) and a data segment (which
        contains the variables, etc. used by the program). A process also has
        some associated properties, such as a unique process identifier (PID),
        security attributes, and so on.
     
    -   **Thread**: A thread is a subset of a process. A process can have one or more
        threads. A thread has its own stack, and a thread shares the code segment
        and the data segment of the process to which it belongs. A thread also
        has some associated properties, such as an execution state (running,
        ready, and so on) and a unique thread identifier.
     
    -   A process can have multiple threads, and each thread has its own stack.
       Each thread shares the code and data segments of the process to which it
       belongs. A thread has its own execution state and a unique thread
       identifier.

## How is the JS code executed?
- When a JS code is executed, an **execution context** is created and everything happens inside this execution context. It represents the environment in which code is executed. It includes all the information needed to manage the code's execution, such as variables, functions, and the scope chain.
- There are mainly two types of execution context:
    1. **Global Execution Context**: It is created when the program is run and is associated with the whole script.
    2. **Function Execution Context**: It is created when a function is called and is associated with that function only. 
- Each execution context undergoes two phases:
    1. Memory Allocation Phase (Variable environment): All the variables associated with the script/function are initialized with the value of **undefined (a special keyword)**. If it encounters a function in its way, it stores the whole function code mapped to its function name.
    2. Code Execution Phase (Thread of execution): It is here where all code is executed line by line in a single thread synchronously. All the variables are assigned their values and when it sees a function call it creates a function execution context and the whole process is repeated.
- JavaScript manages these execution contexts using call stack/execution context stack. Stack is a data structure which uses LIFO principle (Last in first out). When the global execution stack is created it is pushed to this call stack. Also when a function stack is created it is pushed to this stack and is removed and destroyed as soon as the function returns. The global context is also removed and destroyed as soon as the program execution ends.

## Hoisting in JS:
-  Hoisting is a unique behavior in JavaScript where the declarations of functions, variables, and classes are moved to the top of their scope before any code is executed. This means you can access a function or variable before its actual declaration in the code.
-  Hoisting is a consequence of memory allocation phase of execution context. This allows functions to be called before their definition and variables to be accessed before their initial values are assigned.

## Shortest JS program
- An empty js file is the shortest program since it will do everything behind the scenes that it will do for a million lines of code. It will create the execution context and execute the empty file.
- As soon as the JS code is executed the JS engine will create a window object in the global scope with lots of methods and properties. It is created along with the global execution context (it is part of the GEC). Apart from it a **_this_** keyword is created which points to this window object.
  
 ![image](https://github.com/sachdevavaibhav/js-fundamentals/assets/72242181/f1a41879-e893-4df0-b335-b9aad83e6c77)

- There is no window object in Node.js but a global object is created which is kind of similar.
- Any variable or function that we create is created in a global object and is attached to it. And anything inside a function is attached to the scope of that function.

## undefined vs not defined:
- undefined is a primitive data type which signifies absence of value. In the first phase of execution context when variables are assigned memory undefined act as placeholder. But when a variable is neither declared nor initialized and the program tries to access it the JS engine throws **not defined** error.
- Apart from undefined there is another primitive data type called null which signifies intentional absence of value. If you want to intentionally keep a variable empty or don’t want to return anything from function use null.

## Scope chain:
- **Scope** means where you can access specific variables or a function inside code is called scope. Scope is directly dependent on the lexical environment.
- **"lexical"** refers to the structure of the source code. Lexical scoping, a key concept in JavaScript, means that the scope of a variable is determined by its position in the source code. The word "lexical" emphasizes that the scope is based on the physical placement of variables in the code, allowing for predictable variable resolution based on the code's structure.
- The lexical environment is a place where variables are declared and reside along with the reference to the outer/parent lexical environment.
- **Scope chain** is the mechanism in which the JS engine looks for the variable in current lexical environment and if not found it looks in the parent lexical environment until it reaches the global lexical environment.
  
  ![Screenshot from 2023-12-11 19-04-57](https://github.com/sachdevavaibhav/js-fundamentals/assets/72242181/b6cbc76d-7fe8-406f-b4c3-8e520ede186d)

  
## let & const:
- let & const variable declarations are hoisted but they are hoisted in a very different manner than var. let & const are not accessible until they are initialized and the JS engine throws a reference error with a message: **cannot access before initialization**.<br/>

  Please note this error message is not the same when the variable is not present in the code. This also shows that JavaScript knows that variable is declared and is hoisted. Though both errors are ReferenceError the error message is different and tells the JS behavior. If the variable was not present in the code the message will be: **_variable_ not defined.**
- let & const variables are not attached to the Global object though they live inside their execution context but in a separate space than Global.
- **Temporal dead zone** is the time between the let & const variables are hoisted and initialized. During this period these variables can’t be accessed and the JS engine will throw a _ReferenceError_.
- let variables can’t be re-declared once it's declared. The JS engine will throw a SyntaxError and not even a single line of code will be executed.
- It is possible to declare the let variable in one line and initialize it somewhere else in the code. But this is not the case with const variables, they need to be declared and initialized in the same line otherwise JS engine will throw SyntaxError: missing initializer in the const declaration.
- If we try to re-assign a value to the const variable the JS engine will throw a TypeError because the operation violated the rules set by const keyword.

## Block scope and shadowing:
- The term "block" typically refers to a set of statements enclosed within curly braces {}. These blocks are used to group statements together. Block is used where javascript expects a statement (but we want to have a group of statements). A block is also known as a Compound Statement.
- The variables declared using let & const are block scoped which means they are available inside the block and not outside of it. The lexical scope chain applies as it applies elsewhere.
- Shadowing occurs when a variable declared in a certain scope has the same name as a variable declared in an outer scope. When this happens, the inner variable "shadows" the outer variable, meaning that any attempt to access the variable within the inner scope will refer to the inner variable, not the outer one.
- Shadowing works similarly in function scope.
- Shadowing a let variable with var type is not allowed and this is called Illegal Shadowing.(Why?)
Because var ‘overflows’ from the inner scope to outer (since it is function scoped) where let variable is present and we are now left with multiple declarations of let variable which is invalid.

![Screenshot from 2023-12-11 20-35-21](https://github.com/sachdevavaibhav/js-fundamentals/assets/72242181/d3411d1e-6e23-404a-bfca-c1455ec98d7f)

## Closures:
- A **closure** in JavaScript is a function **bundled together with references to its surrounding state** (the lexical environment) where it was declared. This allows the function to retain access to those external variables even when the function is invoked outside of its original lexical scope.
```js
function x() {
  var a = 10;

  return function y() {
    console.log(a);
  }
}

const z = x();
console.log(z);
z(); //10 
```
- The closure has the reference to the identifier and not the actual value. So, if at any time the value in a particular identifier changes it will reflect at other places.
- Uses of closures:
    1. Module Design Pattern
    2. Currying
    3. Functions like once
    4. Memoize
    5. Maintaining state in async world
    6. setTimeouts
    7. Iterators
    8. And many more ...

## setTimeout and closures:
- It’s one of the most asked interview questions.<br/>
  **Ques: Create a function that utilizes setTimeout to print numbers from 1 to 5, where each number is displayed after a corresponding time interval (e.g., 1 is printed after 1 second, 2 after 2 seconds, and so on)?**
  Ans:  The naive approach to this solution would be:
  
    ```js
  function x() {
      for (var i=1; i<=5; i++) {
        setTimeout(function () {
          console.log(i);
        }, i*1000);
      }
    }
    x();
  ```
  But this is the wrong approach and the solution will not work because:
    - The setTimeout callback will create a closure and have reference to its lexical environment. We are particularly interested in the variable i and the closure has reference to it.
    - The variable **i** is declared with **var** and hence has **function scope**. By the time the callbacks execute, the loop has completed, and i has the final value of 6 which is incorrect.
      
  One straight away solution is to use **let** declaration instead of **var** since it has a block scope and ensures each iteration has its own unique **i** value in the closure. Every time the loop runs, a fresh, independent copy of that variable is created. This copy exists only within that specific loop iteration and has no connection to the variable outside the loop or in previous iterations.

  Solution while using **var** declaration:
     - Another solution to the problem while having **var** declaration is to enclose the setTimeout in another function and pass the value of **i** as an argument to that function.<br/>
       ```js
        function x() {
          for (var i=1; i<=5; i++) {
            function close(y) {
              setTimeout(function () {
                console.log(y);
              }, y*1000);
            }
            close(i);
          }
        }
        
        x();
       ```

       This works because in JS the arguments in a function are **pass-by-value** and it will create a copy of that variable in its scope whenever the function is called. So now each invocation creates a separate copy of the variable within the function's scope and the setTimeout closure will have a distinct reference to the individual copy of the variable.

## Function Statement:
- It is one of the ways to declare a function using function keyword followed by function name, its parameters and function body enclosed in curly braces. It is also called function declaration.
```js
function greet(name) {
  console.log("Hello, " + name + "!");
}
```
## Funtion expression:
- Functions are the heart of JS and can act as a value. When a function is assigned to a variable it’s called function expression.
```js
var b = function(name) {
  console.log("Hello, " + name + "!");	
}
```
## Difference b/w function statement and function expression:
- The major difference between them is the way they are hoisted. In a function statement the identifier points to the function code in the memory phase and hence it can be called before its actual declaration. But in case of function expression the identifier (variable) is hoisted with value of undefined in case of var and in case of let & const it is under the temporal dead zone. So, when function expressions are called before actual initialisation the JS engine will give TypeError in case of **var** and ReferenceError in case of **let & const**.

## Function declaration:
- It is the same as a function statement and there is no difference.

## Anonymous functions:
- Functions that are defined without a specified name are called anonymous functions. They are only used as values since they don’t have their own identity. It can be used in function expressions, when a function is passed as an argument to another function or when a function is returned from another function.

```js
var b = function(name) {
  console.log("Hello, " + name + "!");	
}
```
## Named function expression:
- Named function expressions are function expressions that have a name identifier, but the name is only accessible within the function itself. The main advantage of using named function expressions is improved debugging, as the function name can be used in stack traces. In terms of hoisting they behave the same as function expressions.

```js
var add = function namedAdd(a, b) {
  console.log(namedAdd); // Outputs the function definition
  return a + b;
};
```

## Parameters vs Arguments: 
- Parameters are the variables listed in the function declaration and act as placeholders for values that the function will receive when it is called.
- Arguments are the actual values passed to a function when it is invoked.
- As a side note we can also pass functions as arguments in JS.

## First class functions:
- It refers to the concept that functions **act as values** and can be passed as arguments to other functions, assigned to variables, returned from functions and stored in a data structure. Such functions are also called **first class citizens**.

## Callback functions:
- Callback function is a function that is passed as an argument to another function.
- Callback functions enable the asynchronous behavior of JS even though JS is a synchronous non-blocking single threaded language. We will cover this aspect later on.
  ```js
    function x(y) {
      console.log(“x”);
      y();
    };
    x(function y(){
      console.log(“y”);
    })
  ```
## Blocking main thread:
- Since JS is a single threaded language it has a single thread running and has a single call stack. Everything is executed on this call stack only. If any operation blocks this call stack by taking a long time to execute, it will block the call stack since JS is a synchronous language and it will wait for the costly operation to finish first. This is known as blocking the main thread.
- We should always try to execute costly operations (like network calls etc.) in asynchronous manner.

## Event Listners:
- Event listeners are functions that are attached to HTML elements and are designed to respond to specific events triggered by user interactions or other activities. Some of the common event listeners are click, blur, focus etc.
  ```js
    document.getElementById("btn").addEventListener("click", function () {
      console.log("Button clicked");
    });
  ```
 <br/>
 
- This is another use case of callback functions. It will be stored somewhere and as soon as the button is clicked, the function gets into the call stack.
  
  ![image](https://github.com/sachdevavaibhav/js-fundamentals/assets/72242181/7abf0d6a-8a89-4497-8df7-8fc3ba253efa) <br/>
-  One of the most asked interview questions is to maintain a count variable of how many times this button is clicked. One way to solve this is by maintaining a global variable which is not a right approach as someone might change that variable later in the code. <br/> Another way to solve this problem is by creating a closure since they can be used for data hiding/encapsulation. This way the callback will have reference to its lexical environment outside of where it was defined.
  ```js
    function attachEvent() {
      let count = 0;
      document.getElementById("btn").addEventListener("click", function () {
        count++;
        console.log("Button clicked ", count, " times");
      });
    }  
    attachEvent();
   ```
  ![image](https://github.com/sachdevavaibhav/js-fundamentals/assets/72242181/2e45b8f8-ae5a-4eb2-bd18-bf7909bc5fb3)
  
- Event listeners are heavy and can take too much space since they also carry their lexical environment with them. Even if the call stack is empty even then callbacks will not free up space. When there are lots of event listeners present in a website it can cause poor performance and hence it is advised to remove these listeners as soon as their job is done








    


  
        
