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
