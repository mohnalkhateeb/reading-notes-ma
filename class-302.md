# React : State and Props and Component Lifecycle Events

* ## React : Component Lifecycle Events
* ### What are component lifecycle events?
lifecycle events are the methods that you are able to use on these are called . These methods can be called during the lifecycle of a component, and they allow you to update the UI and application states.

![Component Lifecycle Events](https://miro.medium.com/max/2800/0*0saPKFiTUk6W3FYp)

* #### what happens first, the ‘render’ or the ‘componentDidMount’?
  from the chart we note the render then ‘componentDidMount’

* #### What is the very first thing to happen in the lifecycle of React?
 constructor() : The constructor for a React component is called before it is mounted.If the component is a subclass you should call super(props), or the props will be undefined. constructors can be used to assign state using this.state or to bind event handle methods to an instance.

* #### what are the phases of component lifecycle?
 - **1-Mounting :** When an instance of a component is being created and inserted into the DOM it occurs during the mounting phase. Constructor, static getDerivedStateFromProps, render, componentDidMount, and UNSAFE_componentWillMount all occur in this order during mounting.

 - **2-Updating :** Anytime a component is updated or state changes then it is rerendered. These lifecycle events happen during updating in this order. static getDerivedStateFromProps, shouldComponentUpdate, render, getSnapshotBeforeUpdate, componentDidUpdate, UNSAFE_componentWillUpdate, UNSAFE_componentWillReceiveProps

 - **3- Unmounting :** The final phase of the lifecycle if called when a component is being removed from the DOM. componentWillUnmount is the only lifecycle event during this phase. 

* #### What does componentDidMount do?
    ComponentDidMount is invoked immediately after a component is mounted. This method is a good place to set up any subscriptions.

* ## State and Props
* #### What types of things can you pass in the props?
    Props (aka "properties") in React allows us to pass values from a parent component down to a child component. The values can be any data type, from strings to functions, objects, etc.

* #### What is the big difference between props and state?
    props (short for “properties”) and state are both plain JavaScript objects. While both hold information that influences the output of render, they are different in one important way: props get passed to the component (similar to function parameters) whereas state is managed within the component (similar to variables declared within a function).

* #### When do we re-render our application?
    - React components automatically re-render whenever there is a change in their state or props. A simple update of the state, from anywhere in the code, causes all the User Interface (UI) elements to be re-rendered automatically.
    - However, there may be cases where the render() method depends on some other data. After the initial mounting of components, a re-render will occur when:
    1- A component’s setState() method is called.
    2-A component’s forceUpdate() method is called.
* #### What are some examples of things that we could store in state?
    counters, date, flages and any updated thing throughing the code 

## Things I want to know more about
state and prop 