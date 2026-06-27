# Thought Tree Framework Architecture

This document describes the architecture of the Thought Tree Framework: how Thought Tree source definitions are parsed, compiled, executed, validated and traced.

Documentation, specifications and examples in this repository are released under CC0 unless otherwise stated. Code is released under the MIT License.

---

## 1. Architectural Purpose

The Thought Tree Framework is designed to make complex LLM-assisted work:

- structured;
- inspectable;
- modular;
- reusable;
- model-independent;
- auditable;
- validatable;
- improvable.

The central architectural idea is to separate:

```text
What cognitive work should be done
```

from:

```text
How that work is executed
```

A Thought Tree program defines the intended cognitive process. A Cognitive Engine compiles and executes that process using available execution backends such as LLMs, deterministic functions, tools, submodules and human review.

At a high level:

```text
Thought Tree Source Definition
        ↓
Cognitive Engine
parse → validate → compile → plan → execute → validate → trace
        ↓
Execution Backends
LLMs / deterministic functions / tools / submodules / humans
        ↓
Workspace
inputs / intermediate artefacts / outputs / logs / traces
        ↓
Final Outputs
validated Data Units and execution records
```

The framework should not be understood as a prompt runner. It is intended as a cognitive programming architecture: a way to define repeatable transformations over meaningful artefacts.

---

## 2. Core Architectural Principle

The smallest unit of Thought Tree execution is:

```text
Input Data Units → Operation → Output Data Units
```

A complete Thought Tree Module connects many such transformations into a larger process:

```text
Inputs
  ↓
Operations
  ↓
Intermediate Artefacts
  ↓
Review / Validation
  ↓
Final Outputs
  ↓
Execution Trace
```

Although the framework uses the term “Thought Tree”, the executable structure is often better understood as a directed transformation graph.

In that graph:

- Data Units are artefact nodes;
- Operations are transformation nodes;
- Collections are grouped artefact references;
- Modules are reusable subgraphs;
- Contracts are validation requirements;
- dependencies and provenance are edges;
- traces record execution events.

---

## 3. High-Level Architecture

The Thought Tree Framework can be understood as five architectural layers:

```text
1. Source Layer
2. Program Model Layer
3. Compilation and Planning Layer
4. Execution Layer
5. Workspace and Trace Layer
```

Together these layers allow a human-readable cognitive workflow to become an executable, inspectable process.

---

## 4. Source Layer

The Source Layer contains the human-readable or machine-generated definition of a Thought Tree program.

The current source format is:

```text
TTML — Thought Tree Markup Language
```

TTML is an XML-based serialisation format for Thought Tree Modules.

However, TTML is not the whole framework. It is one representation of the underlying Thought Tree Program Model.

Future source formats may include:

- YAML;
- JSON;
- visual graph editors;
- database-backed module definitions;
- LLM-generated modules;
- domain-specific authoring syntaxes.

The Source Layer should be:

- readable by humans;
- authorable by LLMs;
- parseable by tools;
- suitable for validation;
- stable enough for interchange.

A source definition describes the intended process, but it does not execute the process itself.

Example high-level TTML structure:

```xml
<TTML version="0.12.0">
  <Project ... />

  <Inputs>
    ...
  </Inputs>

  <Vars>
    ...
  </Vars>

  <Iterators>
    ...
  </Iterators>

  <Collections>
    ...
  </Collections>

  <Operations>
    ...
  </Operations>

  <Output>
    ...
  </Output>
</TTML>
```

---

## 5. Program Model Layer

The Program Model Layer is the abstract representation of a Thought Tree program.

It exists independently of TTML or any other source syntax.

The main entities are:

- Module;
- Concept;
- Data Unit;
- Semantic Type;
- Operation;
- Relationship;
- Collection;
- Variable;
- Iterator;
- Contract;
- Workspace;
- Execution Trace.

A Cognitive Engine should convert source definitions into this abstract model before execution.

For example:

```text
TTML <File id="plot_outline" extension="txt"/>
```

becomes a logical Data Unit:

```text
id: plot_outline
physical representation: txt
semantic role: optional, e.g. PlotOutline
contract: optional, e.g. PlotOutlineContract
```

This distinction is important. A source file is syntax. A Data Unit is part of the cognitive program.

