# Stacks and Queues
## Stack
* ### What is a Stack
    A stack is a data structure that consists of Nodes. Each Node references the next Node in the stack, but does not reference its previous.
* ### Common terminology for a stack is
    - 1- **Push** - Nodes or items that are put into the stack are pushed
    - 2- **Pop** - Nodes or items that are removed from the stack are popped. When you attempt to pop an empty stack an exception will be raised.
    - 3- **Top** - This is the top of the stack.
    - 4- **Peek** - When you peek you will view the value of the top Node in the stack. When you attempt to peek an empty stack an exception will be raised.
    - 5- **IsEmpty** - returns true when stack is empty otherwise returns false.

* ### Stack Concepts 
    - **FILO** : First In Last Out
    - **LIFO** : Last In First Out

* ### Stack Visualization
    ![stackVirtual](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/stack1.PNG)

* ### Push O(1)
    - First, you should have the Node that you want to add. Here is an example of a Node that we want to add to the stack
    

    - Next, you need to assign the next property of new node to reference the same Node that top is referencing: old node 
    ![push1-2](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/pushStack2.PNG)

    - Technically at this point, your new Node is added to your stack, but there is no indication that it is the first Node in the stack.
    ![push3](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/pushStack3.PNG)

* ### Pop O(1)
    ![pop](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/popStack3.PNG)

## Queue
* ### What is a Queue
    a queue is a collection of entities that are maintained in a sequence and can be modified by the addition of entities at one end of the sequence and the removal of entities from the other end of the sequence
* ### Common terminology for a queue is

    - **Enqueue** - Nodes or items that are added to the queue.
    - **Dequeue** - Nodes or items that are removed from the queue. If called when the queue is empty an exception will be raised.
    - **Front** - This is the front/first Node of the queue.
    - **Rear** - This is the rear/last Node of the queue.
    - **Peek** - When you peek you will view the value of the front Node in the queue. If called when the queue is empty an exception will be raised.
    - **IsEmpty** - returns true when queue is empty otherwise returns false.
* ### Queues concepts:
    - **FIFO** : First In First Out
    - **LILO** : Last In Last Out

* ### Queue Visualization
    ![Qvisual](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/Queue.PNG)

* ### Enqueue O(1)
    ![enQ1](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/Enqueue1.PNG)

    ![enQ2](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/Enqueue2.PNG)
    ![enQ3](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/Enqueue3.PNG)

* ### Dequeue O(1)
    ![deQ1](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/Dequeue1.PNG)
    ![deQ2](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-10/resources/images/Dequeue3.PNG)