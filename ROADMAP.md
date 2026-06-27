# Thought Tree Framework Roadmap

## Project Status

The Thought Tree Framework is currently in **handoff / early specification** status.

The core concepts have been developed and documented, and there are early prototypes demonstrating parts of the idea. However, the framework is not yet a finished production system.

This roadmap describes possible development directions for contributors, implementers, researchers, and future maintainers.

The original author is stepping back from active development due to personal, family, work, health, and capacity constraints. The project is being released openly so that others can adopt, fork, critique, implement, or continue it.

---

## Vision

The Thought Tree Framework aims to move LLM-assisted work beyond isolated prompts, fragile prompt chains, and opaque agent loops.

The long-term goal is to support **cognitive programming**: a way to define, compile, execute, validate, trace, reuse, and improve cognitive workflows whose primary objects are concepts, documents, plans, requirements, reviews, policies, stories, designs, and other semantic artefacts.

At the centre of the framework is the pattern:

```text
Data Units → Operations → Data Units
```

A Thought Tree Module describes a structured cognitive process.

A Cognitive Engine compiles and executes that process using a combination of:

- LLMs;
- deterministic functions;
- external tools;
- submodules;
- generated modules;
- semantic contracts;
- validation gates;
- human review;
- workspace artefacts;
- execution traces.

The intended long-term result is a framework where complex LLM-assisted work can become:

- explicit;
- modular;
- inspectable;
- reusable;
- model-independent;
- auditable;
- validatable;
- improvable.

---

## Current Assets

The project currently includes or is expected to include:

- a full framework explainer document;
- a draft Thought Tree Program Model;
- a draft TTML source format specification;
- early examples of Thought Tree workflows;
- an early proof-of-concept prototype;
- a partial Cognitive Engine prototype;
- early thinking around semantic contracts, execution traces, module libraries, and improvement workflows.

Known existing prototype work includes:

- a C#/Unity proof-of-concept demonstrating LLM workflow execution through text artefact passing;
- a partial C#/Unity Cognitive Engine prototype with support for Anthropic, OpenAI, and local LLMs via KoboldCPP;
- early `TextCompletion` execution;
- no complete TTML importer yet;
- no stable reference engine yet.

---

## Roadmap Overview

The roadmap is divided into phases:

1. Clarify and package the handoff release
2. Formalise the program model
3. Stabilise TTML as the first source format
4. Build a minimal reference Cognitive Engine
5. Define the internal transformation graph
6. Add workspace and execution trace support
7. Add semantic types and contracts
8. Build examples and conformance tests
9. Improve authoring and developer experience
10. Support module libraries and registries
11. Support dynamic planning and module improvement
12. Add security, governance, and human review
13. Explore production-grade engines and ecosystem development

These phases do not need to be completed strictly in order. Contributors are welcome to work on any part of the framework.

---

# Phase 1 — Clarify and Package the Handoff Release

## Goal

Make the project understandable, discoverable, and forkable.

The most important early task is to make the core idea clear without requiring every reader to start with the full framework PDF.

## Tasks

- [ ] Create a clear `README.md`.
- [ ] Add `HANDOFF.md` explaining the project context.
- [ ] Add `STATUS.md` explaining what exists and what is unfinished.
- [ ] Add this `ROADMAP.md`.
- [ ] Add `CONTRIBUTING.md`.
- [ ] Add an open-source license.
- [ ] Add a one-page overview.
- [ ] Add a short architecture summary.
- [ ] Add the full framework PDF.
- [ ] Add at least one minimal example.
- [ ] Link or include the existing prototype repositories.
- [ ] Create GitHub issues for good first contributions.

## Suggested Deliverables

```text
README.md
HANDOFF.md
STATUS.md
ROADMAP.md
CONTRIBUTING.md
docs/ONE_PAGE_OVERVIEW.md
docs/ARCHITECTURE.md
docs/ThoughtTreeFramework.pdf
examples/article-summary-review/
```

## Success Criteria

