# Graphs
Graphs are the main component of Saber Foundry. 

- A graph is simply a collection of interconnected nodes. 
- A graph can be built using predefined nodes. 
- A graph itself can be abstracted into a node to be used in another graph. 
	- This can be done using special `graph_inputs` and `graph_outputs` nodes.
	- The very same nodes can be used to take command line args or print graph output to console.
- Graphs are primarily polyglot, meaning their nodes can be written in different languages. 
	- However, if a graph only has nodes written in a particular language (Monolithic Graph), the protobuf connectors will be replaced with native function calling, remove the protobuf overhead completely.
- Graphs can have graph-specific literals and struct instances. They're saved separately in a .json.
	- Again, if a Graph is Monolithic, literals and struct instances will become global variables (to the scope of the graph only) for all the nodes to use.

# Nodes
Nodes are essentially functions, with clearly defined inputs and outputs.

- Nodes can be made in any language with protobuf support and Saber's language specific worker. 
	- Language Specific Worker is what is responsible for connecting and running the nodes when you press 'Run'.
	- Worker availability will determine if a language is supported by Saber.
	- Initially, I plan on adding workers for python and rust.
- There are two main ways of turning a function into a node - 
	1. Every graph will have a `NodesDefinition.json` which will contain metadata about each node. 
		- Metadata will contain
			- Node Name
			- Language and Version 
			- Inputs and Outputs with their types
			- URL of the file which contains the function
	2. Scripts can be created in or moved to the `nodes` directory. 
		- All scripts in this directly will be scanned for a docstring decorator. 
		- If the docstring decorator is present, the function will be added to `NodesDefinition.json` automatically.
		- Metadata of the function will be parsed through the docstring and through context (like through the file extension).
		- If metadata is explicitly stated in the docstring, that will take priority over contextual parsing.
- Classes will be handled by turning the methods into a regular node, with an input pin for an instance of the class.
	- Classes will be broken down into methods and Attributes.
	- Attributes will be treated like a regular struct.
	- Methods will be treated like a regular function, but with a reference pin to an instance of the struct of this class's attributes.
	- Static classes will be tread like regular functions.
- There are three ways for a Node to get 'triggered'.
	1. When control reaches its input execution pin (or in-exec pin).
	2. When its output is requested by another node which is currently triggered.
	3. in-exec pins can be set to 'listen'. The particular node will then listen for a specific event, giving a way to implement event-driven programming.
		- If multiple listener nodes are listening for the same event, their priority can be defined in the in-exec pin. 
		- Multiple nodes can have the same priority, in which case they will trigger simultaneously on different threads.
	- When a node is triggered, it will try to see if its input pins are literals.
		- If they're literals, the node will execute using the literals as its inputs.
		- If they're not literals (Eg. They're the output of another function), then the node will trigger the node responsible for calculating the literals recursively until the literal resolves into a value.
- In Saber, everything is a node. Commonly used language libraries will have readily available wrappers to make them usable in Saber. Otherwise, creating a wrapper would be as easy as adding the library functions to `nodes.saber` or creating a wrapper script.
	- We'll look into establishing ways of sharing Saber Libraries so we may all help each other build.
	- Each language will have a bunch of core nodes. Stuff like 'add', 'multiply', 'branch' and all that goods stuff.

# Connectors
Connectors are the connections between nodes. They can either be for control flow or data.

## Flow Control Connectors
These connect the execution pins, or exec pins, of nodes.

- Code execution will always start from either the `Start` node or the `graph_input` node.
- The order of execution of each node after that is determined by how their exec pins are connected.
- Special nodes like `branch` can alter the control flow.
- Special nodes or events can split the flow control into multiple independent threads.
- Multiple threads can merge into one, only continuing the flow after all the threads have merged.

## Data Connectors
These connect to the data pins of the nodes. 

- When a node is triggered, it will look for the value in its input nodes based on the connectors.
- If the value is a literal, that will be used as the input.
- If the value is an output of another node, two things can happen.
	- If the node has an exec pin, and it has already been triggered at least once before, then the output value from that execution will be used.
	- If the node has an exec pin, but hasn't been triggered even once yet, it will result in an error.
	- If the node has no exec pins, it'll be recursively triggered until its output is resolved into a value. 
		- That means if the node's output computation requires an input, it will in turn check if its input is a resolved literal.
			- If it is, it'll use that input to compute its output and the chain will end. 
			- If it is the output of another node, it will trigger that node, traversing the chain of nodes until a value is resolved.
- Data connectors correspond to their types. Two value pins of different types cannot be connected without a type-cast node in the middle.
- Data connectors can be split into multiple connectors for when a value needs to be used multiple times.
	- Data connectors from outputs of nodes with exec pins will carry the last value from when the node was last triggered.
	- Data connectors from outputs of nodes without exec pins will trigger their nodes every time the value is requested.
- Struct data connectors can be split into the struct's individual attributes with a splitter node.

## Routing Portals
Nobody likes spaghetti graphs. You can create portals to route your connectors through. Just try to only use them when absolutely necessary, as they are bad for graph readability.

# Comments
Three types of comments - 
1. Rectangular Area 
	- A comment rectangle can be drawn around nodes. 
	- The Rectangle can have a bound text element which will be the comment text. 
	- The border and fill color of each comment can be modified.
2. Random scribbles or just text 
3. A rectangle comment or a text comment that start with # is a docstring for the node.