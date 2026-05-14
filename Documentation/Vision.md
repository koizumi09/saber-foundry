# What is Saber Foundry?
Saber Foundry lets you build software using flowcharts. Its a Visual Programming Tool heavily inspired from Unreal Engine Blueprints system,  Excalidraw and my own system design diagrams. 

Its also a Polyglot tool, meaning the nodes of your flowchart can be in any (supported) language, and they will seamlessly exchange data with other nodes even if they're written in a different language, with as little overhead as possible. It manages this through using Protocol Buffers (protobuf). Any inter-language communication is standardized through a .proto definition.

All flowcharts are Excalidraw flowcharts, so all your work is stored in .json format. No tool/vendor lock in, which is core to the philosophy of Saber.

# Why?
Core Idea - Humans (likely) evolved intelligence and problem solving ability for two purposes - **Language and Food.** 

- Being able to communicate through language requires intelligence, empathy and other social tools.
- As an omnivore, remembering what to eat and what not to eat, where to find food, how to avoid food's self-defense mechanisms requires intelligence and spatial problem solving ability.

In the current state of Computer Programming, we inscribe logic in linear sequences of characters. Even though logic itself is neither linear and rarely sequential, we've restrained ourselves to writing code in text (1D) for decades now.

- This does leverage the linguistic intelligence inherently.
- However, it completely disregards our Spatial Problem Solving capabilities.
- If you've played a factory builder like Satisfactory or Factorio, you know how intuitive problem solving is when you're thinking in terms of space, instead of a sequence.

While learning, we all start with flowcharts, because they're the most intuitive way of inscribing logic in a way that's easy to read and write. However, as we get into writing code, flowcharts and logic become implicit in our code, but the code itself is sequential.

> Saber Foundry is a tool designed to let you inscribe logic in two dimensions, by letting you code through drawing flowcharts.

---

So far, Visual Programming has seen a very limited real world application. Its mostly used in educational stuff, some niche applications like data science or 3D textures (blender) or in game development. 

Saber Foundry aims to take Visual Programming to production-grade software by providing the tools to build sophisticated, complicated software architecture, while abstracting the actual code into nodes that you can use as elements of a flowchart. The abstraction is powerful enough to let you write the underlying code for a node in any language of your choosing which is currently supported.

Saber is built to be extensible. 

- Any language supported by protobuf can be added to Saber with minimal effort.
- Adding new nodes and libraries - Either write a function in any language from scratch, or add a wrapper around preexisting function to turn it into a node.
- Any existing function can be easily wrapped to work with Saber, since every node is essentially just a function.

# Why 'Foundry'?
Because it sounded cool in my head.