A new reader should be able to understand the project in five minutes and decide whether they want to read the full documentation, inspect the examples, or contribute.

---

# Phase 2 — Formalise the Thought Tree Program Model

## Goal

Separate the abstract framework from any specific syntax or implementation.

TTML is currently the proposed XML-based source format, but the deeper value of the framework is the program model underneath it.

## Core Concepts to Define

- Concept
- Data Unit
- Semantic Type
- Operation
- Relationship
- Module
- Collection
- Iterator
- Contract
- Workspace
- Execution Trace
- Cognitive Engine
- Cognitive Transformation Graph

## Tasks

- [ ] Extract the program model from the full framework document into `docs/PROGRAM_MODEL.md`.
- [ ] Define the minimum viable Thought Tree Program Model.
- [ ] Clarify the difference between:
  - Concept;
  - Data Unit;
  - physical representation;
  - semantic type;
  - contract.
- [ ] Clarify operation semantics:
  - inputs;
  - outputs;
  - operation type;
  - description;
  - preconditions;
  - postconditions;
  - validation.
- [ ] Clarify how Modules compose recursively.
- [ ] Clarify how Collections differ from iterator-expanded operations.
- [ ] Clarify how execution traces and provenance should work.

## Suggested Deliverables

```text
docs/PROGRAM_MODEL.md
docs/GLOSSARY.md
docs/CORE_CONCEPTS.md
```

## Success Criteria

The framework can be explained without referring specifically to XML or TTML.

A reader should understand that TTML is one possible source representation of a deeper cognitive programming model.

---

# Phase 3 — Stabilise TTML as the Initial Source Format

## Goal

Develop TTML into a clear draft source format for Thought Tree Modules.

TTML should remain:

- human-readable;
- machine-parseable;
- LLM-authorable;
- suitable for validation;
- suitable for execution by a Cognitive Engine.

## Current TTML Concepts

A TTML document may include:

```xml
<TTML>
  <Project />
  <Inputs />
  <Vars />
  <Iterators />
  <Collections />
  <Operations />
  <Output />
</TTML>
```

Core elements include:

- `Project`
- `Inputs`
- `File`
- `FileRef`
- `Vars`
- `Var`
- `Iterators`
- `Iterator`
- `Collections`
- `Collection`
- `CollectionRef`
- `Operations`
- `Operation`
- root-level `Output`

Current or proposed operation types include:

- `TextCompletion`
- `ExecuteFunction`
- `PreExisting`
- `ProjectCompletion`
- `DynamicCompletion`
- future `HumanReview`
- future `ValidationGate`
- future `ToolCall`

## Tasks

- [ ] Create `spec/TTML_DRAFT.md`.
- [ ] Create or clean up `spec/TTMLSchema.xsd`.
- [ ] Define the minimal valid TTML document.
- [ ] Define required and optional attributes.
- [ ] Define operation type semantics.
- [ ] Define iterator expansion rules.
- [ ] Define Collection resolution rules.
- [ ] Define output collision rules.
- [ ] Define root-level final output resolution.
- [ ] Add examples of valid and invalid TTML.
- [ ] Add notes for future semantic type and contract support.

## Suggested Deliverables

```text
spec/TTML_DRAFT.md
spec/TTMLSchema.xsd
examples/article-summary-review/article_summary_review.ttml
examples/invalid-examples/
```

## Success Criteria

A developer can write a simple TTML parser or validator using the specification.

An LLM or human can create a minimal valid TTML Module by following the documentation.

---

# Phase 4 — Build a Minimal Reference Cognitive Engine

## Goal

Create a simple reference implementation that can execute basic Thought Tree Modules.

This engine does not need to be production-grade. Its purpose is to prove the execution model and give contributors a concrete target.

## Minimum Viable Engine

A minimal Cognitive Engine should support:

- loading a TTML file;
- validating basic structure;
- resolving root-level inputs;
- resolving `FileRef`;
- executing operations in document order;
- supporting `TextCompletion`;
- supporting `ExecuteFunction` for registered deterministic functions;
- storing intermediate artefacts;
- resolving final outputs;
- recording a basic execution trace.