---

## 6. Compilation and Planning Layer

The Compilation and Planning Layer is handled by the Cognitive Engine.

The Cognitive Engine takes a source definition and converts it into an executable plan.

A typical compilation process is:

```text
Source Definition
        ↓
Parse
        ↓
Normalise into Program Model
        ↓
Resolve Symbols
        ↓
Resolve Variables
        ↓
Expand Iterators
        ↓
Resolve Collections
        ↓
Build Dependency Graph
        ↓
Validate Execution Requirements
        ↓
Detect Output Collisions
        ↓
Create Execution Plan
```

### 6.1 Parsing

The engine loads the source document and parses it into structured objects.

For TTML, this means reading elements such as:

- `<Project>`;
- `<Inputs>`;
- `<Vars>`;
- `<Iterators>`;
- `<Collections>`;
- `<Operations>`;
- `<Output>`;
- `<File>`;
- `<FileRef>`;
- `<Collection>`;
- `<CollectionRef>`.

### 6.2 Normalisation

The parsed source is converted into the Thought Tree Program Model.

This allows different source formats to compile into the same internal representation.

For example, TTML, YAML and a visual editor could all produce equivalent internal program graphs.

### 6.3 Symbol Resolution

The engine resolves named references, including:

- Data Unit references;
- Collection references;
- variable references;
- iterator references;
- function names;
- target module paths;
- contract names;
- tool names;
- semantic type names, where supported.

If a required reference cannot be resolved, validation should fail before execution unless the program explicitly permits dynamic resolution.

### 6.4 Iterator Expansion

Iterators define repeated execution.

Example:

```xml
<Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
```

If `ChapterCount` is `12`, then operations using `ChapterIterator` may expand into twelve concrete operation instances.

Example output pattern:

```xml
<File id="draft_chapter_{{ChapterIterator}}" extension="txt"/>
```

expands to:

```text
draft_chapter_1
draft_chapter_2
...
draft_chapter_12
```

Iterator expansion should occur before execution so that the engine can detect:

- missing dependencies;
- output collisions;
- excessive execution cost;
- unintended Cartesian products.

### 6.5 Dependency Graph Construction

The engine builds a graph of dependencies between Data Units, Collections, Operations and Modules.

Example:

```text
story_requirements
        ↓
AnalyzeRequirements
        ↓
requirements_analysis
        ↓
DraftOutline
        ↓
plot_outline
```

The dependency graph is used to determine:

- execution order;
- which operations can run in parallel;
- whether dependencies are missing;
- whether final outputs can be produced;
- whether output collisions exist;
- which artefacts should be traced as provenance.

### 6.6 Execution Planning

After validation, the engine creates an execution plan.

The execution plan identifies:

- concrete operation instances;
- operation order;
- resolved inputs;
- expected outputs;
- active iterator values;
- required functions;
- target submodules;
- required model capabilities;
- validation gates;
- review gates;
- failure strategies;
- possible parallel execution.

A mature engine should support dry-run mode:

```text
compile and display execution plan without running LLM calls
```

This is important for expensive, high-impact or dynamically generated workflows.

---

## 7. Execution Layer

The Execution Layer performs the actual work.

A Cognitive Engine may execute operations using:

- LLM providers;
- deterministic functions;
- tools and external systems;
- pre-existing modules;
- generated modules;
- human review.

The LLM is not the engine. The LLM is one execution backend used by the engine.

The engine remains responsible for:

- structure;
- dependency management;
- execution order;
- workspace management;
- output resolution;
- validation;
- error handling;
- trace recording.

---

## 8. Execution Backends

### 8.1 LLM Providers

LLMs are used for ambiguous semantic work.

Examples:

- summarisation;
- analysis;
- drafting;
- synthesis;
- review;
- critique;
- classification;
- extraction;
- planning;
- revision;
- module generation.

A `TextCompletion` operation may be executed by an LLM.

The engine constructs a request using:

- operation description;
- input Data Units;
- Collections;
- variables;
- iterator values;
- output requirements;
- semantic contracts, where supported;
- engine-level system instructions;
- model configuration.

A Thought Tree Module should generally be model-independent. It should describe the required transformation rather than hard-code a specific provider.

