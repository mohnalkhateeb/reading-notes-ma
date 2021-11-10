# Graphs
A graph is a non-linear data structure that can be looked at as a collection of `vertices` (or `nodes`) potentially connected by line segments named `edges`.

####  some common terminology used when working with Graphs:

- **Vertex** : A vertex, also called a “node”, is a data object that can have zero or more adjacent vertices.
- **Edge** : An edge is a connection between two nodes.
- **Neighbor** : The neighbors of a node are its adjacent nodes, i.e., are connected via an edge.
Degree - The degree of a vertex is the number of edges connected to that vertex.

### Directed vs Undirected
##### Undirected Graphs
An Undirected Graph is a graph where each edge is undirected or bi-directional. This means that the undirected graph does not move in any direction.
![undirectedG](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/UndirectedGraph.PNG)

- Vertices/Nodes = {a,b,c,d,e,f}

- Edges = {(a,c),(a,d),(b,c),(b,f),(c,e),(d,e),(e,f)}

##### Directed Graphs (Digraph)
- A Directed Graph also called a **Digraph** is a graph where every edge is directed.

- Unlike an undirected graph, a Digraph has direction. Each node is directed at another node with a specific requirement of what node should be referenced next.

![Dgragh](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/DirectedGraph.PNG)

- Vertices = {a,b,c,d,e,f}

- Edges = {(a,c),(b,c),(b,f),(c,e),(d,a),(d,e)(e,c)(e,f)}

### Complete vs Connected vs Disconnected
##### Complete Graphs
A complete graph is when all nodes are connected to all other nodes.
![complete](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/CompleteGraph.PNG)

##### Connected
A connected graph is graph that has all of **vertices**/**nodes** have at least one edge
![con](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/ConnectedGraph.PNG)

##### Disconnected
A disconnected graph is a graph where some vertices may not have edges.
![dis](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/DisconnectedGraph.PNG)


### Acyclic vs Cyclic
##### Acyclic Graph
An acyclic graph is a directed graph without cycles.A cycle is when a node can be traversed through and potentially end up back at itself.

![Acyclic](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/threeAcyclic.png)

##### Cyclic Graphs
A Cyclic graph is a graph that has cycles. A cycle is defined as a path of a positive length that starts and ends at the same vertex.
![cyc](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/cyclic.PNG)

### Graph Representation
We represent graphs through:

* Adjacency Matrix

![mgraph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/AdjMatrix.PNG)

* Adjacency List

![listGra](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/AdjList.PNG)


### Weighted Graphs
A weighted graph is a graph with numbers assigned to its edges. These numbers are called weights. This is what a weighted graph looks like
![WGra](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/weightGraph.PNG)

![WMGragh](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/weightMatrix.PNG)

![WListGra](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/weightList.PNG)

## Traversals
#### Breadth First
- Here is what the algorithm breadth first traversal looks like:

    - **Enqueue** the declared start node into the Queue.
    - **Create** a loop that will run while the node still has nodes present.
    - **Dequeue** the first node from the queue
    - if the **Dequeue‘d** node has unvisited child nodes, add the unvisited children to **visited** set and insert them into the queue.

![breadth](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/BreadthFirst.PNG)

#### Depth First

- The algorithm for a depth first traversal is as follows:

    - **Push** the root node into the stack
    - Start a while loop while the stack is not empty
    - **Peek** at the top node in the stack
    - If the top node has unvisited children, mark the top node as visited, and then **Push** any unvisited children back into the stack.
    - If the top node does not have any unvisited children, **Pop** that node off the stack
    - repeat until the stack is empty.

![depth](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/Depth1.PNG)

![depth2](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/depthTrav5.PNG)

### Real World Uses of Graphs

* GPS and Mapping
* Driving Directions
* Social Networks
* Airline Traffic
* Netflix uses graphs for suggestions of products