## Tasks

- [ ] Decide implementation language for the reference engine.
- [ ] Create a CLI runner.
- [ ] Implement TTML loading.
- [ ] Implement basic source validation.
- [ ] Implement workspace initialisation.
- [ ] Implement Data Unit resolution.
- [ ] Implement `TextCompletion`.
- [ ] Implement an LLM provider abstraction.
- [ ] Implement at least one LLM provider adapter.
- [ ] Implement `ExecuteFunction`.
- [ ] Implement a simple function registry.
- [ ] Implement final output resolution.
- [ ] Implement execution trace output.

## Suggested Deterministic Functions

- `CopyFile`
- `ConcatenateFiles`
- `ConvertDocumentToText`
- `ValidateXMLAgainstXSD`
- `CreateArchive`
- `CalculateHash`

## Suggested CLI

```bash
thoughttree run examples/article-summary-review/article_summary_review.ttml --workspace ./runs/run_001
```

Optional dry run:

```bash
thoughttree plan examples/article-summary-review/article_summary_review.ttml
```

## Suggested Deliverables

```text
engine/
  README.md
  src/
  tests/
  examples/
```

## Success Criteria

The reference engine can execute a minimal Article Summary → Review → Revise workflow and produce:

- intermediate artefacts;
- final output;
- execution trace.

---

# Phase 5 — Define the Cognitive Transformation Graph

## Goal

Compile Thought Tree source definitions into an internal graph representation before execution.

Although the framework is called Thought Tree, advanced workflows are better understood as directed transformation graphs.

## Graph Model

The graph should represent:

- Data Unit nodes;
- Operation nodes;
- Collection nodes;
- Module nodes;
- dependency edges;
- output edges;
- validation edges;
- provenance relationships.

Basic pattern:

```text
[Input Data Unit] → [Operation] → [Output Data Unit]
```

## Tasks

- [ ] Define an internal graph data model.
- [ ] Represent operation dependencies.
- [ ] Represent outputs and final outputs.
- [ ] Represent Collections.
- [ ] Represent iterator-expanded operation instances.
- [ ] Detect missing dependencies.
- [ ] Detect output collisions.
- [ ] Generate an execution plan from the graph.
- [ ] Add graph visualisation support.

## Suggested Deliverables

```text
docs/TRANSFORMATION_GRAPH.md
spec/internal-graph-schema-draft.json
tools/ttml-to-graph/
```

## Success Criteria

A TTML Module can be compiled into an inspectable graph before execution.

The engine can show what will run before calling any LLM.

---

# Phase 6 — Workspace and Execution Trace

## Goal

Make execution inspectable and auditable.

A Thought Tree execution should produce more than final outputs. It should preserve the process that produced them.

## Workspace Should Store

- original inputs;
- intermediate Data Units;
- final outputs;
- generated prompts;
- deterministic function outputs;
- validation reports;
- generated submodules;
- human review decisions;
- errors;
- retries;
- execution trace.

## Execution Trace May Include

- source module version;
- engine version;
- input identifiers;
- input hashes;
- resolved variables;
- iterator expansions;
- operation execution order;
- operation status;
- LLM provider;
- model name;
- model settings;
- prompts;
- function calls;
- output identifiers;
- output hashes;
- validation results;
- errors;
- retries;
- final outputs.

## Tasks

- [ ] Define workspace layout.
- [ ] Define execution trace schema.
- [ ] Record operation start and end times.
- [ ] Record inputs and outputs for each operation.
- [ ] Record model and function use.
- [ ] Record errors and retries.
- [ ] Record final output resolution.
- [ ] Add trace viewing utilities.

## Suggested Deliverables

```text
spec/EXECUTION_TRACE_SCHEMA_DRAFT.json
docs/WORKSPACE.md
docs/EXECUTION_TRACE.md
```

## Success Criteria

A user can inspect a completed run and answer:

