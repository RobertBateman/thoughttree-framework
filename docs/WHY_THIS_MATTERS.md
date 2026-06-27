# Why This Matters

## Thought Tree Framework in One Sentence

The Thought Tree Framework is an open framework for describing complex LLM-assisted work as structured, executable, inspectable cognitive programs.

Instead of asking an LLM for one answer, a Thought Tree defines the process by which an answer should be produced, reviewed, validated, revised and traced.

At its simplest:

```text
Data Units → Operations → Data Units
```

At larger scale:

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

The aim is to move LLM-assisted work beyond isolated prompts, brittle prompt chains and opaque autonomous agents toward something closer to reusable cognitive software.

---

## The Problem

Large Language Models are powerful because they can work with ambiguous human concepts:

- documents;
- notes;
- requirements;
- plans;
- policies;
- stories;
- reviews;
- designs;
- decisions;
- arguments;
- creative briefs;
- technical specifications.

But most LLM usage is still organised around:

- individual prompts;
- chat sessions;
- informal prompt chains;
- loosely controlled agents;
- bespoke scripts;
- opaque automation.

These approaches can be useful, but they often lack qualities expected from serious production systems:

- explicit structure;
- inspectable intermediate results;
- repeatable execution;
- modular reuse;
- dependency management;
- validation;
- versioning;
- error handling;
- audit trails;
- separation between workflow definition and execution environment.

For small tasks, a prompt may be enough.

For larger cognitive work, a prompt is usually not enough.

---

## A Prompt Produces an Output. A Thought Tree Defines a Process.

A prompt says:

```text
User → Prompt → LLM → Output
```

A Thought Tree says:

```text
Inputs → Transformations → Intermediate Artefacts → Review → Revision → Final Outputs → Trace
```

This distinction matters.

For example, producing a technical design document from scattered notes may require:

1. normalising the source material;
2. creating a source digest;
3. extracting requirements;
4. identifying assumptions and unresolved questions;
5. decomposing the system;
6. drafting document sections;
7. assembling the document;
8. reviewing for gaps and unsupported claims;
9. revising the final document;
10. preserving a trace of how the document was produced.

That is not just a prompt.

It is a process.

The Thought Tree Framework exists to make that kind of process explicit, reusable and executable.

---

## Cognitive Programming

The central idea of the framework is **cognitive programming**.

In ordinary programming, programs transform formal data structures:

```text
numbers → functions → numbers
records → procedures → records
files → scripts → files
```

In cognitive programming, programs transform meaningful artefacts:

```text
notes → source digest → requirements register → technical design document
story brief → plot outline → chapter drafts → revised manuscript
policy documents → obligation register → gap analysis → audit pack
existing module → validation report → improved module
```

The primary objects are not only strings, files or database rows.

They are concepts:

- requirements;
- risks;
- character profiles;
- plot outlines;
- compliance obligations;
- review reports;
- correction plans;
- technical designs;
- generated modules.

A Thought Tree program describes how these concept-bearing artefacts should be created, transformed, reviewed, validated and assembled.

---

## LLMs Handle Ambiguity. Code Handles Structure.

The framework does not treat the LLM as the whole system.

Instead:

```text
LLMs handle ambiguity.
Code handles structure.
The Cognitive Engine coordinates both.
```

Some tasks are semantic and ambiguous:

- summarise this source material;
- infer requirements from these notes;
- draft a chapter;
- review this document for missing assumptions;
- identify contradictions;
- propose a correction plan.

These are appropriate for LLMs.

Other tasks are mechanical and deterministic:

- concatenate files;
- copy assets;
- validate XML;
- convert Markdown to HTML;
- calculate hashes;
- archive outputs;
- run schema checks.

These are appropriate for ordinary software functions.

Some tasks require external tools:

- search a document store;
- call an API;
- query a database;
- use a vector index;
- render a report.

Some tasks require human judgement:

- approve a generated plan;
- review compliance-sensitive content;
- assess creative quality;
- resolve ambiguous requirements;
- approve publication.

Thought Tree is designed to coordinate all of these execution modes in one structured workflow.

---

## The Cognitive Engine

A Thought Tree Module describes what should happen.

A **Cognitive Engine** determines how to execute it.

The Cognitive Engine acts as both compiler and runtime.

It is responsible for:

- loading a Thought Tree definition;
- validating its structure;
- resolving inputs, variables, iterators and references;
- expanding repeated operations;
- building a dependency graph;
- detecting missing dependencies and output collisions;
- creating an execution plan;
- invoking LLMs, functions, tools, submodules or human review;
- managing the workspace;
- preserving intermediate artefacts;
- validating outputs;
- handling errors and retries;
- recording an execution trace;
- returning final outputs.

This separation is important.

A Thought Tree program should define the cognitive workflow independently of any particular model provider.

The same workflow could potentially be executed using:

- OpenAI;
- Anthropic;
- local models;
- specialist models;
- deterministic functions;
- human review;
- different storage systems;
- different organisational policies.

The program defines the process.

The engine executes the process.

---

## Intermediate Artefacts Matter

One of the most important ideas in the framework is that intermediate artefacts should be preserved.

