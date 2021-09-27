# Maps, primitives and  File I/O
## Java Primitives versus Objects
* #### Java Type System
    Java has a two-fold type system :
    - 1- primitives such as ***int***, ***boolean***
    - 2- reference types such as ***Integer***, ***Boolean***
* #### Pros and Cons
    * ##### Single Item Memory Footprint

    | Type | primitives size | reference size |
    | ---- | --------------- | --------- |
    | boolean | 1 bit | 128 bits |
    | byte | 8 bit | 128 bit|
    | short, char | 16 bit | 128 bit|
    | int, float | 32 bit | 128 bit |
    | long, double | 64 bit | 192 bit |

    * ##### Memory Footprint for Arrays
    ![memFoot](https://www.baeldung.com/wp-content/uploads/2018/08/plot-memory-bits.gif)

    - We can see either that single-element arrays of primitive types are almost always more expensive (except for long and double) than the corresponding reference type.

    * ##### Performance
        - The performance of a Java code is quite a subtle issue, it depends very much on the hardware on which the code runs, on the compiler that might perform certain optimizations, on the state of the virtual machine, on the activity of other processes in the operating system
        - As we have already mentioned, the primitive types live in the stack while the reference types live in the heap. This is a dominant factor that determines how fast the objects get be accessed.

        ![performance](https://www.baeldung.com/wp-content/uploads/2018/08/plot-benchmark-primitive-wrapper-3.gif)

    * #### Default Values
        - the primitive types may acquire values only from their domains, while the reference types might acquire a value (null) that in some sense doesn't belong to their domains.
        - when a primitive type variable has a value that is equal to its type default one, we should find out whether the variable has been really initialized.
        - There's no such a problem with a wrapper class variables since the null value is quite an evident indication that the variable hasn't been initialized.
    * #### Usage
        - On the other hand, current Java language specification doesn't allow usage of primitive types in the parametrized types (generics),  in the Java collections or the Reflection API.

        - When our application needs collections with a big number of elements, we should consider using arrays with as more “economical” type as possible, as it's illustrated on the plot above.

* ## Exceptions
    - An exception is an event, which occurs during the execution of a program, that disrupts the normal flow of the program's instructions.
    - Creating an exception object and handing it to the runtime system is called throwing an exception.
    - The set of possible "somethings" to handle the exception is the ordered list of methods that had been called to get to the method where the error occurred. The list of methods is known as the call stack
    - The runtime system searches the call stack for a method that contains a block of code that can handle the exception. This block of code is called an exception handler.
    ![callstack](https://docs.oracle.com/javase/tutorial/figures/essential/exceptions-errorOccurs.gif)

    * #### Kinds of Exceptions
        - 1- the checked exception. These are exceptional conditions that a well-written application should anticipate and recover from. 
        - 2-  the error. These are exceptional conditions that are external to the application, and that the application usually cannot anticipate or recover from. 
        - 3- the runtime exception. These are exceptional conditions that are internal to the application, and that the application usually cannot anticipate or recover from. These usually indicate programming bugs, such as logic errors or improper use of an API. 
    * #### The throw Statement
    ![threw](https://i.stack.imgur.com/elekj.png)
    * #### The try-with-resources Statement
        -  A resource is an object that must be closed after the program is finished with
        ![tryst](https://miro.medium.com/max/2526/1*wUzgBj2y2Q6iRxXhAJXkYg.png)

    * ###### If a client can reasonably be expected to recover from an exception, make it a checked exception. If a client cannot do anything to recover from the exception, make it an unchecked exception.

    * #### Advantages of Exceptions
        - 1- Separating Error-Handling Code from "Regular" Code
        - 2- Propagating Errors Up the Call Stack
        - 3- Grouping and Differentiating Error Types

* ## Scanning
    Objects of type Scanner are useful for breaking down formatted input into tokens and translating individual tokens according to their data type.
    
    ![scanner](https://appdividend.com/wp-content/uploads/2019/05/Scanner-Class-in-Java-Tutorial-With-Example.png)