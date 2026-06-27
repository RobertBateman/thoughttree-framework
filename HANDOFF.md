# Thought Tree Framework — Handoff Note

## Purpose of this handoff

This repository is a public handoff of the Thought Tree Framework.

Thought Tree is an open framework for describing complex LLM-assisted work as structured, inspectable, reusable cognitive programs. It is intended to move LLM use beyond isolated prompts, informal prompt chains and opaque agent loops toward explicit cognitive workflows made from:

- Data Units;
- Operations;
- Modules;
- Collections;
- Iterators;
- semantic contracts;
- execution traces;
- and a Cognitive Engine that compiles and executes the workflow.

The central abstraction is:

```text
Inputs → Transformation → Outputs
```

Applied recursively, this becomes:

```text
Data Units → Operations → Data Units
```

A Thought Tree Module defines what cognitive process should happen. A Cognitive Engine determines how that process is compiled, executed, validated, traced and stored.

This handoff exists because I can no longer carry the project forward myself, but I believe the idea has value and should be made freely available for others to use, critique, fork, implement, extend or steward.

---

## Why I am stepping back

I have taken this project as far as I reasonably can.

I am stepping back because of several overlapping constraints:

- being a new father;
- overwhelm, burnout and recovery;
- loss of sustained project momentum;
- dyslexia making long-form documentation especially difficult;
- and the fact that the project now needs technical, organisational and ecosystem-building skills beyond my current capacity.

The project has become bigger than me. I believe I have broken the back of the conceptual work, but taking it from framework draft and prototypes to a mature implementation, standard or ecosystem is no longer my domain.

I do not want the idea to remain trapped with me.

This release is intended to give the project a chance to continue without depending on my ongoing involvement.

---

## What Thought Tree is trying to do

The Thought Tree Framework treats LLM-assisted work as a form of **cognitive programming**.

A cognitive program does not merely ask a model for an answer. It defines a structured process for producing, reviewing, validating and improving an answer.

Instead of:

```text
User → Prompt → LLM → Output
```

Thought Tree aims for:

```text
Inputs
↓
Transformations
↓
Intermediate Artefacts
↓
Review / Validation
↓
Final Outputs
↓
Execution Trace
```

The framework is built around the idea that many valuable LLM workflows require:

- explicit structure;
- intermediate artefacts;
- modular decomposition;
- repeatable execution;
- dependency management;
- validation;
- review;
- audit trails;
- versioning;
- error handling;
- and separation between the workflow definition and the model used to execute it.

Thought Tree attempts to provide a program model for that kind of work.

---

## Core concepts

The framework currently defines the following core concepts.

### Data Units

A Data Unit is a discrete artefact used or produced by a workflow.

Examples include:

- source documents;
- summaries;
- requirements registers;
- plot outlines;
- character profiles;
- review reports;
- correction plans;
- generated TTML modules;
- validation reports;
- final documents.

In the current TTML draft, Data Units are commonly represented as files, but conceptually they may be any addressable artefact.

### Operations

An Operation transforms one or more input Data Units into one or more output Data Units.

Operations may be executed by:

- an LLM;
- a deterministic function;
- an external tool;
- a submodule;
- a generated module;
- a human reviewer.

### Modules

A Module is a reusable cognitive program.

It declares inputs, variables, iterators, collections, operations and final outputs.

Modules may call other Modules, allowing recursive composition.

### Cognitive Engine

The Cognitive Engine is the compiler and runtime for Thought Tree programs.

It is responsible for:

- loading a source definition such as TTML;
- validating structure;
- resolving variables and references;
- expanding iterators;
- building an execution graph;
- planning execution;
- invoking LLMs, functions, tools, submodules and humans;
- storing intermediate artefacts;
- validating outputs;
- handling errors;
- recording an execution trace.

### TTML

Thought Tree Markup Language, or TTML, is the current draft XML-based serialisation format for Thought Tree Modules.

TTML is not the whole framework. It is one representation of the underlying Thought Tree Program Model.

Future source formats could include YAML, JSON, visual graph editors or LLM-generated module definitions.

### Semantic Contracts

Semantic Contracts define what an output must satisfy to be considered fit for purpose.

A file existing is not enough. For example, a file called `plot_outline.txt` should actually contain a usable plot outline.

Contracts may specify:

- semantic type;
- required sections;
- format requirements;
- completeness criteria;
- quality criteria;
- validation functions;
- LLM review rubrics;
- human approval requirements.

### Execution Trace

An Execution Trace records what happened during a run.

It may include:

- source module version;
- inputs;
- hashes;
- resolved variables;
- iterator expansions;
- operation order;
- model used;
- prompts;
- function calls;
- generated outputs;
- validation results;
- retries;
- errors;
- human decisions;
- final outputs.

The goal is not always perfect deterministic reproduction. The goal is to make the process explicit, inspectable, auditable and improvable.

---

## What has already been built

There are currently two main prototype codebases.

### 1. Early proof-of-concept prototype

This prototype was written in C# using the Unity Game Engine.

It predates TTML.

It used an array of hardcoded prompts rather than a formal Thought Tree source file.

Its purpose was to prove that an LLM workflow could pass text files between operations as variables / artefacts.

The prototype successfully planned, drafted and reviewed a roughly 50,000-word novel from a roughly 500-word user-submitted description.

This demonstrated the practical value of preserving intermediate artefacts and passing them between LLM operations.

### 2. Partial Cognitive Engine prototype

This prototype was also written in C# using the Unity Game Engine.