A final output is easier to trust, debug and improve when the process that produced it is visible.

For example:

```text
source notes
↓
source digest
↓
requirements register
↓
system decomposition
↓
draft document sections
↓
assembled draft
↓
review report
↓
correction plan
↓
final document
```

If the final document is weak, the user does not have to rerun one giant prompt and hope for the best.

They can inspect the process:

- Was the source digest incomplete?
- Were requirements extracted incorrectly?
- Did the system decomposition miss something?
- Was the draft weak?
- Did the review fail to catch the issue?
- Should a semantic contract be strengthened?
- Should a deterministic validation step be added?

This makes LLM-assisted work more debuggable.

---

## Semantic Contracts

Producing a file is not the same as producing a valid artefact.

A workflow may successfully create:

```text
plot_outline.txt
```

But the file may still be too vague, incomplete or inconsistent to support chapter drafting.

The Thought Tree Framework therefore introduces the idea of **Semantic Contracts**.

A contract defines what an artefact must satisfy to be considered valid.

For example, a `PlotOutline` contract may require:

- premise;
- major characters;
- setting;
- act structure;
- central conflict;
- major turning points;
- climax;
- resolution;
- unresolved continuity notes.

A `TechnicalRequirementsRegister` contract may require:

- functional requirements;
- non-functional requirements;
- assumptions;
- dependencies;
- risks;
- open questions;
- source traceability;
- distinction between explicit, inferred and unresolved requirements.

This allows a Cognitive Engine to distinguish between:

1. schema correctness — the program is structurally valid;
2. execution correctness — the operations ran and produced files;
3. semantic correctness — the outputs are actually fit for purpose.

That third level is essential for cognitive programming.

---

## Execution Traces and Provenance

LLM-assisted workflows are often hard to audit.

A Thought Tree execution should record what happened.

An execution trace may include:

- source module version;
- input identifiers;
- input hashes;
- resolved variables;
- iterator expansions;
- operation execution order;
- prompts;
- model providers;
- model names;
- model settings;
- function calls;
- tool calls;
- submodule executions;
- output identifiers;
- output hashes;
- validation results;
- errors;
- retries;
- human review decisions;
- final outputs.

This makes it possible to ask:

- Which inputs produced this output?
- Which operation generated this artefact?
- Which model was used?
- What prompt was sent?
- Did the output pass validation?
- Was a human reviewer involved?
- Which version of the module was executed?

The goal is not always perfect deterministic reproduction, because LLMs may vary between runs.

The goal is to make the process explicit, inspectable, repeatable and improvable.

---

## Reusable Cognitive Infrastructure

Many organisations repeatedly perform cognitive work such as:

- reviewing documents;
- extracting requirements;
- producing reports;
- generating plans;
- analysing risks;
- preparing compliance evidence;
- summarising research;
- standardising legacy material;
- drafting communications;
- reviewing creative content;
- producing design documents.

Much of this work is currently performed through ad hoc human labour, informal prompting or bespoke automation.

Thought Tree Modules allow these processes to be captured as reusable cognitive programs.

Over time, organisations could build module libraries that encode:

- their document standards;
- their review methods;
- their risk analysis processes;
- their creative development pipelines;
- their compliance procedures;
- their technical documentation patterns;
- their quality criteria.

A module library becomes more than a collection of prompts.

It becomes a repository of reusable cognitive processes.

---

## Why Open Release Matters

This project is being released openly because the idea should not be trapped with one person.

The framework is currently a draft, not a finished product.

It includes:

- a conceptual model;
- a draft TTML source format;
- a Cognitive Engine architecture;
- execution semantics;
- semantic contract ideas;
- examples;
- an improvement process;
- prototype code;
- a roadmap.

There is significant work still to do.

But the core idea may be useful to others working on:

- LLM workflow orchestration;
- agent frameworks;
- cognitive architectures;
- AI-assisted documentation;
- compliance automation;
- game content generation;
- creative production;
- model-independent AI infrastructure;
- human-in-the-loop AI systems;
- reusable AI process libraries.

The project is released under permissive terms so others can freely inspect, reuse, fork, adapt, implement or improve it.

---

## Licensing Intent

The intended licensing approach is:

- **Code:** MIT License
- **Documentation, specifications, examples and conceptual material:** CC0

The intent is to remove friction.

People should be able to:

- implement their own Cognitive Engines;
- adapt the TTML ideas;
- create alternative source formats;
- build compatible or incompatible systems;
- use the terminology;
- fork the examples;
- create module libraries;
- develop commercial or non-commercial tools;
- improve the framework in directions the original author did not anticipate.

Attribution is appreciated, but the goal is not control.

The goal is for the idea to have the best possible chance of being useful.

---

## The Strategic Claim

The strategic claim of the Thought Tree Framework is simple:

> LLM-assisted work should not remain trapped in isolated prompts, opaque chats or uncontrolled agent loops.

It should be possible to define cognitive work as structured programs:

- explicit;
- modular;
- inspectable;
- reusable;
- model-independent;
- hybrid;
- traceable;
- validatable;
- improvable.

Thought Tree is an early attempt to describe what that programming layer might look like.

It is not the final answer.

It is an open starting point.