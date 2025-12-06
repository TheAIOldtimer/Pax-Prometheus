
Markdown

# Cognite: The Language of Intent

### Overview

**Cognite** is a conceptual, intent-driven programming language designed to be the foundational layer for advanced artificial intelligence systems. It represents a paradigm shift from traditional, imperative programming (telling a computer *how* to do something) to cognitive programming (telling an AI *what* to achieve).

The core motto of Cognite is: **"Describe the thought, not the process."**

Instead of writing step-by-step instructions, a developer in Cognite defines `Concepts`, sets `Goals`, and establishes `Policies`. A powerful, AI-native runtime then generates a `Plan` to achieve those goals, seeks human approval for sensitive actions, and executes the plan, interacting with both digital and physical systems.

### Relationship to Pax-Prometheus

Cognite is the enabling technology that makes the **Pax-Prometheus** governance framework a practical reality.

*   **Prometheus (The Planner AI)** is implemented by the **Cognite Runtime**. This is the engine that translates a high-level `Goal` into a concrete, executable `Plan`.
*   **Pax (The Human Arbiter)** is implemented by Cognite's **Safety & Policy Layer**. This layer checks every plan against a `policy.json` file and triggers a `pax_approval_required` workflow for any action deemed sensitive, pausing execution until a human gives explicit consent.

In short, Pax-Prometheus defines the *philosophy* of safe AI governance, and Cognite provides the *tools* to build it.

### Core Principles

1.  **Intent-First:** Programs describe desired end-states (`Goals`), not algorithms.
2.  **Cognitive Primitives:** `Concepts`, `Goals`, `Perception`, and `Schemas` are native language constructs.
3.  **Explainability by Design:** Every action is traceable. The runtime produces a detailed `Plan` and `Trace`, explaining what it's doing and why.
4.  **Safety is Foundational:** A Capability & Policy engine is a non-negotiable, core component of the runtime, implementing the Pax-Prometheus approval loop.
5.  **Hardware Agnostic:** The language abstracts away the underlying hardware (CPU, NPU, etc.), allowing the compiler/runtime to choose the best processor for each task.

### Example: The "Perfect Morning" Program

Below is a conceptual example of a Cognite program to manage a smart home.

```cognite
// perfect_morning.cog

// Define the concepts in our world
singleton concept Owner: Person {
    property state: BiometricState = perceive("owner_sensors");
}

concept SmartHome {
    property bedroom_lights: Light = device("bedroom_light_group");
    property coffee_machine: CoffeeMaker = device("kitchen_coffee_pro");
}

// Instantiate our concepts
let home = new SmartHome();
let owner = get Owner;

// Set a goal triggered by an event
on Time.is("06:30 AM") {
    goal "Wake the owner gently." {
        // Define the desired end-state for the lights
        achieve home.bedroom_lights.state {
            intensity: from 0% to 60%;
            duration: 15 minutes;
            // The runtime will delay the start to align with the owner's light sleep phase
            start_time: align_with(owner.state.sleep_cycle, phase: "light_sleep");
        }
        // Define a dependent goal for the coffee machine
        achieve home.coffee_machine.state {
            status: "brewed";
            // Dependency: Trigger only when the owner gets out of bed
            trigger: on owner.state.is_moving(threshold: "out_of_bed");
        }
    }
    execute(goal);
}

The Python Prototype
To prove the viability of these concepts, a minimal, runnable prototype was created in Python. It implements the core loop: Goal -> Plan -> Policy Check -> Pax Approval -> Execute.
The prototype consists of two main files:

    cognite_runtime.py: The core engine with planning, execution, and safety checks.
    cognite_repl.py: An interactive command-line interface for defining concepts, setting goals, and acting as the "Pax" arbiter for approvals.

Example REPL Session (demonstrating the Pax-Prometheus loop):
Plain Text

cognite> register home
[runtime]: # "Registered concept 'home'."

cognite> show
- home: <Concept home {'front_door_lock': {'status': 'locked'}}>

cognite> goal
Enter goal description:
> Let the dog out
Add achieve targets. Enter empty target to finish.
target> home.front_door_lock
constraints as json> {"status": "unlocked"}
target>

[runtime]: # "Planning for goal: Let the dog out"

=== PLAN ===
- Set home.front_door_lock -> {'status': 'unlocked'}  (confidence=0.90)
    rationale: Satisfy goal 'Let the dog out' by updating home.front_door_lock

[APPROVAL REQUIRED]: # "The system wants to set home.frontdoorlock -> {'status': 'unlocked'}. Reason: Fulfill goal 'Let the dog out'. Proceed? [y/n] y"
[runtime]: # "User approved. Executing step."

=== TRACE ===
{'id': '...', 'goal': 'Let the dog out', 'plan': [...], 'approved': true}

cognite> show
- home: <Concept home {'front_door_lock': {'status': 'unlocked'}}>

This session demonstrates the entire Pax-Prometheus framework in action: a goal was defined, a plan was proposed by the runtime (Prometheus), and it was paused until explicit approval was granted by the human user (Pax).
Plain Text


---
*   This is perfect. Let's rename "human_in_loop" to "pax_approval_required" in the code to match the new documentation.
*   Let's improve the planner. A smarter Prometheus needs better dependency ordering.
*   What's the next logical feature of the Pax-Prometheus framework we should build into Cognite?