It began moving toward a more general Cognitive Engine.

Implemented or partially implemented:

- connection to Anthropic;
- connection to OpenAI;
- connection to local LLMs through KoboldCPP;
- execution of a basic `TextCompletion` operation.

Not completed:

- TTML importer;
- stable TTML schema implementation;
- graph compiler;
- iterator expansion;
- collection resolution;
- semantic contracts;
- full trace model;
- robust workspace management;
- production-ready execution.

Work on the importer paused because the TTML structure was still being revised.

---

## What exists in this handoff

This handoff may include some or all of the following:

- the full framework explainer;
- previous framework drafts;
- TTML draft material;
- example workflows;
- prototype repositories;
- architectural notes;
- roadmap;
- glossary;
- commercial application notes;
- and this handoff documentation.

The most complete conceptual description is currently the long-form framework draft:

```text
ThoughtTreeFramework.pdf
```

That document describes:

- the strategic vision of cognitive programming;
- the core model;
- system architecture;
- the Thought Tree Program Model;
- the Cognitive Engine;
- execution semantics;
- semantic contracts and validation;
- the TTML standard;
- authoring guidance;
- examples;
- improvement processes;
- commercial applications;
- roadmap;
- glossary.

---

## What is unfinished

This project is not a finished production framework.

Major unfinished areas include:

- finalising the Thought Tree Program Model;
- stabilising TTML;
- producing a formal TTML schema;
- building a reference Cognitive Engine;
- implementing TTML parsing and import;
- compiling TTML into a transformation graph;
- resolving iterators and collections;
- detecting output collisions;
- defining a standard execution trace format;
- implementing semantic contracts;
- implementing validation gates;
- supporting human review;
- supporting submodule execution;
- supporting DynamicCompletion and ProjectCompletion safely;
- creating conformance tests;
- creating example modules;
- building authoring tools;
- creating a module registry or library;
- writing comparison documents against existing agent/workflow frameworks;
- deciding whether Unity/C# should remain the implementation path or whether a reference engine should be built in another environment such as Python, TypeScript, C#/.NET CLI or Rust.

---

## Suggested next steps for future contributors

The most useful next steps are probably:

### 1. Create a minimal reference engine

A minimal reference engine could:

- load a simple TTML file;
- resolve root-level inputs;
- execute operations in document order;
- support `TextCompletion`;
- support `ExecuteFunction`;
- store intermediate outputs;
- resolve final outputs;
- write a basic execution trace.

This does not need to support every advanced concept immediately.

### 2. Extract the specification from the PDF

The current framework document is large. Future contributors could extract shorter specification documents such as:

- Program Model;
- TTML Draft;
- Execution Semantics;
- Cognitive Engine Requirements;
- Semantic Contracts;
- Execution Trace Format.

### 3. Build a minimal example suite

Useful examples include:

- article summary → review → revision;
- technical design document generation;
- novel development pipeline;
- compliance gap analysis;
- module improvement workflow.

### 4. Define the execution trace schema

A trace format would make the framework much more concrete.

A basic trace could be JSON containing:

- run ID;
- module ID;
- engine version;
- input references;
- operation executions;
- model calls;
- function calls;
- outputs;
- errors;
- validation results.

### 5. Stabilise TTML or create a simpler source syntax

TTML is currently the proposed standard source format because XML is explicit and schema-validatable.

However, future contributors may decide to support a YAML or JSON authoring layer that compiles to the same internal program model.

### 6. Compare the framework with existing tools

Useful comparison targets include:

- LangGraph;
- LangChain;
- AutoGen;
- CrewAI;
- Semantic Kernel;
- Airflow / DAG workflow engines;
- traditional prompt chaining;
- autonomous agent systems.

The project should be positioned clearly and fairly.

Thought Tree is not necessarily a replacement for these tools. It may be better understood as a program model and source format for structured cognitive workflows.

---

## What kind of help would be valuable

Valuable contributors might include:

- LLM application developers;
- workflow engine developers;
- compiler/runtime developers;
- documentation writers;
- XML/YAML/schema designers;
- AI tooling researchers;
- agent framework developers;
- human-computer interaction designers;
- people interested in auditability and AI governance;
- people interested in computational creativity;
- people interested in technical documentation automation;
- people interested in open standards for LLM workflows.

Contributions could include:

- writing;
- editing;
- critique;
- prototyping;
- implementation;
- examples;
- diagrams;
- test cases;
- schema design;
- engine design;
- comparison with existing work;
- independent forks.

---

## Licensing intention

The intention of this handoff is to make the Thought Tree Framework freely available for others to use, implement, adapt, fork, extend and build upon.

Please see the repository licence files for the exact legal terms.

If the repository does not yet contain a licence, that should be fixed before serious reuse.

A suggested approach is:

- code under Apache-2.0 or MIT;
- documentation/specification under CC BY 4.0.

This is not legal advice.

---

## Final note

Thought Tree began as an attempt to make LLM-assisted creative production more structured.

Over time, it became clear that the deeper idea was broader:

> LLM-assisted work can be treated as structured cognitive software.

The aim is not to make LLMs magically correct.  
The aim is to make the process around them explicit, inspectable, reusable, validatable and improvable.

If this idea is useful to you, please take it further.

Fork it. Critique it. Rebuild it. Simplify it. Implement it differently. Compare it with other frameworks. Use only the parts that are valuable.

My part in this project is done for now. I am releasing it so that the idea has a chance to continue.
