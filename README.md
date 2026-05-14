# Saber Foundry

> Work in Progress. Not in a usable state.

Saber Foundry lets you build software using flowcharts. Its a Visual Programming Tool heavily inspired from Unreal Engine Blueprints system,  Excalidraw and my own system design diagrams. 

Its also a Polyglot tool, meaning the nodes of your flowchart can be in any (supported) language, and they will seamlessly exchange data with other nodes even if they're written in a different language, with as little overhead as possible. It manages this through using Protocol Buffers (protobuf). Any inter-language communication is standardized through a .proto definition.

All flowcharts are Excalidraw flowcharts, so all your work is stored in .json format. No tool/vendor lock in, which is core to the philosophy of Saber.