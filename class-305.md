# Putting it all together

* ### How would you break a mock into a component heirarchy?
    Using the same techniques for deciding if you should create a new function or object. One such technique is the single responsibility principle,

* ### What is the *single responsibility principle* and how does it apply to components?
    single responsibility principle is, a component should ideally only do one thing. If it ends up growing, it should be decomposed into smaller subcomponents.

    Since you’re often displaying a JSON data model to a user, you’ll find that if your model was built correctly, your UI (and therefore your component structure) will map nicely. That’s because UI and data models tend to adhere to the same information architecture. Separate your UI into components, where each component matches one piece of your data model.

* ### What does it mean to build a ‘static’ version of your application?
    - To build a ‘static’ version of your application this is building a version that takes your data model and renders the UI but has no interactivity.
    - To build a static version of your app that renders your data model, you’ll want to build components that reuse other components and pass data using props. props are a way of passing data from parent to child. If you’re familiar with the concept of state, don’t use state at all to build this static version. State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don’t need it.
    - You can build top-down or bottom-up. That is, you can either start with building the components higher up in the hierarchy (i.e. starting with FilterableProductTable) or with the ones lower in it (ProductRow). In simpler examples, it’s usually easier to go top-down, and on larger projects, it’s easier to go bottom-up and write tests as you build.

* ### Once you have a static application, what do you need to add?
    Adding the **Inverse Data Flow** that allows us to send data between parent and child components as props, or properties. However, components that are cousins or siblings cannot directly communicate with each other.

* ### What are the three questions you can ask to determine if something is state?
    - ##### 1- Is it passed in from a parent via props? If so, it probably isn’t state.
    - ##### 2-Does it remain unchanged over time? If so, it probably isn’t state.
    - ##### 3-Can you compute it based on any other state or props in your component? If so, it isn’t state.

* ### How can you identify where state needs to live?

    - React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. This is often the most challenging part for newcomers to understand, so follow these steps to figure it out:

    - ##### For each piece of state in your application:

        - **1- Identify every component that renders something based on that state.**
        - **2- Find a common owner component (a single component above all the components that need the state in the hierarchy).**
        - **3- Either the common owner or another component higher up in the hierarchy should own the state.**
        - **4- If you can’t find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common owner component.**