- What inputs were used?
- What operation produced this output?
- What model or function was used?
- What prompt was sent?
- What intermediate artefacts were created?
- What final outputs were returned?

---

# Phase 7 — Semantic Types and Contracts

## Goal

Move beyond checking whether an output file exists.

A Thought Tree workflow should be able to check whether an output is meaningful, complete, and fit for downstream use.

## Key Concepts

A Data Unit may have:

- physical format;
- semantic type;
- contract;
- validation status.

Example:

```text
File: plot_outline.md
Physical format: Markdown
Semantic type: PlotOutline
Contract: ThreeActPlotOutlineContract
```

## Contract Types May Include

- required sections;
- required fields;
- formatting rules;
- completeness criteria;
- consistency criteria;
- source traceability requirements;
- quality rubric;
- deterministic validators;
- LLM review validators;
- human approval requirements.

## Tasks

- [ ] Define semantic type metadata.
- [ ] Define contract metadata.
- [ ] Create `spec/CONTRACT_SCHEMA_DRAFT.json`.
- [ ] Add optional TTML support for semantic types.
- [ ] Add optional TTML support for contracts.
- [ ] Implement deterministic validation functions.
- [ ] Implement LLM-assisted validation.
- [ ] Add contract outcomes:
  - pass;
  - pass with warnings;
  - fail and retry;
  - fail and request review;
  - terminal failure.
- [ ] Record contract validation in execution traces.

## Example Future TTML

```xml
<File
  id="plot_outline"
  extension="md"
  semanticType="PlotOutline"
  contract="ThreeActPlotOutlineContract"/>
```

## Suggested Deliverables

```text
docs/SEMANTIC_CONTRACTS.md
spec/CONTRACT_SCHEMA_DRAFT.json
contracts/examples/
```

## Success Criteria

An operation can produce an output and the engine can validate that output against a declared contract before downstream operations continue.

---

# Phase 8 — Examples and Conformance Tests

## Goal

Create examples that demonstrate the framework and test cases that prevent regressions.

## Example Workflows

Initial examples should include:

- article summary and review;
- legacy project recovery;
- novel development pipeline;
- video game technical design document generation;
- compliance gap analysis;
- cyclic research monitoring;
- module authoring;
- module improvement.

## Tasks

- [ ] Create minimal article summary example.
- [ ] Create medium-sized documentation generation example.
- [ ] Create larger creative production example.
- [ ] Create module improvement example.
- [ ] Add expected graph output for examples.
- [ ] Add validation tests.
- [ ] Add invalid examples for parser testing.
- [ ] Add conformance tests for engines.

## Suggested Deliverables

```text
examples/
  article-summary-review/
  video-game-tdd/
  novel-generation/
  compliance-gap-analysis/
  module-improvement/

tests/conformance/
```

## Success Criteria

A new engine implementation can run the conformance tests and determine which parts of the framework it supports.

---

# Phase 9 — Authoring and Developer Experience

## Goal

Make Thought Tree Modules easier to write, inspect, debug, and improve.

Raw XML is useful as an interchange format, but authoring complex workflows directly in TTML may be difficult.

## Possible Tools

- TTML linter;
- TTML formatter;
- schema-aware editor support;
- CLI validator;
- graph visualiser;
- dry-run execution planner;
- YAML-to-TTML converter;
- visual graph editor;
- LLM-assisted authoring assistant.

## Tasks

- [ ] Create TTML linter.
- [ ] Create TTML formatter.
- [ ] Add CLI validation.
- [ ] Add dry-run mode.
- [ ] Add Graphviz or Mermaid visualisation.
- [ ] Prototype YAML syntax.
- [ ] Create prompt pack for LLM-assisted TTML authoring.
- [ ] Create authoring guide.

## Suggested Deliverables

```text
tools/ttml-lint/
tools/ttml-format/
tools/ttml-graph/
docs/AUTHORING_GUIDE.md
```

## Success Criteria

A user can author a module, validate it, visualise its execution plan, and fix common errors without needing to understand the whole engine implementation.

