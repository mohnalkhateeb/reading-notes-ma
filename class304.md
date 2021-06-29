# React and Forms

## React Docs - Forms
* ####  What is a ‘Controlled Component’?
    “controlled component” is a technique to controlle by  combining the two by making the React state be the “single source of truth”. Then the React component that renders a form also controls what happens in that form on subsequent user input. 

    ![control](https://miro.medium.com/max/2178/1*lPa4yO4rACiK244hdyRYXw.png)

* #### Should we wait to store the users responses from the form into state when they submit the form OR should we update the state with their responses as soon as they enter them? Why
    we should update the state with users responses as soon as they enter them Since the value attribute is set on our form element, the displayed value will always be this.state.value, making the React state the source of truth. Since handleChange runs on every keystroke to update the React state, the displayed value will update as the user types

* #### How do we target what the user is entering if we have an event handler on an input field?
    using event.target in the event handelfunction. firstly, we Create the state to hold the input value. then, we Define the event handler to update the state when the user types into the input and finally, we Attach the event handler and set value attribute on the input field
## The Conditional (Ternary) Operator Explained

* #### Why would we use a ternary operator?
    we use a ternary operator to specify that a certain block of code should be executed if a certain condition is met.

* #### Rewrite the following statement using a ternary statement:
    ***if(x===y){
    console.log(true);
    } else {
    console.log(false);
     }***

    - (x===y) ? console.log(true) : console.log(false)