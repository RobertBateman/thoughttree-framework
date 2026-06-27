# Thought Tree Framework — One Page Overview

**Thought Tree is an open framework for cognitive programming with LLMs.**

It describes complex LLM-assisted work as structured, executable, inspectable workflows made from artefacts, transformations, validation steps and execution traces.

The core pattern is:

```text
Data Units → Operations → Data Units
```
A Data Unit is a named artefact, such as a document, summary, requirement register, plot outline, review report, generated module or final output.

An Operation transforms one or more Data Units into new Data Units. Operations may be performed by an LLM, deterministic function, tool, submodule or human reviewer.

A Module is a reusable cognitive program: a structured workflow that receives inputs, performs operations and produces outputs.

A Cognitive Engine is the compiler and runtime that loads a Thought Tree Module, resolves dependencies, executes operations, stores intermediate artefacts, validates outputs and records a trace.

# The Problem
LLMs are powerful, but many LLM workflows are still built from:

- one-off prompts;
- chat sessions;
- brittle prompt chains;
- opaque autonomous agents;
- bespoke scripts;
- workflow tools without semantic validation.

These approaches can produce useful outputs, but they often lack:

- explicit structure;
- inspectable intermediate results;
- reusable modules;
- dependency management;
- validation;
- audit trails;
- model independence;
- provenance;
- and controlled human review.

For simple tasks, a prompt may be enough.
For complex cognitive work, the process matters.

# The Thought Tree Approach
Instead of asking for a final result in one step, a Thought Tree defines the production process.

Example:
```text
design_notes
↓
CreateSourceDigest
↓
source_digest
↓
ExtractRequirements
↓
requirements_register
↓
DesignSystemDecomposition
↓
system_decomposition
↓
DraftSections
↓
draft_sections
↓
AssembleDraft
↓
draft_document
↓
ReviewDraft
↓
correction_plan
↓
ReviseFinalDocument
↓
final_document
```
Each intermediate artefact can be inspected, replaced, validated, reused or traced.

This turns LLM-assisted work from an opaque completion into an auditable cognitive production pipeline.

# Cognitive Programming
Thought Tree proposes a shift from prompting to cognitive programming.

Traditional programs operate mainly on formal data structures.

Thought Tree programs operate on meaningful artefacts:

- documents;
- ideas;
- requirements;
- plans;
- summaries;
- reviews;
- policies;
- stories;
- designs;
- decisions;
- generated modules.

The goal is not to make LLM outputs magically correct.

The goal is to make LLM-assisted work more:

- structured;
- modular;
- inspectable;
- reusable;
- validatable;
- traceable;
- and improvable.

# How It Works
A Thought Tree workflow is defined as a Module.

A Module may contain:

- required inputs;
- variables;
- iterators;
- collections;
- operations;
- intermediate outputs;
- semantic contracts;
- review stages;
- final outputs.

The Cognitive Engine compiles the Module into an executable transformation graph.
```text
Source Definition
↓
Parse
↓
Validate
↓
Resolve references
↓
Expand iterators
↓
Build dependency graph
↓
Plan execution
↓
Run LLMs / functions / tools / submodules / human review
↓
Validate outputs
↓
Record trace
↓
Return final artefacts
```
In this model:

- LLMs handle ambiguity.
- Code handles structure.
- Humans handle judgement.
- The Cognitive Engine coordinates all three.

# TTML
The current draft source format is TTML: Thought Tree Markup Language.

TTML is an XML-based format for defining Thought Tree Modules.

A simplified TTML operation looks like:
```xml
<Operation
  id="ReviewSummary"
  type="TextCompletion"
  desc="Review the draft summary against the source article. Identify omissions, inaccuracies and unsupported claims.">
  <FileRef id="source_article"/>
  <FileRef id="draft_summary"/>
  <Output>
    <File id="summary_review" extension="txt"/>
  </Output>
</Operation>
```
TTML is not the whole framework.
It is one representation of the underlying Thought Tree Program Model.
Future authoring formats could include YAML, JSON, visual graphs or LLM-assisted editors.

# Why This Is Different
**Compared with a prompt**
A prompt asks for an answer.
A Thought Tree defines the process for producing, reviewing and tracing an answer.

**Compared with a prompt chain**
A prompt chain links model calls.
Thought Tree formalises artefacts, dependencies, outputs, validation and traces.

**Compared with autonomous agents**
Agents are flexible but can be unpredictable and hard to audit.
Thought Tree emphasises explicit structure, inspectable plans and controlled execution.

**Compared with traditional workflow tools**
Workflow tools handle deterministic processes well.
Thought Tree is designed for hybrid cognitive workflows where LLMs, code, tools and humans work together.

# Example Use Cases
Thought Tree is especially suited to complex cognitive production tasks, such as:

- technical documentation generation;
- legacy project recovery;
- requirements extraction;
- compliance and audit preparation;
- policy analysis;
- research monitoring and reporting;
- creative writing pipelines;
- personalised media generation;
- game content generation;
- module generation and improvement;
- enterprise knowledge transformation.

# Current State
This project is currently a handoff release.

The framework has:

- a developed conceptual model;
- a long-form explainer;
- a draft TTML standard;
- example workflows;
- a roadmap;
- an early proof-of-concept prototype;
- and a partial Cognitive Engine prototype.

The early proof of concept showed that LLM operations could pass text artefacts between stages and successfully generate, review and revise a 50,000-word novel from a short user brief.

The partial Cognitive Engine prototype supports connections to Anthropic, OpenAI and local LLMs through KoboldCPP, and can execute basic text completions.

However, the project is not complete.

It still needs:

- a stable TTML schema;
- a reference Cognitive Engine;
- a TTML importer;
- execution graph compilation;
- semantic contracts;
- validation gates;
- trace schema;
- conformance tests;
- examples;
- tooling;
- and community stewardship.

# What Is Being Released
The project is being released openly so others can:

- study it;
- fork it;
- implement it;
- critique it;
- compare it with existing frameworks;
- build tools around it;
- create module libraries;
- or evolve it into something better.

The original author is stepping back due to family commitments, caring responsibilities, work commitments, burnout recovery and limited capacity to take the project further.

The aim of the handoff is simple:

The idea should not be trapped with one person.

# The Strategic Vision
Thought Tree is not just a prompt format.

It is an early model for a programming layer over cognitive work.

The long-term vision is a world where useful cognitive processes can be:

- written;
- shared;
- inspected;
- executed;
- tested;
- validated;
- versioned;
- improved;
- and reused.

A mature ecosystem could include:

- Cognitive Engines;
- TTML or alternative source formats;
- visual editors;
- semantic contract libraries;
- function registries;
- module libraries;
- conformance tests;
- execution trace tooling;
- and reusable cognitive workflow packages.

The most important idea is that LLM workflows can become maintainable cognitive assets, not disposable prompts.

# Short Summary
Thought Tree represents cognitive work as:
```text
Concepts / Data Units
↓
Transformations / Operations
↓
Intermediate Artefacts
↓
Validation / Review
↓
Final Outputs
↓
Execution Trace
```
It is a framework for making LLM-assisted work:

- programmable
- modular
- inspectable
- model-independent
- auditable
- validatable
- reusable
- improvable

This repository is an open handoff of the Thought Tree Framework so that others can continue, adapt or build on the idea.