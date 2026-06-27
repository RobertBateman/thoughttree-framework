# Thought Tree Program Model

This document describes the abstract Thought Tree Program Model.

Documentation, specifications and examples in this repository are released under CC0 unless otherwise stated. Code is released under the MIT License.

---

## 1. Overview

The Thought Tree Program Model is the conceptual model underlying the Thought Tree Framework.

It defines how cognitive work is represented before it is serialised into TTML, compiled by a Cognitive Engine, or executed using LLMs, deterministic functions, tools, submodules and human review.

The core idea is:

```text
Data Units → Operations → Data Units
```

A Thought Tree program describes a structured process for transforming meaningful inputs into meaningful outputs.

Those inputs and outputs may be:

- documents;
- prompts;
- plans;
- requirements;
- summaries;
- reviews;
- datasets;
- media assets;
- structured records;
- generated modules;
- validation reports;
- human decisions.

The program model treats these not merely as files, but as concept-bearing artefacts connected by explicit relationships and transformed through Operations.

---

## 2. Purpose of the Program Model

The Program Model exists to make LLM-assisted work:

- explicit;
- inspectable;
- modular;
- reusable;
- model-independent;
- traceable;
- validatable;
- improvable.

A Thought Tree program should not be treated as a single prompt or a hidden agent conversation.

It should be understood as an executable cognitive process:

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

The Program Model is independent of any particular source syntax.

TTML is currently the main serialisation format, but YAML, JSON, visual graph editors or other syntaxes could compile to the same model.

---

## 3. Core Entities

The main entities in the Thought Tree Program Model are:

- Concept;
- Data Unit;
- Semantic Type;
- Operation;
- Relationship;
- Module;
- Collection;
- Variable;
- Iterator;
- Contract;
- Workspace;
- Execution Trace;
- Cognitive Transformation Graph.

These entities allow cognitive work to be represented as an executable structure rather than an informal instruction.

---

## 4. Concept

A Concept is a meaningful entity within a cognitive workflow.

Concepts are the things the workflow is about, creates, transforms, reviews or validates.

Examples:

- Article;
- Summary;
- Character;
- Plot Outline;
- Setting Overview;
- Chapter Draft;
- Technical Requirement;
- Risk Register;
- Policy Obligation;
- Evidence Map;
- Game Mechanic;
- Technical Design Document;
- Review Report;
- Generated TTML Module.

A Concept is semantic. It describes what something means in the workflow.

A Concept may be represented by a Data Unit.

Example:

```text
Concept: PlotOutline
Data Unit: plot_outline.txt
Semantic Type: PlotOutline
Physical Representation: txt
```

The file is the storage representation. The Concept is the meaning.

---

## 5. Data Unit

A Data Unit is a discrete artefact that can be supplied to, produced by, referenced by, reviewed by or transformed by a Thought Tree program.

In TTML, Data Units are commonly represented as files:

```xml
<File id="plot_outline" extension="txt"/>
```

or referenced using:

```xml
<FileRef id="plot_outline"/>
```

However, in the abstract Program Model, a Data Unit may be any addressable unit of information.

Examples:

- text document;
- Markdown file;
- JSON object;
- XML document;
- PDF;
- image;
- audio file;
- video file;
- database record;
- API response;
- vector search result;
- generated prompt;
- generated module;
- validation report;
- human review decision;
- in-memory object.

A Data Unit may have:

```text
id
content
physical representation
semantic type
contract
version
provenance
validation status
```

Example:

```text
Identifier: plot_outline
Physical representation: plot_outline.txt
Semantic type: PlotOutline
Contract: PlotOutlineContract
Produced by: GeneratePlotOutline
Inputs used: story_requirements, character_system_brief, setting_overview
Version: 1
Validation status: passed
```

A Data Unit is not merely a file. It is a logical artefact in the cognitive program.

---

## 6. Semantic Type

