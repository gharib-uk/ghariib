---
title: "Godot 4.3+ Hierachical State Machine"
date: 2025-01-22
---

> In this post I am assuming you have basic knowledge of how a State Machine Works

## This is a Hierarchical Finite State Machine!

This means it is setup in the editor as custom nodes!

### The State Class

In this state machine, each state will extend from the `State` class below. Transitions between states can be called with this nice function I made called `transition_to()`. When using this state machine treat the `update` and `physics_update` like `_process` and `_physics_process` respectively.  

```
class_name State
extends Node

signal transitioned(state: State, new_state_name: String)

var player: CharacterBody2D
@onready var sprite: AnimatedSprite2D = %AnimatedSprite2D

func enter() -> void:
    pass

func exit() -> void:
    pass

func update(delta: float) -> void:
    pass

func physics_update(delta: float) -> void:
    pass

func transition_to(new_state_name: String) -> void:
    transitioned.emit(self, new_state_name)

```

### The State Machine

The State Machine node is where we will be adding our State nodes to as children. The state machine mostly takes care of transitions, keeping track of current state, and executes the code of the states.  
**Must be a child of a CharacterBody2D (Aka your player)**  

```

class_name StateMachine
extends Node

@export var initial_state: State
var current_state: State
var states: Dictionary = {}

func _ready() -> void:
    _initialize_states()
    _set_initial_state()

func _initialize_states() -> void:
    for child in get_children():
        if child is State:
            states[child.name.to_lower()] = child
            child.transitioned.connect(_on_state_transition)

func _set_initial_state() -> void:
    if initial_state:
        _assign_player_reference(initial_state)
        initial_state.enter()
        current_state = initial_state
    else:
        push_warning("No initial state set for StateMachine in " + owner.name)

func _process(delta: float) -> void:
    if current_state:
        current_state.update(delta)

func _physics_process(delta: float) -> void:
    if current_state:
        current_state.physics_update(delta)

func _on_state_transition(state: State, new_state_name: String) -> void:
    if state != current_state:
        return

    var new_state = states.get(new_state_name.to_lower())
    if not new_state:
        push_warning("State '" + new_state_name + "' not found in StateMachine")
        return

    _transition_to_new_state(new_state)

func _transition_to_new_state(new_state: State) -> void:
    current_state.exit()
    current_state = new_state
    _assign_player_reference(new_state)
    new_state.enter()

func _assign_player_reference(state: State) -> void:
    var parent = get_parent()
    if parent is CharacterBody2D:
        state.player = parent
    else:
        push_warning("StateMachine's parent must be CharacterBody2D")

```

## Example States and proper usage:

> Write code necessary only for that state.

```

class_name IdleState
extends State

@export_range(120, 420) var friction: float

func enter() -> void:
    print("Entered Idle")
    sprite.play("idle")

func exit() -> void:
    print("Exited Idle")

func update(delta: float) -> void:
    if Input.get_axis("ui_left", "ui_right") != 0:
        transition_to("Walk")
    if not player.is_on_floor():
        transition_to("Fall")

func physics_update(delta: float) -> void:
    if player.velocity.x != 0:
        player.velocity.x = lerp(player.velocity.x, 0, friction * delta)

```

Go to Source
