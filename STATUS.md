
# Thought Tree Framework — Project Status

## Current status

The Thought Tree Framework is in **handoff status**.

It is not a finished production system.

The project currently consists of:

- a developed conceptual framework;
- a long-form explainer document;
- draft TTML ideas;
- example workflow designs;
- an early proof-of-concept prototype;
- a partial Cognitive Engine prototype;
- roadmap and architecture notes;
- and a handoff invitation for others to continue the work.

The original author is stepping back due to family, caring, work, health and capacity constraints.

The project is being made publicly available so that others can adopt, fork, implement, critique or continue it.

---

## Summary

Thought Tree is a proposed framework for **cognitive programming** with LLMs.

It describes complex LLM-assisted work as explicit, inspectable workflows composed of:

- Data Units;
- Operations;
- Modules;
- Collections;
- Iterators;
- Semantic Contracts;
- Execution Traces;
- and a Cognitive Engine.

The framework is intended to move from:

```text
Prompt → LLM → Output
```

toward:

```text
Inputs
↓
Transformations
↓
Intermediate Artefacts
↓
Validation / Review
↓
Final Outputs
↓
Execution Trace
```

The core model is:

```text
Data Units → Operations → Data Units
```

At larger scale, a Thought Tree Module becomes a cognitive transformation graph.

---

## What is conceptually defined

The current framework draft defines the following major concepts.

### Thought Tree Program Model

The abstract model underlying the framework.

A Thought Tree program defines:

- required inputs;
- meaningful Data Units;
- transformations;
- relationships;
- intermediate artefacts;
- validation expectations;
- final outputs;
- and execution trace requirements.

### Data Units

Discrete artefacts used or produced by the workflow.

Examples:

- documents;
- summaries;
- plans;
- reviews;
- requirements registers;
- chapter drafts;
- design documents;
- validation reports;
- generated modules.

### Operations

Transformations that consume Data Units and produce new Data Units.

Operation types currently described include:

- `TextCompletion`;
- `ExecuteFunction`;
- `PreExisting`;
- `ProjectCompletion`;
- `DynamicCompletion`.

Possible future operation types include:

- `HumanReview`;
- `ToolCall`;
- `ValidationGate`;
- `Condition`;
- `Loop`;
- `Schedule`.

### Modules

Reusable cognitive programs.

A Module declares:

- inputs;
- variables;
- iterators;
- collections;
- operations;
- outputs.

Modules may call other Modules, allowing recursive composition.

### Cognitive Engine

The compiler and runtime for Thought Tree programs.

The Cognitive Engine is responsible for:

- loading source definitions;
- parsing and validation;
- resolving references;
- expanding iterators;
- building an execution graph;
- planning execution;
- invoking LLMs, deterministic functions, tools, submodules and humans;
- storing artefacts;
- validating outputs;
- handling errors;
- recording traces.

### TTML

Thought Tree Markup Language.

TTML is the proposed XML-based source and interchange format for Thought Tree Modules.

TTML is currently a draft concept and not a finished standard.

### Semantic Contracts

Contracts define what an output must satisfy to be considered valid.

They distinguish between:

1. schema correctness;
2. execution correctness;
3. semantic correctness.

For example, a file named `plot_outline.txt` should not merely exist. It should satisfy the requirements of a usable plot outline.

### Execution Trace

A record of what happened during execution.

A trace may include:

- source module version;
- inputs;
- variables;
- iterator expansions;
- operation executions;
- model calls;
- prompts;
- function calls;
- generated outputs;
- validation results;
- errors;
- retries;
- human decisions;
- final outputs.

---

## What has been proven

An early proof-of-concept prototype demonstrated that:

- LLM operations can pass text artefacts between steps;
- intermediate generated files can be used as context for later operations;
- a large output can be produced through a structured multi-step process rather than one prompt.

The prototype successfully planned, drafted and reviewed a roughly 50,000-word novel from a roughly 500-word user-submitted description.

Important caveat:

- this proof-of-concept did **not** use TTML;
- the workflow was implemented as an array of hardcoded prompts;
- it should be treated as a historical proof of concept rather than a production implementation.

---

## Existing codebases

### 1. Unity Novel Proof of Concept

Language / environment:

```text
C# / Unity Game Engine
```

Purpose:

- prove that an LLM workflow can pass text files between operations;
- demonstrate chained generation, planning, drafting and review;
- test large creative production through intermediate artefacts.

Status:

```text
Historical proof of concept.
Not production ready.
Predates TTML.
```

Implemented:

- hardcoded prompt sequence;
- intermediate file passing;
- large creative generation pipeline.

Not implemented:

- TTML;
- general Cognitive Engine;
- graph compilation;
- semantic contracts;
- formal execution trace.

---

### 2. Unity Cognitive Engine Prototype

Language / environment:

```text
C# / Unity Game Engine
```

Purpose:

- begin implementing a more general Cognitive Engine;
- connect to multiple LLM backends;
- support basic text completion operations.

Implemented or partially implemented:

- Anthropic connection;
- OpenAI connection;
- local LLM connection via KoboldCPP;
- basic `TextCompletion` execution.

Paused / unfinished:

- TTML importer;
- stable TTML model;
- graph compiler;
- iterator expansion;
- collection resolution;
- dependency validation;
- output collision detection;
- semantic contracts;
- execution trace model;
- submodule execution;
- dynamic module generation;
- production workspace handling.

Reason work paused:

- TTML structure was being revised;
- the importer was delayed until the source model stabilised;
- the project grew beyond the original author’s available capacity.

---

## Documentation status

### Full framework draft

The most complete description is:

```text
ThoughtTreeFramework.pdf
```

It currently includes sections on:

- Introduction;
- Strategic Vision: Cognitive Programming;
- Core Model;
- System Architecture;
- Thought Tree Program Model;
- Cognitive Engine;
- Execution Semantics;
- Semantic Contracts and Validation;
- TTML Standard;
- Authoring Thought Tree Modules;
- Examples;
- Improvement Process;
- Commercial Applications;
- Roadmap;
- Glossary.

Status:

```text
Substantial draft.
Conceptually rich.
Not yet edited into a polished public specification.
```

### Recommended documentation extraction

Future contributors may wish to extract the PDF into smaller Markdown files:

```text
docs/PROGRAM_MODEL.md
docs/COGNITIVE_ENGINE.md
docs/EXECUTION_SEMANTICS.md
docs/SEMANTIC_CONTRACTS.md
docs/TTML_DRAFT.md
docs/AUTHORING_GUIDE.md
docs/ROADMAP.md
docs/GLOSSARY.md
```

This would make the project easier to navigate on GitHub.

---

## TTML status

TTML stands for:

```text
Thought Tree Markup Language
```

It is intended to be an XML-based serialisation format for Thought Tree Modules.

A high-level TTML structure currently looks like:

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

TTML currently describes:

- project metadata;
- root inputs;
- variables;
- iterators;
- collections;
- operations;
- operation inputs;
- operation outputs;
- final outputs.

Current TTML status:

```text
Draft concept.
Not final.
No stable 1.0 schema yet.
No complete importer yet.
```

Recommended next steps:

- create a draft XSD;
- create minimal valid examples;
- create invalid examples for testing;
- define iterator expansion precisely;
- define collection resolution precisely;
- define output collision rules;
- define semantic type and contract syntax;
- define conformance levels.

---

## Cognitive Engine status

The Cognitive Engine is conceptually defined but not fully implemented.

A minimal Cognitive Engine should eventually support:

- loading a Thought Tree source document;
- validating source structure;
- resolving root inputs;
- resolving variables;
- resolving FileRefs;
- executing operations in document order;
- supporting `TextCompletion`;
- supporting `ExecuteFunction`;
- storing intermediate artefacts;
- resolving final outputs;
- recording an execution trace.

