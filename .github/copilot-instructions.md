# AI Agent Instructions for Wargame Engine

This document provides essential context for AI agents working with the Wargame Engine codebase.

## Architecture Overview

The Wargame Engine is a 2D game engine built on Pygame with these key components:

- **Node System**: Scene graph architecture where everything is a node (`BaseNode` -> `ImageNode` -> specialized nodes)
  - Core file: `wargame/nodes.py`
  - Example: `ImageNode` for sprites, `GuiNode` for UI elements

- **Scene Management**: Scenes contain and manage collections of nodes
  - Core file: `wargame/scene.py`
  - Handles drawing, updates, and event routing

- **Message System**: Event-driven communication between components
  - Core files: `wargame/events.py`, `wargame/message.py`
  - Example: `NodeTransmit`/`NodeReceive` pattern in `wargame-examples/06_node_message/`

- **GUI System**: Windowing and container system
  - Core files: `wargame/gui/nodes.py`, `wargame/gui/containers.py`
  - Uses composition: `Window` contains `Container` contains `GuiNode`s

## Key Development Patterns

1. **Scene Creation Flow**:
```python
controller = wargame.engine.init(resources_path)
scene = Scene([node1, node2, ...])
controller.add_scene('scene_name', scene)
controller.run('scene_name')
```

2. **Node Communication**: Use message passing instead of direct references
```python
# Sender:
self.message_id = MessageType.get_new()
MessageSystem.send(Message(self.message_id))

# Receiver:
self.messages.append(message_id)  # Register for message
def handle(self, message):        # Handle in callback
```

3. **GUI Layout**: Use containers for layout management
```python
container = VerticalContainer([node1, node2], background=(214, 214, 214))
window = Window(container)
```

## Project Conventions

1. **Resource Loading**: Always use the `Resources` class for loading assets
   - Example: `Resources.load_image()` instead of direct Pygame calls

2. **Node Hierarchy**:
   - `BaseNode`: Non-visual base class
   - `ImageNode`: Base for visible elements
   - `GuiNode`: Base for UI elements
   - Custom nodes should inherit from appropriate base

3. **Scene Management**:
   - One scene active at a time
   - Scenes handle their own event processing
   - Use `Scene.add_node()` for dynamic additions

## Examples Directory Structure

Key example files demonstrating core concepts:
- Basic setup: `test.py`
- UI elements: `wargame-examples/11_button/button.py`
- Messaging: `wargame-examples/06_node_message/node-message.py`
- Containers: `wargame-examples/08_containers_simple/containers.py`

Reference these examples when implementing similar functionality.