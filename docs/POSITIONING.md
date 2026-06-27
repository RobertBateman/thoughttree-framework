# Positioning

## What Thought Tree Is

The Thought Tree Framework is an open framework for defining, compiling, executing and improving structured LLM-assisted workflows.

It describes cognitive work as transformations over named artefacts:

```text
Data Units → Operations → Data Units
```

A Thought Tree Module defines:

- required inputs;
- intermediate artefacts;
- operations;
- dependencies;
- collections;
- iterators;
- validation expectations;
- final outputs;
- execution trace requirements.

A Cognitive Engine then compiles and executes the module using some combination of:

- LLMs;
- deterministic functions;
- external tools;
- submodules;
- human review.

The framework is best understood as an early model for **cognitive programming**: programming with concepts, documents, plans, requirements, reviews, contracts and transformations between them.

---

## What Thought Tree Is Not

Thought Tree is not intended to be:

- just a prompt template format;
- just a prompt chaining convention;
- just an agent framework;
- just a visual workflow tool;
- just a document automation system;
- just a Unity project;
- just a novel-generation prototype;
- just an XML schema.

TTML is one possible source format for Thought Tree programs.

The deeper idea is the program model:

```text
Concepts
represented as Data Units
transformed by Operations
organised into Modules
validated by Contracts
executed by a Cognitive Engine
recorded through Traces
```

---

## Compared With Prompt Engineering

### Prompt Engineering

Prompt engineering focuses on crafting instructions for a model.

A typical pattern is:

```text
User → Prompt → LLM → Output
```

This is useful for simple tasks, but the process is usually implicit.

The prompt may contain instructions such as:

- analyse this;
- summarise this;
- draft this;
- review this;
- improve this.

But the structure of the work is not usually represented as an executable artefact with named intermediate outputs, dependencies, validation stages and traces.

### Thought Tree

Thought Tree treats prompts as one part of a larger cognitive program.

A Thought Tree workflow may look like:

```text
source material
↓
source digest
↓
requirements extraction
↓
draft output
↓
review report
↓
correction plan
↓
final output
↓
execution trace
```

The difference is:

```text
Prompting asks for an answer.
Thought Tree defines a process for producing, checking and improving an answer.
```

Prompts still matter, but they are embedded inside a structured workflow.

---

## Compared With Prompt Chains

### Prompt Chains

Prompt chains connect multiple prompts together.

For example:

```text
Prompt 1 → Output 1 → Prompt 2 → Output 2 → Prompt 3 → Final Output
```

This is closer to Thought Tree than a single prompt, but prompt chains are often implementation-specific and informal.

They may not explicitly define:

- logical artefact identifiers;
- semantic types;
- operation types;
- grouped collections;
- iterator expansion;
- deterministic functions;
- output collision rules;
- validation gates;
- execution traces;
- reusable modules;
- semantic contracts.

### Thought Tree

Thought Tree formalises the chain as a cognitive program.

Instead of treating outputs as anonymous strings passed between prompts, it treats them as named Data Units with meaning, provenance and downstream use.

For example:

```text
story_requirements
→ AnalyzeRequirements
→ story_requirements_analysis
→ DraftPlotOutline
→ plot_outline
→ ReviewPlotOutline
→ plot_outline_review
→ RevisePlotOutline
→ final_plot_outline
```

Each artefact can be inspected, validated, replaced, reused or traced.

The key distinction is:

```text
Prompt chains connect model calls.
Thought Tree connects semantic artefacts through explicit transformations.
```

---

## Compared With Autonomous Agents

### Autonomous Agents

Autonomous agents typically allow an LLM to:

- plan;
- act;
- observe;
- call tools;
- revise its plan;
- continue iterating.

This can be powerful, especially for exploratory tasks.

However, autonomous agents can be difficult to predict, debug or audit.

They may:

- drift from the original objective;
- lose track of decisions;
- repeat work unnecessarily;
- misuse tools;
- hide important intermediate reasoning;
- make unsupported assumptions;
- produce outputs whose provenance is unclear.

### Thought Tree

Thought Tree favours structured cognitive execution.

The process is explicitly represented before or during execution.

The Cognitive Engine manages:

- state;
- dependencies;
- artefacts;
- validation;
- errors;
- traces;
- intermediate outputs;
- final output resolution.

The LLM is used where it is strongest: semantic transformation over ambiguous material.

The engine provides structure and control.

A useful summary is:

```text
Agents let the model decide much of the process at runtime.
Thought Tree makes the process explicit, inspectable and reusable.
```

This does not mean Thought Tree rejects dynamic behaviour.