A Semantic Type defines what kind of meaningful artefact a Data Unit represents.

The physical representation tells the engine how the artefact is stored.

The semantic type tells the engine, author or reviewer what the artefact is meant to be.

Examples:

```text
plot_outline.txt
physical representation: text/plain
semantic type: PlotOutline
```

```text
risk_register.md
physical representation: markdown
semantic type: RiskRegister
```

```text
chapter_3_review.md
physical representation: markdown
semantic type: ChapterReview
```

Semantic Types are useful because many different artefacts may share the same physical format while serving different cognitive roles.

For example:

```text
plot_outline.txt
character_profile.txt
risk_register.txt
```

are all text files, but they represent different semantic types.

Semantic Types support:

- clearer operation definitions;
- stronger validation;
- better error messages;
- better module reuse;
- safer dynamic module generation;
- better contract attachment;
- better human review.

Possible semantic types include:

- SourceDigest;
- Summary;
- PlotOutline;
- CharacterProfile;
- SettingOverview;
- ChapterDraft;
- ReviewReport;
- CorrectionPlan;
- TechnicalRequirementsRegister;
- TechnicalDesignDocument;
- ComplianceGapAnalysis;
- RiskRegister;
- EvidenceMap;
- TTMLModule;
- ValidationReport.

---

## 7. Operation

An Operation is a transformation applied to zero or more input Data Units to produce one or more output Data Units.

The basic pattern is:

```text
Input Data Units → Operation → Output Data Units
```

Examples:

```text
source_article → SummariseArticle → draft_summary
```

```text
draft_chapter + feedback → ReviseChapter → revised_chapter
```

```text
all_revised_chapters → ConcatenateFiles → compiled_manuscript
```

Operations are the active units of a Thought Tree program.

An Operation may be executed by:

- an LLM;
- a deterministic function;
- an external tool;
- another Module;
- a generated submodule;
- a human reviewer;
- or a combination of these.

An Operation has at least:

```text
id
type
description
inputs
outputs
```

A more advanced Operation may also define:

```text
preconditions
postconditions
contracts
model capability requirements
tool requirements
retry strategy
review requirements
cost constraints
safety constraints
```

Example:

```text
Operation: ReviseChapter
Type: TextCompletion
Inputs:
  - draft_chapter_1
  - feedback_for_chapter_1
Output:
  - revised_chapter_1
Description:
  Revise the chapter using all feedback while preserving continuity with the plot outline.
Contract:
  RevisedChapterContract
Relationship:
  revised_chapter_1 revises draft_chapter_1
```

---

## 8. Operation Types

The Program Model supports multiple operation types.

The current TTML draft includes:

- TextCompletion;
- ExecuteFunction;
- PreExisting;
- ProjectCompletion;
- DynamicCompletion.

Future operation types may include:

- HumanReview;
- ToolCall;
- ValidationGate;
- Condition;
- Loop;
- Schedule;
- WaitForInput.

---

### 8.1 TextCompletion

A `TextCompletion` operation invokes an LLM to perform semantic work.

Suitable tasks include:

- summarisation;
- analysis;
- drafting;
- review;
- revision;
- classification;
- synthesis;
- extraction;
- ideation;
- planning.

Example:

```text
Input: story_requirements
Operation: DraftPlotOutline
Output: plot_outline
```

TTML-style example:

```xml
<Operation
  id="DraftPlotOutline"
  type="TextCompletion"
  desc="Draft a coherent plot outline from the story requirements.">
  <FileRef id="story_requirements"/>
  <Output>
    <File id="plot_outline" extension="txt"/>
  </Output>
</Operation>
```

---

### 8.2 ExecuteFunction

An `ExecuteFunction` operation invokes deterministic code.

Suitable tasks include:

- copying files;
- concatenating files;
- validating XML;
- converting formats;
- resizing images;
- querying databases;
- creating archives;
- generating checksums.

Example:

```text
Input: all_revised_chapters
Operation: ConcatenateFiles
Output: compiled_novel_manuscript
```

TTML-style example:

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

This prevents LLMs from being used for tasks ordinary software can perform more reliably.

---

### 8.3 PreExisting

A `PreExisting` operation executes another Thought Tree Module.

This allows modules to be recursively composed.

Example:

```text
NovelProduction
  calls ConceptDevelopment
```

TTML-style example:

```xml
<Operation
  id="ConceptDevelopment"
  type="PreExisting"
  desc="Run the concept development submodule."
  target="/subprojects/ConceptDevelopment.ttml">
  <FileRef id="story_requirements"/>
  <Output>
    <File id="plot_outline" extension="txt"/>
    <File id="setting_overview" extension="txt"/>
  </Output>
</Operation>
```

The parent Module treats the submodule as an Operation. The submodule may itself contain many Operations.

---

### 8.4 ProjectCompletion

A `ProjectCompletion` operation expands a complex task into a generated submodule.

This is useful when a task is too large or ambiguous for one direct completion.

Example:

```text
Develop the character, setting and plot concepts for this novel.
```

could become a generated submodule containing:

- analyse requirements;
- create character system;
- generate character profiles;
- develop setting overview;
- draft plot outline;
- review consistency;
- revise concept documents.

Recommended execution pattern:

```text
ProjectCompletion Operation
        ↓
Generate Proposed Submodule
        ↓
Validate Submodule
        ↓
Optional Human Approval
        ↓
Execute Submodule
        ↓
Map Outputs Back to Parent Operation
```

Generated submodules should be preserved as artefacts and recorded in the execution trace.

---

### 8.5 DynamicCompletion

A `DynamicCompletion` operation allows the Cognitive Engine to choose the execution strategy.

It may resolve into:

- a direct TextCompletion;
- an ExecuteFunction call;
- a PreExisting Module call;
- a generated ProjectCompletion submodule;
- a sequence of smaller operations;
- a combination of strategies.

DynamicCompletion is powerful but introduces ambiguity.

Therefore, the engine should record:

- the selected strategy;
- generated operations or modules;
- validation results;
- human approval, if required;
- final output mappings.

DynamicCompletion should not bypass validation, traceability or security controls.

---

## 9. Relationship

A Relationship describes how Concepts, Data Units, Operations or Modules are connected.

Relationships turn isolated operations into a coherent cognitive program.

Common relationships include:

```text
depends on
produces
derived from
reviews
revises
validates
aggregates
decomposes
implements
references
replaces
version of
```

Examples:

```text
requirements_analysis is derived from story_requirements
plot_outline depends on requirements_analysis
chapter_1_draft depends on plot_outline
chapter_1_review reviews chapter_1_draft
revised_chapter_1 revises chapter_1_draft
final_novel aggregates revised chapters
```

Relationships may be represented:

- explicitly;
- implicitly through operation inputs and outputs;
- through Collections;
- through execution traces;
- through provenance metadata.

The Cognitive Engine uses relationships to:

- resolve dependencies;
- build execution graphs;
- detect missing inputs;
- prevent output collisions;
- support traceability;
- enable debugging;
- support semantic review.

---

## 10. Module

A Module is a reusable cognitive program.

A Module defines a bounded process that receives inputs, performs Operations and produces outputs.

A Module may represent:

- a simple task;
- a complex production workflow;
- a reusable sub-process;
- a generated execution plan;
- an artefact to be reviewed or improved.

A Module may contain:

```text
metadata
inputs
variables
iterators
collections
operations
contracts
final outputs
```

Example Module:

```text
ArticleSummaryReview
Inputs:
  - source_article

Operations:
  - DraftSummary
  - ReviewSummary
  - ReviseSummary

Final Output:
  - final_summary
```

At the conceptual level:

```text
source_article
        ↓
DraftSummary
        ↓
draft_summary
        ↓
ReviewSummary
        ↓
summary_review
        ↓
ReviseSummary
        ↓
final_summary
```