A more complete Cognitive Engine should support:

- iterator expansion;
- collections;
- dependency graph compilation;
- output collision detection;
- semantic contracts;
- validation gates;
- submodule execution;
- generated submodules;
- DynamicCompletion;
- ProjectCompletion;
- human review;
- tool use;
- model selection;
- caching;
- parallel execution;
- module libraries;
- module improvement workflows.

Current implementation status:

```text
Only partial prototype exists.
No production-ready reference engine exists yet.
```

---

## Semantic Contracts status

Semantic Contracts are conceptually defined.

They are not yet implemented in a reference engine.

The framework describes contracts as a way to validate whether outputs are fit for purpose.

Example contract expectations:

```text
PlotOutline:
- includes premise;
- includes major characters;
- includes act structure;
- includes climax;
- includes resolution;
- is consistent with story requirements.
```

```text
TechnicalRequirementsRegister:
- includes functional requirements;
- includes non-functional requirements;
- marks requirements as explicit, inferred or unresolved;
- includes assumptions;
- includes risks;
- includes open questions;
- avoids unsupported claims.
```

Current status:

```text
Conceptual design complete enough for discussion.
Needs schema, implementation and examples.
```

Recommended next steps:

- define a contract schema;
- support basic required-section validation;
- support deterministic validators;
- support LLM review validators;
- record validation results in execution traces.

---

## Execution Trace status

Execution Trace is conceptually defined.

No stable trace schema currently exists.

A future trace schema should probably record:

- run ID;
- source module ID;
- source module version;
- engine version;
- input identifiers;
- input hashes;
- resolved variables;
- iterator expansions;
- operation instances;
- operation statuses;
- model provider;
- model name;
- model settings;
- prompts, where appropriate;
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

Current status:

```text
Conceptual only.
Needs JSON schema or equivalent.
```

---

## Examples status

The framework draft describes several example workflows.

These include:

- article summary and review;
- legacy project recovery;
- novel development;
- video game Technical Design Document generation;
- compliance and policy review;
- cyclic research and monitoring;
- module authoring;
- module improvement.

Recommended repository examples:

```text
examples/article-summary-review/
examples/legacy-project-recovery/
examples/novel-generation/
examples/video-game-tdd/
examples/compliance-gap-analysis/
examples/module-improvement/
```

Current status:

```text
Examples exist in explanatory form.
Executable TTML examples need to be created or refined.
```

---

## Known limitations

### 1. The project is conceptually ahead of the implementation

The framework documentation is much more developed than the code.

### 2. TTML is not final

The TTML draft has changed over time and still needs stabilisation.

### 3. Unity may not be the ideal long-term environment

The existing prototypes are in Unity/C#, but a reference Cognitive Engine may be better implemented as:

- a CLI tool;
- a Python package;
- a TypeScript package;
- a .NET command-line application;
- or a language-neutral specification with multiple engines.

This is an open design question.

### 4. The framework overlaps with existing AI workflow and agent tools

Thought Tree should be compared carefully with:

- LangGraph;
- LangChain;
- AutoGen;
- CrewAI;
- Semantic Kernel;
- workflow DAG engines;
- agent orchestration systems.

The project needs clear positioning.

### 5. Semantic validation is difficult

Semantic Contracts are important but non-trivial.

Some validation can be deterministic, but much of it may require LLM review or human judgement.

### 6. Dynamic execution requires governance

`DynamicCompletion` and `ProjectCompletion` are powerful but risky.

Generated submodules should be validated and traced before execution.

### 7. No community or maintainer base currently exists

This is a handoff release, not an active mature open-source project.

---

## Open questions

Future contributors may need to answer:

1. Should TTML remain the primary authoring format?
2. Should there be a simpler YAML or JSON authoring syntax?
3. What language should a reference Cognitive Engine be written in?
4. What should the minimal conformance level be?
5. How should semantic contracts be represented?
6. How should execution traces be stored?
7. How should module libraries work?
8. How should dynamic submodule generation be governed?
9. How should Thought Tree relate to existing agent frameworks?
10. Should the framework become a specification, a tool, a library, or an ecosystem?
11. What is the smallest useful implementation?
12. What examples best demonstrate the value?
13. What licence model best supports free use and adoption?

