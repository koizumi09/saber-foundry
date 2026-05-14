# Core Systems

## Language Worker
This is the core process responsible for execution of code.

- Language Workers for each language used in a Graph will start at the beginning of the run.
- Each worker will wait until the control arrives at a node of its language.
- It will then map the protobuf stream to the inputs of the node.
- It will execute the node and map the outputs to the protobuf steam.

## Island Fusion System
When multiple nodes of the same language are to be executed in sequence, the Island Fusion System will come into play. 

- Its responsible for fusing all the nodes based on their inputs and outputs.
- It will consolidate all logic in one fused node. 
	- Inputs will be -
		- Inputs of the first node.
		- Inputs of any following nodes that depend on a protobuf stream (i.e the value comes from a node of a different language.
	- Outputs Will be -
		- Outputs of the last node.
		- Outputs of any node that map to a protobuf stream.
- If a graph is monolithic, this system is responsible for consolidating all the logic.

## Node Finder
This core process is responsible for populating the list of usable nodes. 

- It goes through all the scripts in `/nodes` and its subfolders and look for node decorators in function docstrings.
- It catalogs the functions and their metadata in `nodes.saber
- `nodes.saber` can also have node signatures manually added.
	- Adding nodes this way are not restricted to the `/nodes` folder. The script can be located anywhere in the project directory, as long as its URL is included.
	- Idea behind that is sometimes you might want to avoid making any edits to a script but still use its functions as nodes. 
	- You might also want a custom file structure for the project, and this allows you to still be able to use the nodes.

## Protocol Buffers
A protobuf standard definition will contain all the data types required for communication between nodes of different languages.

- If a graph is not monolithic, a process will go through all the nodes and generate a protobuf definition based on their inputs and outputs.

## Graph Structure
A 'graph' in this system is just a directory containing all the data required to construct and run the logic.

- `graph.excalidraw` contains a .json mapping of the graph.
	- Contains the excalidraw mapping of the graph.
- `literals.saber` contains a .json of all the global literals for the graph.
	- Can contain struct instances as well.
- `nodes.saber` contains a .json of all the node signatures.
	- #TODO Think of a system for including libraries

## User Interface
Think of Unreal Engine Blueprints Interface with a Left Panel, a Right panel and a canvas in the middle.

### Left Panel 
Contains Interfaces for - 
- Adding or removing graph variables. Variable can be dragged into the canvas to use.
- Adding or removing custom node graphs. Essentially, create a node that is just another graph nested within this graph. Can be dragged into the canvas.

### Right Panel
Contains Interfaces for -
- Editing graph variables.
	- Data types
	- Values
	- Names
	- Aliases
	- Colors(?)
- Editing custom nodes
	- Names
	- Aliases
	- Colors(?)
- It shows the details for the currently selected node.
	- Can be either in the Left Panel or Canvas.

### Canvas
Just an excalidraw rendering of the `graph.excalidraw`.
- Extra custom functionality based on nodegraph structure of the diagram.
	- Nodes.
		- Keybind to go into add mode.
			- Focus shifts to a text input.
			- Use this input to search node by name or alias.
	- Connectors.
		- Connector snapping.
		- Connectors without a complete connection should be highlighted or auto deleted.
		- Refuse to draw connectors between incompatible pins.


## Error Handling
The system will have 2 layers of Error Handling

1. Every node has a failure exec pin, along with a failure data pin.
	- This lets each node internally handle errors in its native language.
	- Explicit.
2. The Worker on runtime catches runtime errors on specific nodes.
	- Worker's health is monitored. If it crashes or freezes, the system restarts it and reports the error.

## Race Condition Handling
If multiple threads try to modify the same global variable, we'll end up in a race condition.

- When a global variable is accessed by more than one parallel threads, it should be turned 'Atomic'.
- If a non-commutative operation is performed on the variable, the operation is marked as a logic hazard and a warning is shown.

## Environment Management
What compiler binaries should be used.

- Automatic Detection of any environments in the root of the project folder.
- User can explicitly declare which environments to use in `environments.json`

## CLI
Need a CLI to make it container friendly.

- A system just without the UI. (headless)
- A container configured with this.

