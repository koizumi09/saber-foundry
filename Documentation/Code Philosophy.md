## 1. The Priority of the Spatial
**Humans are spatial animals.** Logic is rarely linear, yet text-based code forces it into 1D sequences.

- If a feature can be represented visually without losing precision, it _must_ be visual.
- When adding features to the UI, prioritize Excalidraw-native behaviors (drag-and-drop, grouping, arrows) over nested menus or hidden toggles.

## 2. Prioritize Explicitness
Inspired by Rust and the Zen of Python, Saber avoids "magical" hidden behaviors.

- Data flow and execution flow must be physically drawn. No hidden global events, no "ghost" variables.
- Avoid adding "automatic" features that move data behind the scenes. If data moves from Node A to Node B, there should be a wire or a Portal connecting them.

## 3. Polyglot by Design, Monolithic by Optimization
Saber is a bridge, not a silo. We embrace every language for its strengths (Python for AI, Rust for Performance, Node for Web).

- The **Orchestrator** is language-agnostic. The **Language Worker** is the only place where language-specific "opinions" live.
- When writing core systems, use **Protobuf** as the source of truth. Never assume the "next node" is written in the same language as the current one.

## 4. The "No Vendor Lock-in" Covenant
Your logic belongs to you, not the tool.

- Every Saber project must remain readable and editable as a standard `.json` (Excalidraw) and `.proto` file.
- Do not use proprietary binary formats. If you add a new metadata type, it must be human-readable in the `.saber` files.
- Note: `.saber` is just `.json` renamed.

## 5. Performance as a "First-Class Citizen"
Visual programming should not be "slow" programming.

- Aim for the most optimized path to a problem without sacrificing on the spatial nature.
- Under the hood, things should always be as fast as possible.

## 6. Keyboard Only Inclusive Design
We don't want Saber to feel slow due to constant need to move from mouse to keyboard.

- As much as possible, every feature should be fully usable with just keyboard.
- Prioritize VIM-style keybinds for moving elements and navigating between them.