### 8.2 Deterministic Functions

Deterministic functions are used for mechanical work better handled by ordinary software.

Examples:

- copy files;
- concatenate files;
- validate XML;
- convert formats;
- generate checksums;
- query a database;
- resize images;
- create archives.

These are usually invoked through `ExecuteFunction` operations.

Example:

```xml
<Operation
  id="CompileNovel"
  type="ExecuteFunction"
  desc="Concatenate revised chapters into one manuscript."
  function="ConcatenateFiles">
  <CollectionRef id="all_revised_chapters"/>
  <Output>
    <File id="compiled_novel_manuscript" extension="txt"/>
  </Output>
</Operation>
```

Using deterministic functions prevents LLMs from being misused for tasks that software can perform more reliably.

### 8.3 Tools and External Systems

Tools may include:

- search engines;
- vector databases;
- web APIs;
- document stores;
- code execution environments;
- rendering systems;
- game engines;
- project management systems;
- enterprise databases.

Tool use should be governed by explicit permissions and recorded in the execution trace.

If a tool has side effects, such as publishing content or writing to an external database, the engine should require explicit authorisation.

### 8.4 Submodules

A Module may execute another Module.

This allows complex workflows to be decomposed into reusable parts.

Example:

```text
NovelProduction
├── ConceptDevelopment
├── ChapterDrafting
├── ContinuityReview
├── Revision
└── FinalCompilation
```

In TTML, submodule execution may be represented by a `PreExisting` operation.

The parent Module maps inputs into the submodule and maps outputs back into the parent workflow.

### 8.5 Generated Modules

A Cognitive Engine may generate a new submodule to complete a complex or ambiguous task.

This may occur through:

- `ProjectCompletion`;
- `DynamicCompletion`;
- future planning operation types.

Recommended process:

```text
Ambiguous Operation
        ↓
Generate Proposed Plan or Submodule
        ↓
Validate Generated Module
        ↓
Optional Human Approval
        ↓
Execute Generated Module
        ↓
Record Generated Module in Trace
```

Generated modules should be preserved as execution artefacts.

### 8.6 Human Review

Human review may be used where judgement, approval or correction is required.

Examples:

- approving generated plans;
- reviewing compliance-sensitive outputs;
- validating generated submodules;
- resolving ambiguous decisions;
- correcting failed artefacts;
- approving final publication.

Human review should be treated as part of the execution process, not as an invisible external correction.

A trace should record:

- reviewer identity where appropriate;
- decision;
- timestamp;
- artefacts reviewed;
- changes made;
- approval or rejection status.

---

## 9. Workspace and Trace Layer

Every execution takes place inside a Workspace.

The Workspace stores or references:

- initial inputs;
- intermediate Data Units;
- Collections;
- generated prompts;
- generated submodules;
- deterministic function outputs;
- tool outputs;
- validation reports;
- review decisions;
- error diagnostics;
- final outputs;
- execution traces.

A Workspace may be implemented as:

- a local directory;
- cloud object storage;
- a database;
- an in-memory context;
- a project repository;
- a hybrid asset store.

The Workspace maps logical identifiers to concrete assets.

Example:

```text
Logical ID:
plot_outline

Concrete asset:
/workspaces/run_001/outputs/plot_outline.txt
```

or:

```text
asset_id: plot_outline
run_id: run_001
version: 1
mime_type: text/plain
semantic_type: PlotOutline
produced_by: GeneratePlotOutline
```

The storage mechanism is implementation-specific, but logical identifiers must remain traceable.

---

## 10. Execution Trace

Every Thought Tree execution should produce an execution trace.

The trace records what happened during execution.

A trace may include:

- source module identifier;
- source module version;
- TTML or source format version;
- engine version;
- input identifiers;
- input hashes;
- resolved variables;
- iterator expansions;
- dependency graph;
- execution plan;
- operation start and end times;
- operation status;
- active iterator values;
- model provider;
- model name;
- model settings;
- generated prompt;
- deterministic function calls;
- tool calls;
- submodule paths;
- generated modules;
- output identifiers;
- output hashes;
- validation reports;
- contract results;
- retries;
- errors;
- human review decisions;
- final outputs.