---

## Suggested immediate priorities

For anyone continuing the project, the most useful first priorities are:

### Priority 1: Create a minimal public specification

Extract the core ideas into short Markdown files:

- one-page overview;
- Program Model;
- TTML Draft;
- Cognitive Engine;
- Execution Semantics;
- Examples.

### Priority 2: Create a minimal executable example

Start with article summary:

```text
source_article
↓
DraftSummary
↓
ReviewSummary
↓
ReviseSummary
↓
final_summary
```

This is the simplest useful Thought Tree pattern.

### Priority 3: Build a tiny reference runner

A minimal runner should:

- read a simple TTML file;
- execute operations in order;
- call an LLM for `TextCompletion`;
- write outputs to a workspace;
- record a basic trace.

### Priority 4: Add deterministic functions

Start with:

- `CopyFile`;
- `ConcatenateFiles`;
- `ValidateXMLAgainstXSD`;
- `CreateArchive`.

### Priority 5: Add iterator and collection support

This enables more interesting workflows such as chapter generation, batch review and document compilation.

### Priority 6: Define trace format

A trace format makes the framework auditable and testable.

### Priority 7: Add semantic contracts

Start simple:

- required sections;
- expected format;
- LLM review rubric;
- pass / warning / fail result.

---

## Possible roadmap

A practical roadmap could be:

```text
Phase 1 — Documentation extraction
Phase 2 — TTML draft stabilisation
Phase 3 — Minimal reference engine
Phase 4 — Execution trace schema
Phase 5 — Iterator and collection support
Phase 6 — Semantic contracts
Phase 7 — Module examples
Phase 8 — Testing and validation
Phase 9 — Module libraries
Phase 10 — Dynamic submodule generation
Phase 11 — Human review and governance
Phase 12 — Production engines and ecosystem
```

---

## Current author involvement

The original author is stepping back and should not be assumed to be an active maintainer.

The project is open for:

- forks;
- independent implementations;
- critique;
- documentation improvements;
- specification work;
- experimental engines;
- example modules;
- research;
- adoption by other projects.

If future maintainers emerge, they should feel free to create their own roadmap, implementation strategy and governance structure.

---

## Contribution opportunities

Good first contributions could include:

- convert sections of the PDF into Markdown;
- create a one-page overview;
- create a minimal TTML example;
- draft a TTML XSD;
- write a JSON execution trace schema;
- build a simple Python TTML runner;
- build a simple C#/.NET CLI runner;
- write a comparison with LangGraph or AutoGen;
- create diagrams of the Cognitive Engine architecture;
- define semantic contract examples;
- create example workflows;
- build a Graphviz visualiser for TTML;
- write a linter for unresolved FileRefs;
- write tests for iterator expansion;
- write tests for output collision detection.

---

## Summary of current readiness

| Area | Status |
|---|---|
| Core concept | Substantially developed |
| Long-form documentation | Draft complete |
| Program Model | Conceptually defined |
| TTML | Draft, unstable |
| TTML schema | Needs work |
| Proof of concept | Exists |
| Cognitive Engine | Partial prototype |
| Reference engine | Not yet |
| Semantic contracts | Conceptual |
| Execution trace | Conceptual |
| Examples | Described, need executable versions |
| Module library | Not yet |
| Governance | Handoff mode |
| Production readiness | Not production ready |

---

## Final status note

Thought Tree should currently be treated as:

```text
A substantial open framework draft with prototype evidence,
not a finished software product.
```

Its main value is the conceptual model:

> structured cognitive programs made from concepts, artefacts, transformations, contracts and traces.

The next major milestone is a minimal reference implementation that proves the framework can execute simple TTML modules end-to-end.