---

# Phase 10 — Module Libraries and Registries

## Goal

Support reusable cognitive programs.

The long-term value of Thought Tree may come from reusable libraries of modules, contracts, functions, and validators.

## Registry Types

Possible registries include:

- Module Registry;
- Function Registry;
- Contract Registry;
- Semantic Type Registry;
- Model Registry;
- Tool Registry.

## Module Metadata May Include

- module ID;
- version;
- author;
- description;
- required inputs;
- declared outputs;
- required TTML version;
- required engine capabilities;
- required functions;
- required contracts;
- dependencies;
- test cases;
- validation history.

## Tasks

- [ ] Define module package metadata.
- [ ] Define versioning policy.
- [ ] Define dependency declarations.
- [ ] Create local module registry prototype.
- [ ] Add module search.
- [ ] Add module validation status.
- [ ] Add module test history.
- [ ] Add private and public registry concepts.

## Suggested Deliverables

```text
spec/MODULE_PACKAGE_DRAFT.json
docs/MODULE_LIBRARIES.md
registry/examples/
```

## Success Criteria

Modules can be packaged, versioned, searched, validated, reused, and improved.

---

# Phase 11 — Dynamic Planning and Module Improvement

## Goal

Support controlled dynamic execution and self-improvement patterns.

The framework should allow a high-level ambiguous operation to be expanded into a generated submodule, but this must be done with validation, traceability, and review.

## Dynamic Planning Flow

```text
Ambiguous Operation
↓
Generate Proposed Plan or Submodule
↓
Validate Generated Module
↓
Optional Human Approval
↓
Execute Approved Module
↓
Record Generated Module in Trace
```

## Module Improvement Flow

```text
Original Module
↓
Extract Intended Requirements
↓
Validate Schema
↓
Validate Executability
↓
Review Design
↓
Generate Test Cases
↓
Compile Improvement Findings
↓
Design Improved Module
↓
Draft Improved Module
↓
Validate Improved Module
↓
Compare Original and Improved Versions
↓
Produce Final Improved Module
```

## Tasks

- [ ] Define `ProjectCompletion` semantics.
- [ ] Define `DynamicCompletion` semantics.
- [ ] Preserve generated submodules.
- [ ] Validate generated submodules before execution.
- [ ] Add optional human approval.
- [ ] Create module improvement workflow example.
- [ ] Compare original and improved module versions.
- [ ] Record improvement trace.

## Suggested Deliverables

```text
docs/DYNAMIC_PLANNING.md
docs/IMPROVEMENT_PROCESS.md
examples/module-improvement/
```

## Success Criteria

A Cognitive Engine can generate or improve a module while preserving intent, validation, traceability, and human review options.

---

# Phase 12 — Security, Governance, and Human Review

## Goal

Make Thought Tree execution safe enough for real-world use.

As engines gain the ability to call tools, execute functions, generate modules, read files, and write outputs, governance becomes essential.

## Concerns

- untrusted modules;
- prompt injection;
- unsafe file access;
- tool permissions;
- side effects;
- API credentials;
- external model privacy;
- dynamic module generation;
- human approval requirements;
- cost control;
- loop termination;
- audit retention.

## Tasks

- [ ] Define trusted and untrusted module modes.
- [ ] Define safe execution profiles.
- [ ] Define tool permission model.
- [ ] Sandbox deterministic functions.
- [ ] Add human review operation type or gate.
- [ ] Add cost limits.
- [ ] Add loop and dynamic execution safeguards.
- [ ] Add audit export guidance.
- [ ] Add governance checklist.

## Suggested Deliverables

```text
docs/SECURITY.md
docs/GOVERNANCE.md
docs/HUMAN_REVIEW.md
```

## Success Criteria

A user or organisation can understand and control what a Thought Tree Module is allowed to do before executing it.

---

# Phase 13 — Production Engines and Ecosystem

## Goal

Enable production-grade implementations and a wider ecosystem.

Different implementations may specialise in:

