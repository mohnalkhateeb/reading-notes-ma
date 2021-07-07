# **Functional Programming**
* #### What is functional programming?
    **Functional programming** is a programming paradigm — a style of building the structure and elements of computer programs — that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data

* #### What is a pure function and how do we know if something is a pure function?
    **pure functions** is function returns the same result if given the same arguments (it is also referred as deterministic) and does not cause any observable side effects
* #### What are the benefits of a pure function?
    The code’s definitely easier to test. We don’t need to mock anything. So we can unit test pure functions with different contexts:
    - Given a parameter A → expect the function to return value B
    - Given a parameter C → expect the function to return value D
* #### What is immutability?
    Unchanging over time or unable to be changed. When data is immutable, its state cannot change after it’s created. If you want to change an immutable object, you can’t. Instead, you create a new object with the new value.
* #### What is Referential transparency?
    if a function consistently yields the same result for the same input, it is referentially transparent.
    - **pure functions + immutable data = referential transparency**

* #### What is a module?
    **Module** in Node.js is a simple or complex functionality organized in single or multiple JavaScript files which can be reused throughout the Node.js application.Each module in Node.js has its own context, so it cannot interfere with other modules or pollute global scope.

* #### What does the word ‘require’ do?
    **require function** is the easiest way to include modules that exist in separate files. The basic functionality of **require** is that it reads a JavaScript file, executes the file, and then proceeds to return the exports object

* #### How do we bring another module into the file the we are working in?
    **var module = require('module_name');**

    As per above syntax, specify the module name in the require() function. The require() function will return an object, function, property or any other JavaScript type, depending on what the specified module returns

* #### What do we have to do to make a module available?

    Use the **exports** keyword to make properties and methods available outside the module file