Modules are recursively composable:

```text
Module
  └── Operation
        └── Submodule
              └── Operation
                    └── Submodule
```

This recursive structure is one of the main strengths of the framework.

---

## 11. Collection

A Collection is a named group of Data Units treated as one logical input or output.

Collections are useful when an Operation needs to consume or produce multiple related artefacts.

Examples:

- all character profiles;
- all revised chapters;
- all feedback reports for one chapter;
- all TDD sections;
- all policy evidence files.

TTML-style example:

```xml
<Collection id="all_revised_chapters" orderBy="ChapterIterator">
  <FileRef id="revised_chapter_{{ChapterIterator}}"/>
</Collection>
```

This may represent:

```text
revised_chapter_1
revised_chapter_2
...
revised_chapter_12
```

A later operation can consume the collection:

```xml
<CollectionRef id="all_revised_chapters"/>
```

Important rule:

```text
A CollectionRef should not automatically cause the consuming Operation to execute once per Collection member.
```

Instead, the Collection is supplied as grouped context.

Collections support the pattern:

```text
Iterate → Aggregate → Transform
```

Example:

```text
feedback_chapter_1_persona_1
feedback_chapter_1_persona_2
feedback_chapter_1_persona_3
        ↓
feedback_for_chapter_1
        ↓
ReviseChapter
        ↓
revised_chapter_1
```

Collections help avoid accidental output collisions.

---

## 12. Variable

A Variable is a named reusable value.

Variables may configure:

- iterator ranges;
- output names;
- operation descriptions;
- module settings;
- contract parameters;
- model or tool configuration.

Example:

```xml
<Vars>
  <Var name="ChapterCount" type="integer" value="12"/>
</Vars>
```

Referenced as:

```text
{{ChapterCount}}
```

Variables are generally local to the current Module unless explicit parameter passing or inheritance is defined.

---

## 13. Iterator

An Iterator defines repeated execution over a range or set of values.

Example:

```xml
<Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
```

If `ChapterCount` is `12`, then:

```text
ChapterIterator = 1, 2, 3, ..., 12
```

An iterator can be used in identifiers:

```xml
<File id="draft_chapter_{{ChapterIterator}}" extension="txt"/>
```

which expands to:

```text
draft_chapter_1
draft_chapter_2
...
draft_chapter_12
```

Iterators are useful for repeated structures such as:

- chapters;
- scenes;
- characters;
- personas;
- test cases;
- documents;
- requirements;
- risks;
- assets;
- variants.

The Cognitive Engine is responsible for expanding iterators into concrete operation instances before execution.

---

## 14. Active Iterator

An Active Iterator is an iterator that causes an Operation to expand.

An iterator becomes active for an Operation when it appears directly in:

- an input Data Unit reference;
- an output Data Unit declaration;
- a CollectionRef identifier;
- an operation description or attribute, if supported by the engine.

An iterator used only inside the internal member definition of a Collection does not necessarily cause the consuming Operation to expand.

This distinction is important.

Example:

```xml
<Collection id="all_revised_chapters" orderBy="ChapterIterator">
  <FileRef id="revised_chapter_{{ChapterIterator}}"/>
</Collection>
```

A later operation using:

```xml
<CollectionRef id="all_revised_chapters"/>
```

should normally run once, receiving the whole collection, not once per chapter.

---

## 15. Multiple Iterators

If an Operation references multiple active iterators, the default behaviour is Cartesian expansion.

Example:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="beta_reader_persona_{{PersonaIterator}}"/>
<Output>
  <File id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}" extension="txt"/>