The framework includes concepts such as `DynamicCompletion` and `ProjectCompletion`, where an engine may choose a strategy or generate a submodule.

But dynamic behaviour should be:

- validated;
- recorded;
- optionally approved by a human;
- preserved in the execution trace.

The intended pattern is:

```text
Ambiguous task
↓
Generated plan or submodule
↓
Validation
↓
Optional human approval
↓
Execution
↓
Trace
```

Thought Tree can therefore include agent-like planning, but within a controlled execution model.

---

## Compared With LangChain / LangGraph

LangChain and LangGraph are implementation frameworks for building LLM applications, chains, agents and graph-based workflows.

They provide practical tools for:

- model calls;
- tool use;
- retrieval;
- graph orchestration;
- state management;
- agent workflows;
- integrations.

Thought Tree is positioned differently.

It is primarily a proposed program model and source/specification layer for cognitive workflows.

Its focus is on:

- named Data Units;
- semantic artefacts;
- explicit transformations;
- reusable Modules;
- TTML as a possible source format;
- Cognitive Engines as compiler/runtimes;
- semantic contracts;
- execution traces;
- module improvement;
- model independence.

A possible relationship is:

```text
Thought Tree could be implemented using LangGraph.
```

LangGraph could provide runtime orchestration.

Thought Tree could provide a higher-level cognitive program format and semantics.

In that sense, Thought Tree is not necessarily a competitor to LangChain or LangGraph.

It could be:

- an alternative conceptual model;
- a source format;
- a specification layer;
- a module format;
- an authoring approach;
- an execution semantics proposal;
- a pattern library for structured cognitive work.

---

## Compared With Microsoft Semantic Kernel

Semantic Kernel provides abstractions for AI orchestration, plugins, planners, memory and integration with conventional software.

It is useful for building AI applications that combine LLM calls with functions and external systems.

Thought Tree shares some broad concerns:

- combining LLMs with deterministic functions;
- separating semantic work from mechanical work;
- enabling reusable workflows;
- supporting orchestration.

The difference is emphasis.

Thought Tree focuses specifically on representing cognitive production workflows as explicit transformation graphs over named artefacts.

It emphasises:

- Data Units as concept-bearing artefacts;
- intermediate output preservation;
- TTML Modules;
- semantic contracts;
- execution traces;
- output provenance;
- module improvement workflows.

Semantic Kernel may be a useful implementation backend for parts of a Cognitive Engine.

Thought Tree is more concerned with the structure and semantics of the cognitive program itself.

---

## Compared With AutoGen / CrewAI / Multi-Agent Frameworks

Multi-agent frameworks such as AutoGen and CrewAI often model work as collaboration between agents with roles.

For example:

```text
Researcher Agent → Writer Agent → Reviewer Agent → Editor Agent
```

This can be intuitive and powerful.

However, role-based agent collaboration may still leave the artefact model under-specified.

Thought Tree focuses less on simulated agents and more on explicit artefact transformation.

A Thought Tree workflow asks:

- What artefacts exist?
- What operation transforms them?
- What outputs are produced?
- What depends on what?
- What should be reviewed?
- What should be validated?
- What trace is recorded?

The equivalent Thought Tree framing might be:

```text
source_material
→ ExtractResearchFindings
→ research_findings
→ DraftArticle
→ draft_article
→ ReviewArticle
→ article_review
→ ReviseArticle
→ final_article
```

The distinction is:

```text
Multi-agent frameworks organise work around actors.
Thought Tree organises work around artefacts, transformations, contracts and traces.
```

Both approaches can coexist.

A Cognitive Engine could implement an Operation using a multi-agent process internally, as long as the operation’s inputs, outputs and trace remain explicit.

---

## Compared With Traditional Workflow Engines

Traditional workflow engines are good at orchestrating deterministic processes.

They can coordinate:

- tasks;
- queues;
- approvals;
- API calls;
- scheduled jobs;
- business processes;
- file movement;
- data pipelines.

However, traditional workflow engines are not usually designed around ambiguous semantic transformation.

They are good at:

```text
known input → deterministic process → known output
```

LLM-assisted cognitive work often involves:

```text
ambiguous input → semantic interpretation → structured artefact
```

Thought Tree attempts to combine these worlds.

It treats LLM calls as semantic transformations inside a structured workflow.

It also allows deterministic functions where ordinary software is more reliable.

For example:

```text
LLM: extract requirements from notes
Code: validate JSON schema
LLM: review requirements for unsupported assumptions
Code: concatenate sections
Human: approve final report
```

The distinction is:

