# Error Handling Debugging
### Execution Contexts and Variable Scope
* Every statement in a script lives in one of three
execution contexts:
  * 1- **Global Context** Code that is in the script, but not in a function. There is only one global context in any page.
  * 2- **Function Context** Code that is being run within a function. Each function has its own function context.
  * 3- **Eval Context** Text is executed like code in an internal function called eva l {) (which is not covered in this book).

* The first two execution contexts correspond with the notion of scope
  * 1- **Global Scope** If a variable is declared outside a function, it can be used anywhere because it has global scope. If you do not use the var keyword when creating a variable, it is placed in global scope.
  * 2- **Function-Level Scope** When a variable is declared within a function, it can only be used within that function. This is because it has function-level scope.
## The Stack
**Stack** is a data structure that follows a simple rule. Last-in, first-out, or LIFO. In Javascript,  Functions are stack on top of each other until the function returns and functions will start popping off the stack.
![stack](https://res.cloudinary.com/practicaldev/image/fetch/s--o4oEkzug--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/hmx76lefmzylwpnqffge.png)

## Error Objects
  - If a JavaScript statement generates an error, then it throws an exception. At that point, the interpreter stops and looks for exception-handling code.
  - **Error objects** can help you find where your mistakes are and browsers have tools to help you read them.
  - When an Er ror object is created, it will contain the following properties:

    | PROPERTY | DESCRIPTION |
    | -------- | ----------- |
    | name | Type of execution |
    | message | Description |
    | fileNumber | Name of the JavaScript file |
    | lineNumber | Line number of error |

  - There are seven types of built-in error objects in JavaScript. You'll see them on the next two pages:

    | OBJECT | DESCRIPTION |
    | ------ | ----------- |
    | Error | Generic error - the other errors are all based upon this error |
    | Syntax Error | Syntax has not been followed |
    | ReferenceError | Tried to reference a variable that is not declared/within scope |
    | TypeError | An unexpected data type that cannot be coerced |
    | Range Error | Numbers not in acceptable range |
    | URI Error | encodeURI ().decodeURI(),and similar methods used incorrectly |
    | EvalError | eva l () function used incorrectly |





    