</Output>
```

If there are 12 chapters and 3 personas, this expands to 36 operation instances:

```text
Chapter 1, Persona 1
Chapter 1, Persona 2
Chapter 1, Persona 3
Chapter 2, Persona 1
...
Chapter 12, Persona 3
```

This may be intended.

However, if the output omits one active iterator, collisions may occur.

Bad pattern:

```xml
<FileRef id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}"/>
<Output>
  <File id="revised_chapter_{{ChapterIterator}}" extension="txt"/>
</Output>
```

Here, multiple persona-specific executions may attempt to write the same `revised_chapter_n`.

Valid resolutions include:

1. include all active iterators in the output identifier;
2. aggregate one iterator dimension into a Collection;
3. add a separate aggregation Operation;
4. create versioned outputs;
5. explicitly declare overwrite behaviour;
6. require review or resolution.

Silent overwriting should be invalid by default.

---

## 16. Contract

A Contract defines what a Data Unit or Operation output must satisfy to be considered valid.

A Contract may specify:

- expected semantic type;
- physical format;
- required sections;
- required fields;
- formatting rules;
- completeness criteria;
- quality criteria;
- consistency requirements;
- prohibited content;
- source traceability requirements;
- downstream usage expectations;
- deterministic validation functions;
- LLM review rubrics;
- human approval requirements.

Contracts allow the framework to distinguish between:

```text
A file exists.
```

and:

```text
The right cognitive artefact was produced.
```

Example PlotOutline contract:

```text
Semantic Type: PlotOutline

Required Sections:
- Premise
- Main Characters
- Setting
- Act I
- Act II
- Act III
- Climax
- Resolution
- Unresolved Questions

Quality Criteria:
- central conflict is clear
- protagonist motivation is explicit
- ending resolves the main conflict
- outline is consistent with story requirements
- outline is detailed enough to support chapter drafting
```

Contracts support semantic validation.

---

## 17. Validation Levels

The Thought Tree Program Model distinguishes three levels of correctness.

### 17.1 Schema Correctness

The source definition is structurally valid.

For TTML, this means the XML conforms to the TTML schema.

Question:

```text
Is this a valid source document?
```

### 17.2 Execution Correctness

The Cognitive Engine can resolve dependencies, execute operations and produce declared outputs.

Question:

```text
Can this program run?
```

### 17.3 Semantic Correctness

The outputs satisfy their intended meaning, quality and downstream use.

Question:

```text
Did the program produce the right kind of artefact?
```

Semantic correctness may require contracts, validators, LLM review or human approval.

---

## 18. Workspace

A Workspace is the execution environment where Data Units and execution artefacts are stored or referenced.

A Workspace may contain:

- root inputs;
- intermediate outputs;
- final outputs;
- generated prompts;
- generated modules;
- function outputs;
- tool outputs;
- validation reports;
- error reports;
- human review decisions;
- execution traces.

A Workspace may be implemented as:

- a local directory;
- cloud storage;
- a database;
- an in-memory context;
- a version-controlled repository;
- a hybrid asset store.

The Workspace maps logical identifiers to concrete assets.

Example:

```text
Logical Identifier: plot_outline
Concrete Asset: /workspaces/run_001/outputs/plot_outline.txt
```

or:

```text
asset_id: plot_outline
run_id: run_001
version: 1
semantic_type: PlotOutline
produced_by: GeneratePlotOutline
```

The implementation may vary, but the mapping should remain traceable.

---

## 19. Provenance

Provenance records where a Data Unit came from and how it was produced.

For each Data Unit, provenance may include:

- producing Operation;
- source inputs;
- previous versions;
- semantic type;
- contract;
- validation status;
- timestamp;
- model used;
- function used;
- tool used;
- human reviewer;
- output hash.

Provenance allows users to answer:

```text
Where did this artefact come from?
Which inputs produced it?
Which operation generated it?
Was it reviewed?
Was it validated?
Which downstream outputs used it?
```

Provenance is essential for auditability and improvement.

---

## 20. Execution Trace

An Execution Trace records what happened during a Thought Tree run.

A trace may include:

- source module ID;
- source module version;
- schema version;
- engine version;
- input identifiers;
- input hashes;
- resolved variables;
- iterator expansions;
- dependency graph;
- execution plan;
- operation start and end times;
- operation statuses;
- active iterator values;
- prompts;
- model provider;
- model name;
- model settings;
- function calls;
- tool calls;
- submodule executions;
- generated modules;
- output identifiers;
- output hashes;
- validation results;
- contract results;
- retries;
- errors;
- human review decisions;
- final outputs.

The trace makes the workflow inspectable, auditable, debuggable and improvable.

---

## 21. Cognitive Transformation Graph

Although Thought Tree Modules may be visualised as trees, the underlying execution structure is usually a graph.

In the Cognitive Transformation Graph:

- Data Units are artefact nodes;
- Operations are transformation nodes;
- Collections are grouped artefact nodes or references;
- Modules are reusable subgraphs;
- Contracts are validation rules;
- Relationships are edges;
- Traces record execution events.

Simple example:

```text
source_article
        ↓