```text
Traditional workflow engines coordinate tasks.
Thought Tree coordinates cognitive transformations over meaningful artefacts.
```

---

## Compared With Document Automation

Document automation usually fills templates or assembles predefined content.

For example:

```text
form fields + template → generated document
```

This is valuable when the document structure and data are known.

Thought Tree is intended for cases where the structure may need to be inferred, synthesised, reviewed or revised from ambiguous material.

For example:

```text
scattered project notes
↓
source digest
↓
requirements register
↓
system decomposition
↓
draft technical design document
↓
review report
↓
final technical design document
```

Thought Tree can include document automation, but it is broader.

It can:

- analyse;
- extract;
- infer;
- draft;
- review;
- revise;
- validate;
- assemble;
- trace.

The distinction is:

```text
Document automation fills known structures.
Thought Tree defines cognitive processes that can create, review and improve structured outputs.
```

---

## Compared With Data Pipelines

Data pipelines transform structured or semi-structured data.

For example:

```text
raw data → cleaned data → transformed data → analytics table
```

Thought Tree is similar in spirit, but the objects are cognitive artefacts.

For example:

```text
raw notes → source digest → requirements register → design document
```

A data pipeline may validate types, schemas and row counts.

A Thought Tree workflow may validate:

- semantic type;
- completeness;
- source grounding;
- consistency;
- downstream readiness;
- human approval.

Thought Tree could be understood as a kind of semantic or cognitive pipeline system.

The distinction is:

```text
Data pipelines transform formal data.
Thought Tree pipelines transform concept-bearing artefacts.
```

---

## Compared With Visual No-Code AI Workflow Builders

No-code AI workflow builders make it easier to connect model calls, tools and integrations visually.

They are useful for accessibility and rapid prototyping.

Thought Tree is not primarily a UI concept.

It is a program model and execution semantics proposal.

A visual editor could be built for Thought Tree.

Such an editor might let users create:

```text
Data Unit nodes
Operation nodes
Collection nodes
Contract nodes
Module nodes
Trace views
```

But the visual editor would be one authoring interface, not the framework itself.

The distinction is:

```text
No-code tools provide an interface.
Thought Tree defines the underlying cognitive program structure.
```

---

## Compared With XML / YAML / JSON Workflow Formats

TTML is currently XML-based.

However, Thought Tree is not fundamentally about XML.

TTML is one possible serialisation of the underlying program model.

Future source formats could include:

- YAML;
- JSON;
- visual graphs;
- database-backed definitions;
- LLM-generated modules;
- domain-specific languages.

The relationship is:

```text
Thought Tree Program Model
↓
TTML / YAML / JSON / visual editor
↓
Cognitive Engine
↓
Execution graph
```

The distinction is:

```text
TTML is a source format.
Thought Tree is the framework and program model.
```

---

## Compared With BPMN

BPMN and related business process modelling notations define business workflows.

They are mature, standardised and useful for modelling organisational processes.

Thought Tree is narrower and more specific in one sense, but broader in another.

It is narrower because it focuses on LLM-assisted cognitive production.

It is broader because its primary artefacts may be ambiguous semantic objects rather than formal business events.

Thought Tree workflows are built around:

- documents;
- requirements;
- plans;
- reviews;
- drafts;
- generated modules;
- semantic contracts;
- LLM transformations.

BPMN may model an approval process.

Thought Tree may model the cognitive work that produces the artefact being approved.

The two could complement each other.

---

## Compared With Notebooks

Computational notebooks such as Jupyter combine code, text, outputs and experimentation.

They are excellent for exploratory analysis.

Thought Tree shares the idea of preserving intermediate outputs, but differs in intent.

A notebook is usually an interactive document.

A Thought Tree Module is intended to be an executable cognitive program.

It defines:

- inputs;
- operations;
- outputs;
- dependencies;
- validation;
- trace behaviour.

A notebook is often linear and human-driven.

A Thought Tree is designed to be compiled, validated and executed by a Cognitive Engine.

---

## Compared With Makefiles / Build Systems

There is a useful analogy between Thought Tree and build systems.

A build system defines how source files become output artefacts.

For example:

```text
source code → compiler → binary
markdown → converter → HTML
assets → packer → archive
```

Thought Tree applies a similar idea to cognitive artefacts:

```text
source notes → LLM analysis → source digest
source digest → LLM extraction → requirements register
requirements register → LLM drafting → design document
draft document → review operation → correction plan
correction plan → revision operation → final document
```

The Cognitive Engine is analogous to a compiler/runtime/build executor.

But unlike ordinary build systems, Thought Tree includes semantic transformations that may be probabilistic, ambiguous or judgement-based.