The goal of tracing is not always perfect deterministic reproduction. LLM outputs may vary. The goal is to make the process explicit, inspectable, repeatable and improvable.

---

## 11. Cognitive Transformation Graph

Internally, the Cognitive Engine should represent a Thought Tree program as a Cognitive Transformation Graph.

A simplified graph pattern is:

```text
[Input Data Unit] ──used by──> [Operation] ──produces──> [Output Data Unit]
```

With validation and provenance:

```text
[Input Data Unit]
        │
        ▼
[Operation] ──produces──> [Output Data Unit]
        │                         │
        │                         └── satisfies Contract
        │
        └── executed by LLM / Function / Tool / Module / Human
```

The graph allows the engine to reason about:

- dependencies;
- operation order;
- parallel execution;
- missing inputs;
- output collisions;
- caching;
- provenance;
- validation;
- error recovery;
- final output resolution.

Although simple workflows may look like trees or chains, advanced workflows may form directed graphs.

---

## 12. Registries

A mature Cognitive Engine may maintain several registries.

### 12.1 Module Registry

A catalogue of reusable Thought Tree Modules.

May store:

- module ID;
- version;
- author;
- description;
- required inputs;
- declared outputs;
- compatible schema version;
- required functions;
- required contracts;
- dependencies;
- test results;
- quality status.

### 12.2 Function Registry

A catalogue of deterministic functions.

May store:

- function name;
- purpose;
- input requirements;
- output behaviour;
- supported formats;
- deterministic status;
- side effects;
- error conditions;
- version;
- permissions required.

### 12.3 Contract Registry

A catalogue of reusable semantic contracts.

May store:

- contract identifier;
- semantic type;
- required structure;
- validation rules;
- LLM review rubrics;
- human approval requirements;
- compatible validators.

### 12.4 Model Registry

A catalogue of available LLMs or AI models.

May store:

- provider;
- model name;
- capabilities;
- context length;
- supported modalities;
- cost profile;
- latency profile;
- privacy constraints;
- structured output support;
- tool support.

### 12.5 Tool Registry

A catalogue of external tools.

May store:

- tool name;
- purpose;
- input schema;
- output schema;
- permissions;
- authentication requirements;
- rate limits;
- side effects;
- safety constraints.

Registries allow Thought Tree programs to remain abstract while still being executable in a concrete environment.

---

## 13. Execution Lifecycle

A typical Thought Tree execution follows this lifecycle:

```text
1. Load source definition
2. Validate source syntax
3. Parse into Program Model
4. Initialise Workspace
5. Resolve inputs and variables
6. Resolve Data Units and Collections
7. Expand iterators
8. Build dependency graph
9. Detect missing dependencies and output collisions
10. Create execution plan
11. Execute operations
12. Store intermediate artefacts
13. Validate outputs
14. Handle retries, repairs or review gates
15. Resolve final outputs
16. Record execution trace
17. Return or store final artefacts
```

A Cognitive Engine should not execute a program if pre-execution validation detects unresolved required inputs, invalid iterator expansion, missing functions, missing target modules or unintended output collisions.

---

## 14. Correctness Levels

The architecture distinguishes three levels of correctness.

### 14.1 Schema Correctness

The source document is structurally valid.

For TTML, this means the XML conforms to the TTML schema.

This answers:

```text
Is this a valid Thought Tree source document?
```

### 14.2 Execution Correctness

The Cognitive Engine can resolve dependencies, execute operations and produce declared outputs.

This answers:

```text
Can this program run?
```

### 14.3 Semantic Correctness

The produced outputs satisfy their intended meaning, structure, quality and downstream purpose.

This answers:

```text
Did the program produce the right kind of artefact?
```

Semantic correctness may be checked through:

- deterministic validators;
- semantic contracts;
- LLM review;
- comparison against source material;
- human approval.

---

## 15. Error Handling

Execution may fail for many reasons:

- invalid source syntax;
- schema validation failure;
- unresolved Data Unit references;
- unresolved Collection references;
- invalid variables;
- invalid iterators;
- output collisions;
- missing input files;
- missing deterministic functions;
- unavailable target Modules;
- unavailable model provider;
- LLM API failure;
- malformed LLM output;
- failed semantic contract validation;
- permission errors;
- storage errors;
- human review rejection;
- final output resolution failure.