DraftSummary
        ↓
draft_summary
        ↓
ReviewSummary
        ↓
summary_review
        ↓
ReviseSummary
        ↓
final_summary
```

Branching example:

```text
source_digest
  ├── ExtractRequirements → requirements_register
  ├── ExtractTimeline     → project_timeline
  └── ExtractRisks        → risk_register
```

Aggregation example:

```text
revised_chapter_1
revised_chapter_2
...
revised_chapter_12
        ↓
all_revised_chapters
        ↓
CompileNovel
        ↓
compiled_manuscript
```

The graph allows the Cognitive Engine to reason about execution order, parallelism, dependency resolution, validation, provenance and final output resolution.

---

## 22. Minimal Abstract Example

A minimal Thought Tree program:

```text
Input:
- source_article

Operations:
1. DraftSummary
   Input: source_article
   Output: draft_summary

2. ReviewSummary
   Inputs: source_article, draft_summary
   Output: summary_review

3. ReviseSummary
   Inputs: source_article, draft_summary, summary_review
   Output: final_summary

Final Output:
- final_summary
```

This is more than a single prompt.

Instead of:

```text
Summarise this article.
```

the program defines:

```text
draft → review → revise → final
```

It preserves intermediate artefacts and makes the process inspectable.

---

## 23. Dynamic and Recursive Programs

A Thought Tree Module may call or generate other Modules.

This enables recursive cognitive programming.

Patterns include:

```text
Operation calls pre-existing Module
```

```text
Operation generates new Module
```

```text
Module reviews another Module
```

```text
Module improves another Module
```

Example self-improvement flow:

```text
original_ttml
        ↓
ValidateSchema
        ↓
ValidateExecution
        ↓
ReviewDesign
        ↓
GenerateImprovementPlan
        ↓
DraftImprovedTTML
        ↓
ValidateImprovedTTML
        ↓
CompareVersions
        ↓