- local execution;
- enterprise execution;
- creative production;
- compliance workflows;
- research automation;
- visual authoring;
- hosted execution;
- module libraries.

## Possible Ecosystem Components

- reference Cognitive Engine;
- production Cognitive Engines;
- hosted Thought Tree execution service;
- TTML visual editor;
- module marketplace;
- contract registry;
- conformance test suite;
- model benchmarking tools;
- enterprise governance tools.

## Tasks

- [ ] Define engine conformance levels.
- [ ] Build production-ready execution service.
- [ ] Add model routing.
- [ ] Add caching and incremental reruns.
- [ ] Add dashboards.
- [ ] Add audit export.
- [ ] Add module marketplace concepts.
- [ ] Add public example module library.
- [ ] Write case studies.

## Success Criteria

Thought Tree becomes usable as a practical framework for building, sharing, executing, validating, and improving cognitive programs.

---

# Suggested Good First Issues

Contributors looking for a starting point may consider:

- [ ] Convert a section of the framework PDF into Markdown.
- [ ] Create a minimal `article-summary-review.ttml` example.
- [ ] Draft a `TTML_DRAFT.md` from the existing documentation.
- [ ] Write a comparison with LangGraph, AutoGen, CrewAI, Semantic Kernel, or LangChain.
- [ ] Create a Mermaid diagram of the Cognitive Engine architecture.
- [ ] Draft an execution trace JSON schema.
- [ ] Implement a simple TTML parser.
- [ ] Implement a `ConcatenateFiles` deterministic function.
- [ ] Build a dry-run planner that prints operation order.
- [ ] Create a Graphviz visualisation from a TTML file.
- [ ] Write a minimal contract example for `Summary`.
- [ ] Create invalid TTML examples for test cases.
- [ ] Improve README clarity.

---

# Conformance Levels

The project may eventually define formal conformance levels.

## Level 0 — Documentation / Spec Consumer

Can read and understand Thought Tree definitions but does not execute them.

## Level 1 — Basic TTML Executor

Supports:

- TTML loading;
- basic validation;
- root-level inputs;
- `FileRef`;
- document-order operation execution;
- `TextCompletion`;
- final output resolution;
- basic execution trace.

## Level 2 — Structured Executor

Adds:

- variables;
- iterators;
- Collections;
- output collision detection;
- `ExecuteFunction`;
- function registry;
- workspace management.

## Level 3 — Graph Engine

Adds:

- transformation graph compilation;
- dependency planning;
- dry-run planning;
- safe parallelisation;
- richer traces.

## Level 4 — Contract-Aware Engine

Adds:

- semantic types;
- contracts;
- validation gates;
- deterministic validators;
- LLM review validators;
- repair or retry flows.

## Level 5 — Advanced Cognitive Engine

Adds:

- `PreExisting` modules;
- `ProjectCompletion`;
- `DynamicCompletion`;
- generated submodules;
- module improvement workflows;
- human review;
- module libraries;
- governance controls.

---

# Guiding Principles

Future development should preserve the core principles of the framework:

1. **Explicit process over opaque prompting**
2. **Data Units as inspectable artefacts**
3. **Operations as transformations**
4. **Modules as reusable cognitive programs**
5. **LLMs for semantic work**
6. **Deterministic functions for mechanical work**
7. **Contracts for quality control**
8. **Traces for auditability**
9. **Model independence**
10. **Human review where judgement matters**
11. **Dynamic execution only with validation and traceability**
12. **Improvement through review, testing, and versioning**

---

# Final Note

This roadmap is intentionally broad.

The Thought Tree Framework can evolve in several directions:

- as a specification;
- as a reference engine;
- as a set of authoring tools;
- as a module library ecosystem;
- as a research project;
- as an enterprise workflow framework;
- as a foundation for vertical AI products.

Contributors are welcome to adopt, fork, simplify, challenge, or extend the roadmap.

The core aim is to give LLM-assisted work a more structured, inspectable, reusable, and improvable foundation.