The distinction is:

```text
Build systems compile formal artefacts.
Thought Tree compiles and executes cognitive transformation graphs.
```

---

## Core Differentiators

Thought Tree’s distinctive ideas are:

### 1. Cognitive programming

The framework treats LLM-assisted work as programmable cognitive transformation, not just prompting.

### 2. Artefact-first design

The framework centres named Data Units and intermediate artefacts rather than anonymous model messages.

### 3. Hybrid execution

LLMs, deterministic functions, tools, submodules and humans can all be execution backends.

### 4. Cognitive Engine as compiler/runtime

The engine parses, validates, compiles, plans, executes, validates and traces the workflow.

### 5. Model independence

The program should describe the work, not hard-code one model provider.

### 6. Semantic contracts

Outputs can be checked for meaning, structure, completeness and downstream usefulness.

### 7. Execution traces

The process should be inspectable after execution.

### 8. Recursive modularity

Modules can call, generate, validate and improve other modules.

### 9. Module libraries

Reusable cognitive workflows can become shared or private assets.

### 10. Improvement process

The framework can be applied to its own modules through validation, review, revision and comparison.

---

## Where Thought Tree May Be Useful

Thought Tree is most relevant when work is:

- multi-step;
- document-heavy;
- ambiguous;
- quality-sensitive;
- repeatable;
- reviewable;
- auditable;
- modular;
- improved over time.

Potential domains include:

- technical documentation;
- project recovery;
- requirements extraction;
- compliance analysis;
- policy review;
- audit preparation;
- game content generation;
- creative writing pipelines;
- research synthesis;
- market monitoring;
- internal knowledge transformation;
- module generation;
- module improvement.

Thought Tree is less necessary for simple one-off tasks where a single prompt is sufficient.

---

## Where Thought Tree Is Probably Not Needed

Thought Tree may be unnecessary or excessive when:

- the task is simple;
- the output is low-impact;
- no intermediate artefacts are useful;
- traceability does not matter;
- validation does not matter;
- reuse is unlikely;
- a single prompt is good enough;
- a deterministic script can solve the whole problem.

The framework is not intended to replace simple prompting.

It is intended for larger cognitive processes where structure matters.

---

## Relationship to Existing Ecosystems

Thought Tree should not be positioned as requiring a completely separate ecosystem.

It could be implemented using existing tools.

Possible implementation backends include:

- LangGraph;
- LangChain;
- Semantic Kernel;
- custom Python runtimes;
- TypeScript runtimes;
- local-first LLM tooling;
- enterprise workflow engines;
- game engine tooling;
- document processing systems.

The Thought Tree contribution is the cognitive program model:

```text
Data Units
Operations
Modules
Collections
Iterators
Contracts
Traces
Cognitive Engine semantics
```

Different engines could implement these ideas in different ways.

---

## Open Source Positioning

The project is intended to be released openly and permissively.

Planned licensing approach:

- **Code:** MIT License
- **Documentation, specifications, examples and conceptual material:** CC0

The intent is to maximise reuse.

People should be free to:

- fork the project;
- implement independent Cognitive Engines;
- use or discard TTML;
- create alternative source formats;
- build commercial products;
- build open tools;
- adapt the terminology;
- create module libraries;
- compare the model with other frameworks;
- take the idea in directions not anticipated by the original author.

Thought Tree is best treated as an open starting point, not a controlled standard.

---

## Suggested Short Positioning Statement

> Thought Tree is an open framework for cognitive programming with LLMs. It defines complex LLM-assisted work as executable transformation graphs over named artefacts, with explicit operations, intermediate outputs, validation contracts and execution traces.

---

## Suggested Longer Positioning Statement

> Thought Tree is not another prompt format or agent loop. It is a proposed programming layer for structured cognitive work. A Thought Tree Module describes how concept-bearing artefacts such as notes, requirements, drafts, reviews and plans should be transformed into validated outputs. A Cognitive Engine compiles and executes that module using LLMs, deterministic functions, tools, submodules and human review, while preserving intermediate artefacts and recording an execution trace.

---

## Summary

Thought Tree sits between several existing categories:

```text
Prompting
Prompt chains
Agent frameworks
Workflow engines
Document automation
Data pipelines
Build systems
```

It borrows ideas from all of them, but applies them to a specific problem:

> How can complex LLM-assisted cognitive work be made explicit, inspectable, reusable, validatable and improvable?

The framework’s answer is:

```text
Represent cognitive work as programs made from Data Units, Operations, Modules, Contracts and Traces.
```

That is the core positioning.