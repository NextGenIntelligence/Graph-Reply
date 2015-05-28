Graph Reply
===========
I have been teaching myself C for a while and Graph Reply is my first attempt at a REPL style program. It stores data in graph form and supports a variety of read/write operations including saving to disk. To run, compile the source code and run the output file. You will be taken to a repl where you can type commands. To close the program enter the command "exit".
```shell
make
./reply
> new mynode
> save mygraph
> exit
```


WRITE DATA
----------
Creates a node with label "person"
```code
> new person 
```

Creates a directed edge from person (source) to manager (target), the nodes are created if they do not exist
```code
> link person manager
```

The command wn (write node) sets the data attribute of a given node.
Here we set the data attribute of person to "John".
```code
> wn person
[(person).data]> John
```

The command wl (write link) sets the data attribute of the edge from person to manager, in this case it is given the value "has job".
```code
> wl person manager
[(person->manager).data]> has job
```

READ DATA
---------
You can use the id or name of a node to get its data with the id and rn (read node) commands respectively.
```code
> id 0
{ id: 0, "name": "person", "data": "John" }
> rn person
{ id: 0, "name": "person", "data": "John" }
```

Use the rl (read link) command to get the edge information for two nodes
```code
> rl person manager
{ "source": "person", "target": "manager", "data": "has job" }
```

The command out returns an array of nodes connected to the given node by outgoing edges.
```code
> out person
[ { "id": 1, "name": "manager" } ]
```

The command in returns an array of nodes connected to the given node by incoming edges.
```code
> in manager
[ { "id": 0, "name": "person" } ]
```

The command getn (get node) asks for data and then returns all nodes with the given data.
```code
> getn
[node.data]> John
[ { id: 0, "name": "person", "data": "John" } ]
```

The command getl (get link) asks for data and then returns a pair of ids where the first id is the source and the second the target of an edge with the given data.
```code
> getl
[link.data]> has job
[ [0, 1] ]
```

The command path takes two nodes as arguments and returns the first path it finds through the graph via outgoing edges from the first node to the second, or returns false if no path exists.
```code
> link fish bird
> link bird cat
> path fish cat
{ "hasPath": true, "path": [ 0, 1, 2 ] }
```

The command all will print every node and edge in the graph in one fell swoop.
```code
> all
{ "nodes": [{ "id": 0, "name": "person", "data": "John" }, { "id": 1, "name": "manager", "data": "NULL" }], "links": [{ "source": 0, "target": "1", "data": "has job" }] }
```

Math
----
Basic arithmetic operations are possible if numbers are stored on the data attribute of nodes, the following examples will suffice to illustrate their usage:
```code
> link a b
> wn a
[(a).data]> 10
> wn b
[(b).data]> 2.5
> add a b
12.500000
> sub a b
7.500000
> mult a b
25.000000
> div a b
4.000000
```
Aliasing Commands
----------------
Commands can be stored on the data property of a node and run using the cmd command.
```code
> link a b
> wn a
[(a).data]> 10
> wn b
[(b).data]> 2.5
> new operation
> wn operation
[(operation).data]> add a b
> cmd operation
12.50000
```
Disk Storage
----
```code
> save my_graph
```
The save command converts the state of the graph into graph-reply commands and saves them to a file with the name given as input. The above command saves the graph in the file my_graph.gph.
```code
> load my_graph
```
The load command loads a previously saved graph into memory from a file.