final_improved_ttml
```

This does not mean the system is automatically correct. Generated or improved modules still require validation and, where appropriate, human review.

---

## 24. Model Independence

A Thought Tree program should describe the intended cognitive process without depending unnecessarily on a specific LLM provider.

Instead of:

```text
Use Model X from Provider Y.
```

a Module should generally describe capability requirements:

```text
requires long-context reasoning
requires structured output
requires high-quality creative synthesis
requires low-cost summarisation
requires local execution for sensitive data
```

The Cognitive Engine maps these requirements to available models.

This separation allows the same Thought Tree program to run with:

- different LLM providers;
- local or remote models;
- different model sizes;
- different cost settings;
- different privacy policies;
- different deterministic function libraries;
- different execution environments.

---

## 25. Hybrid Execution

The Program Model is hybrid by design.

Different work should be handled by different execution resources.

LLMs are suitable for:

- ambiguous semantic transformation;
- language generation;
- analysis;
- synthesis;
- review;
- planning;
- creative drafting.

Deterministic functions are suitable for:

- validation;
- copying;
- concatenation;
- format conversion;
- checksums;
- database queries;
- archiving.

Tools are suitable for:

- retrieval;
- search;
- external APIs;
- rendering;
- code execution;
- asset generation.

Humans are suitable for:

- judgement;
- approval;
- correction;
- governance;
- high-impact review;
- creative direction.

The Cognitive Engine coordinates these execution resources.

---

## 26. Output Resolution

Each Operation declares the outputs it is expected to produce.

An Operation is complete when:

1. execution has finished;
2. all declared outputs exist;
3. outputs are stored or referenced in the Workspace;
4. required validation has passed;
5. no unrecovered errors remain.

A Module is complete when:

1. all required Operations have completed;
2. all declared final outputs can be resolved;
3. final outputs are available in the Workspace;
4. required contracts have passed, where supported;
5. no unrecovered errors remain;
6. the execution trace has been recorded.

If a final output cannot be resolved, execution should be considered incomplete or failed.

---

## 27. Versioning

Outputs should be treated as versioned artefacts unless explicitly overwritten.

If an Operation produces an output identifier that already exists, the engine should use one of the following strategies:

1. reject as an output collision;
2. create a new version;
3. overwrite only if explicitly permitted;
4. preserve both and require resolution;
5. invoke review or error handling.

Example file-based versioning:

```text
draft_chapter_1.v1.txt
draft_chapter_1.v2.txt
draft_chapter_1.v3.txt
```

Example logical versioning:

```text
Data Unit: draft_chapter_1

Version 1:
  status: failed_contract

Version 2:
  status: passed
  repaired_from: version 1
```

Versioning supports auditability, rollback, comparison and improvement.

---

## 28. Program Model Summary

The Thought Tree Program Model represents cognitive work as executable transformations over meaningful artefacts.

In compact form:

```text
Concepts
  are represented by
Data Units
  which are transformed by
Operations
  connected through
Relationships
  organised into
Modules
  compiled and executed by
Cognitive Engines
  producing
Outputs, Traces and Improved Artefacts
```

Or more simply:

```text
Data Units → Operations → Data Units
```

At small scale, this may describe a single draft-review-revise workflow.

At large scale, it can describe a modular cognitive production pipeline involving LLMs, deterministic functions, tools, submodules, human review, semantic contracts and execution traces.

---

## 29. Current Status

This Program Model is a draft.

It is intended to support:

- TTML as an initial source format;
- a future reference Cognitive Engine;
- semantic contracts;
- execution traces;
- module libraries;
- dynamic planning;
- module improvement workflows.

Open implementation questions include:

- exact TTML schema stabilisation;
- contract schema design;
- execution trace schema design;
- conformance levels;
- security model;
- module registry format;
- human review representation;
- visual graph representation;
- source format alternatives such as YAML or JSON.

---

## 30. Recommended Next Steps

For contributors, useful next steps include:

1. implement these entities as language-level data structures;
2. create a minimal TTML-to-Program-Model parser;
3. define JSON schemas for execution traces and contracts;
4. build a dependency graph compiler;
5. implement iterator expansion;
6. implement Collection resolution;
7. implement output collision detection;
8. create minimal example modules;
9. build a CLI dry-run mode;
10. compare the Program Model with existing workflow and agent frameworks.

A useful first implementation target is:

```text
Load a simple article summary TTML file,
compile it into this Program Model,
display the execution graph,
execute three TextCompletion operations,
store intermediate artefacts,
and write execution_trace.json.
```

---

## 31. Final Note

The Thought Tree Program Model is intended to move LLM-assisted work beyond isolated prompts and opaque agent runs.

It defines cognitive work as a structured, inspectable and improvable program made from:

```text
concepts,
artefacts,
relationships,
transformations,
contracts,
and traces.
```

This is the foundation of Thought Tree as a cognitive programming framework.