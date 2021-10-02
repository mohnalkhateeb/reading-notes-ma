# Inheritance and Interfaces
* ## What Is an Object?
    - Software objects are conceptually similar to real-world objects: they too consist of state and related behavior. 
    - An object stores its state in fields (variables in some programming languages) and exposes its behavior through methods (functions in some programming languages). 
    - Methods operate on an object's internal state and serve as the primary mechanism for object-to-object communication. 
    - Hiding internal state and requiring all interaction to be performed through an object's methods is known as data encapsulation — a fundamental principle of object-oriented programming
    - A software object.
    ![objects](https://docs.oracle.com/javase/tutorial/figures/java/concepts-object.gif)
            
    - A bicycle modeled as a software object.
    ![objectex](https://docs.oracle.com/javase/tutorial/figures/java/concepts-bicycleObject.gif)

    - #### Bundling code into individual software objects provides a number of benefits, including:

        - 1- Modularity: The source code for an object can be written and maintained independently of the source code for other objects. Once created, an object can be easily passed around inside the system.
        - 2- Information-hiding: By interacting only with an object's methods, the details of its internal implementation remain hidden from the outside world.
        - 3- Code re-use: If an object already exists (perhaps written by another software developer), you can use that object in your program. This allows specialists to implement/test/debug complex, task-specific objects, which you can then trust to run in your own code.
        - 4- Pluggability and debugging ease: If a particular object turns out to be problematic, you can simply remove it from your application and plug in a different object as its replacement. This is analogous to fixing mechanical problems in the real world. If a bolt breaks, you replace it, not the entire machine.

* ## What Is a Class?
    A **class** is the blueprint from which individual objects are created.

    ![class-in-java](https://www.w3resource.com/w3r_images/jave-static-class-image.png)

* ## Java Classes
    * ##### Declaring Classes
        - class declarations can include these components, in order:

        - 1- Modifiers such as public, private, and a number of others that you will encounter later. (However, note that the private modifier can only be applied to Nested Classes.)
        - 2- The class name, with the initial letter capitalized by convention.
        - 3- The name of the class's parent (superclass), if any, preceded by the keyword extends. A class can only extend (subclass) one parent.
        - 4- A comma-separated list of interfaces implemented by the class, if any, preceded by the keyword implements. A class can implement more than one interface.
        - 5- The class body, surrounded by braces, {}

        ![classdec](https://i.ytimg.com/vi/i8VwdGMQDp8/maxresdefault.jpg)

    * ##### Access Modifiers
        - ***public*** modifier—the field is accessible from all classes.
        - ***private*** modifier—the field is accessible only within its own class.
* ## Inheritance 
* ### What Is Inheritance?
    - Object-oriented programming allows classes to inherit commonly used state and behavior from other classes.In the Java programming language, each class is allowed to have one direct superclass, and each superclass has the potential for an unlimited number of subclasses.
    ![bycleinh](https://docs.oracle.com/javase/tutorial/figures/java/concepts-bikeHierarchy.gif)

    *  **inheritance Syntax**
    ![inhertancesyntax](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRSLl-frUwuwdBVWQGVUrCeD7GfZ7wbppuwWQ&usqp=CAU)

    - The idea of inheritance is simple but powerful: When you want to create a new class and there is already a class that includes some of the code that you want, you can derive your new class from the existing class. In doing this, you can reuse the fields and methods of the existing class without having to write (and debug!) them yourself.

    - A subclass inherits all the members (fields, methods, and nested classes) from its superclass. Constructors are not members, so they are not inherited by subclasses, but the constructor of the superclass can be invoked from the subclass.

    - ##### The Java Platform Class Hierarchy
    ![hierarchyinher](https://docs.oracle.com/javase/tutorial/figures/java/classes-object.gif)

    - ##### What You Can Do in a Subclass
        - The inherited fields can be used directly, just like any other fields.
        - You can declare a field in the subclass with the same name as the one in the superclass, thus hiding it (not recommended).
        - You can declare new fields in the subclass that are not in the superclass.
        - The inherited methods can be used directly as they are.
        - You can write a new instance method in the subclass that has the same signature as the one in the superclass, thus overriding it.
        - You can write a new static method in the subclass that has the same signature as the one in the superclass, thus hiding it.
        - You can declare new methods in the subclass that are not in the superclass.
        - You can write a subclass constructor that invokes the constructor of the superclass, either implicitly or by using the keyword super.

    - ##### Private Members in a Superclass
        - A subclass does not inherit the private members of its parent class. However, if the superclass has public or protected methods for accessing its private fields, these can also be used by the subclass.

        - A nested class has access to all the private members of its enclosing class—both fields and methods. Therefore, a public or protected nested class inherited by a subclass has indirect access to all of the private members of the superclass.

    - #### Casting Objects
    ![casting](https://media.geeksforgeeks.org/wp-content/uploads/20210119153851/UpcastingVsDowncasting.jpg)


* ## Interfaces
* ### What is the Interfaces?
    - There are a number of situations in software engineering when it is important for disparate groups of programmers to agree to a "contract" that spells out how their software interacts. Each group should be able to write their code without any knowledge of how the other group's code is written. Generally speaking, interfaces are such contracts.
    - In the Java programming language, an interface is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods. Interfaces cannot be instantiated—they can only be implemented by classes or extended by other interfaces. Extension is discussed later in this lesson.

    ![interface](https://appdividend.com/wp-content/uploads/2019/05/How-to-implement-Java-Interface.png)

    - ##### Interfaces as APIs
        - The robotic car example shows an interface being used as an industry standard Application Programming Interface (API). APIs are also common in commercial software products. Typically, a company sells a software package that contains complex methods that another company wants to use in its own software product. An example would be a package of digital image processing methods that are sold to companies making end-user graphics programs. The image processing company writes its classes to implement an interface, which it makes public to its customers. The graphics company then invokes the image processing methods using the signatures and return types defined in the interface. While the image processing company's API is made public (to its customers), its implementation of the API is kept as a closely guarded secret—in fact, it may revise the implementation at a later date as long as it continues to implement the original interface that its customers have relied on

* ## Package
* #### What Is a Package?
    - A package is a namespace that organizes a set of related classes and interfaces. Conceptually you can think of packages as being similar to different folders on your computer.
    - The Java platform provides an enormous class library (a set of packages) suitable for use in your own applications. This library is known as the "Application Programming Interface", or "API" for short. Its packages represent the tasks most commonly associated with general-purpose programming
    - The Java Platform API Specification contains the complete listing for all packages, interfaces, classes, fields, and methods supplied by the Java SE platform. Load the page in your browser and bookmark it. As a programmer, it will become your single most important piece of reference documentation.