The engine may recover by:

- retrying the operation;
- using a different model;
- repairing malformed output;
- invoking a validation or repair Module;
- requesting human review;
- rolling back partial outputs;
- creating a diagnostic report;
- failing safely.

Silent failure should not be allowed.

Silent overwriting should be invalid by default.

All errors and recovery attempts should be recorded in the execution trace.

---

## 16. Parallel and Optimised Execution

The default execution model may be document-order execution.

However, once a dependency graph exists, the engine may optimise execution by:

- running independent operations in parallel;
- caching outputs;
- skipping unchanged deterministic steps;
- reusing validated artefacts;
- selecting cheaper models for low-risk steps;
- selecting stronger models for high-risk steps;
- batching similar calls;
- rerunning only affected downstream operations after input changes.

Optimisation must not change the intended semantics of the program.

Correctness, traceability and safety should take priority over speed.

---

## 17. Security and Governance

A Cognitive Engine may read files, write outputs, call external tools, execute functions, use external models and generate new modules. This creates security and governance concerns.

Important safeguards include:

- file access permissions;
- tool access permissions;
- sandboxed deterministic functions;
- side-effect controls;
- API credential management;
- prompt injection mitigation;
- untrusted module validation;
- cost limits;
- loop termination safeguards;
- human approval policies;
- data privacy controls;
- audit log retention.

A mature engine should distinguish between:

- trusted and untrusted Modules;
- read-only and side-effecting operations;
- local and external model execution;
- safe and unsafe tools;
- advisory and required validation gates.

---

## 18. Minimal Engine Conformance

A basic Cognitive Engine should support:

- loading a Thought Tree source document;
- validating its structure;
- resolving root-level inputs;
- resolving variables;
- resolving Data Unit references;
- executing operations in document order;
- supporting `TextCompletion`;
- supporting registered `ExecuteFunction` operations;
- producing declared outputs;
- resolving final outputs;
- detecting unresolved dependencies;
- detecting output collisions;
- recording an execution trace.

A more advanced engine may support:

- Collections;
- iterator expansion;
- semantic types;
- semantic contracts;
- validation gates;
- submodule execution;
- generated submodules;
- DynamicCompletion;
- ProjectCompletion;
- human review;
- module registries;
- function registries;
- model selection;
- tool use;
- caching;
- parallel execution;
- module testing;
- module improvement workflows.

---

## 19. Current Implementation Status

This repository is a handoff release.

Known existing implementation work includes:

- an early C#/Unity proof of concept that used hardcoded prompt flows and passed text files between operations;
- a partial C#/Unity Cognitive Engine prototype;
- provider connection work for Anthropic, OpenAI and local LLMs through KoboldCPP;
- initial support for a `TextCompletion`-style operation;
- exploratory work toward TTML import, not completed.

The current implementation is not production-ready.

The architecture described here is the intended framework direction.

---

## 20. Recommended Next Implementation Steps

Suggested next steps for contributors:

1. Extract the abstract Program Model into code.
2. Define a minimal TTML parser.
3. Implement a CLI runner.
4. Implement workspace mapping from logical Data Unit IDs to files.
5. Execute a simple `TextCompletion` operation.
6. Execute a simple `ExecuteFunction` operation.
7. Record a JSON execution trace.
8. Add iterator expansion.
9. Add Collection resolution.
10. Add output collision detection.
11. Add dependency graph visualisation.
12. Add semantic contract prototypes.

A useful first milestone is:

```text
Run examples/article-summary-review/article_summary_review.ttml
and produce:
- draft_summary
- summary_review
- final_summary
- execution_trace.json
```

---

## 21. Summary

The Thought Tree Framework architecture separates cognitive workflow definition from execution.

A Thought Tree source definition describes a cognitive process. The Cognitive Engine compiles that definition into a transformation graph, plans execution, invokes LLMs, deterministic functions, tools, submodules and human review, stores intermediate artefacts, validates outputs and records a trace.

This architecture is intended to move LLM-assisted work beyond isolated prompts and opaque agents toward structured cognitive programming:

```text
Data Units → Operations → Data Units
```

with explicit dependencies, intermediate artefacts, semantic contracts and execution traces.