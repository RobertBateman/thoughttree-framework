
# TTML Draft Specification

**Thought Tree Markup Language**  
**Draft version:** 0.12.0  
**Status:** Handoff draft / unstable  
**Repository path:** `spec/TTML_DRAFT.md`

---

## 1. Status of This Document

This document is a draft specification for **Thought Tree Markup Language**, or **TTML**.

TTML is the current XML-based source format for defining **Thought Tree Modules**: structured, executable, inspectable cognitive workflows that transform named Data Units through Operations.

This specification is not final. It is intended as a handoff document for contributors who may wish to:

- refine the TTML schema;
- implement a TTML parser;
- build a reference Cognitive Engine;
- write conformance tests;
- create examples;
- compare TTML with other workflow and agent formats;
- or evolve the format into something better.

The current draft should be treated as a starting point, not a settled standard.

---

## 2. Purpose of TTML

TTML provides a common XML-based format for describing LLM-assisted cognitive workflows.

A TTML document defines:

- what inputs are required;
- what variables and iterators are available;
- what Collections of Data Units exist;
- what Operations should be executed;
- what intermediate Data Units should be produced;
- what final outputs should be returned;
- enough structure for a Cognitive Engine to validate, execute and trace the workflow.

At its simplest, TTML expresses the core Thought Tree pattern:

```text
Data Units → Operations → Data Units
```

A TTML Module should not merely be a list of prompts. It should be a structured cognitive program.

---

## 3. Relationship to the Thought Tree Framework

TTML is one source representation of the underlying **Thought Tree Program Model**.

The conceptual relationship is:

```text
Thought Tree Program Model
↓
TTML source document
↓
Cognitive Engine parser / compiler
↓
Executable cognitive transformation graph
↓
LLMs / deterministic functions / tools / submodules / humans
↓
Intermediate artefacts, final outputs and execution trace
```

TTML is not the entire framework.

Future source formats may include:

- YAML;
- JSON;
- visual graph formats;
- database-backed module definitions;
- higher-level authoring languages;
- LLM-generated workflow plans.

Those formats could compile to the same underlying program model.

---

## 4. Design Goals

TTML is intended to be:

### 4.1 Human-readable

A human should be able to inspect a TTML file and understand:

- what the Module is for;
- what inputs it needs;
- what Operations it performs;
- what intermediate artefacts it produces;
- what final outputs it returns.

### 4.2 Machine-executable

A Cognitive Engine should be able to:

- parse TTML;
- validate its structure;
- resolve references;
- expand variables and iterators;
- detect missing dependencies;
- detect output collisions;
- execute Operations;
- produce declared outputs;
- record an execution trace.

### 4.3 LLM-authorable

TTML should be regular and explicit enough that an LLM can help generate, review, repair or improve TTML Modules.

This is important because one intended use case of the framework is generating and improving Thought Tree Modules using other Thought Tree Modules.

### 4.4 Model-independent

TTML should describe the cognitive workflow, not bind it unnecessarily to a particular LLM provider.

The Module defines the process.  
The Cognitive Engine decides how to execute that process using available models, functions, tools and policies.

### 4.5 Inspectable and traceable

Each significant transformation should have an identifier, inputs and outputs so that an execution trace can show how final artefacts were produced.

### 4.6 Extensible

TTML should remain simple enough for early implementations while leaving room for:

- semantic types;
- contracts;
- validation gates;
- human review;
- dynamic submodules;
- tool calls;
- conditional execution;
- scheduling;
- security policies;
- conformance levels.

---

## 5. High-Level Structure

A TTML document represents one Thought Tree Module.

A typical TTML document has this structure:

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

The major sections are:

| Section | Purpose |
|---|---|
| `<TTML>` | Root element and version declaration |
| `<Project>` | Module metadata |
| `<Inputs>` | Required starting Data Units |
| `<Vars>` | Reusable named values |
| `<Iterators>` | Repeated execution ranges |
| `<Collections>` | Named groups of Data Units |
| `<Operations>` | Transformations to execute |
| Root-level `<Output>` | Final exported Data Units |

---

## 6. Root Element: `<TTML>`

The root element declares the document as a TTML Module.

Example:

```xml
<TTML version="0.12.0">
  ...
</TTML>
```

### Required attributes

| Attribute | Description |
|---|---|
| `version` | TTML version expected by the Module |

A Cognitive Engine should record the TTML version in the execution trace.

---

## 7. Project Metadata: `<Project>`

The `<Project>` element describes the Module.

Example:

```xml
<Project
  id="ArticleSummaryModule"
  genre="Analysis"
  author="Example"
  date="2026-06-23"
  desc="Summarise an article through draft, review and revision." />
```

### Common attributes

| Attribute | Description |
|---|---|
| `id` | Module identifier |
| `desc` | Human-readable description |
| `author` | Author or originator |
| `date` | Creation or revision date |
| `genre` | Optional domain or category |

The `id` should be stable and descriptive.

The `desc` should explain the transformation performed by the Module.

---

## 8. Inputs: `<Inputs>`

The `<Inputs>` section declares Data Units that must be available before execution begins.

Example:

```xml
<Inputs>
  <File id="source_article" folder="/inputs" extension="txt"/>
</Inputs>
```

An input Data Unit may be supplied by:

- the user;
- a parent Module;
- a workspace;
- an external resolver;
- a test harness;
- another execution environment.

A Cognitive Engine must resolve required inputs before executing dependent Operations.

---

## 9. Data Units: `<File>` and `<FileRef>`

In the current TTML draft, Data Units are usually represented as files.

### 9.1 `<File>`

A `<File>` declares a Data Unit.

Example:

```xml
<File id="plot_outline" extension="txt"/>
```

Common attributes:

| Attribute | Description |
|---|---|
| `id` | Logical Data Unit identifier |
| `folder` | Optional folder or workspace hint |
| `extension` | Physical representation or file extension |

The `id` is a logical identifier, not merely a filename.

The Cognitive Engine maps the logical identifier to a concrete asset in the workspace.

For example:

```xml
<File id="story_requirements" folder="/inputs" extension="txt"/>
```

may resolve to:

```text
/workspaces/run_001/inputs/story_requirements.txt
```

or to an equivalent database-backed asset.

### 9.2 `<FileRef>`

A `<FileRef>` refers to a Data Unit already declared, supplied or produced.

Example:

```xml
<FileRef id="plot_outline"/>
```

Before executing an Operation, a Cognitive Engine must resolve each required `FileRef`.

A `FileRef` may resolve to:

- a root-level input;
- an output from an earlier Operation;
- a workspace asset;
- a submodule output;
- an externally resolved asset;
- a generated or repaired asset, if explicitly allowed.

If a required `FileRef` cannot be resolved, execution should fail validation unless the Module or engine explicitly defines a recovery strategy.

---

## 10. Variables: `<Vars>` and `<Var>`

Variables provide reusable values inside a Module.

Example:

```xml
<Vars>
  <Var name="ChapterCount" type="integer" value="12"/>
</Vars>
```

Variables may be referenced using double curly braces:

```text
{{ChapterCount}}
```

Example:

```xml
<Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
```

Common variable attributes:

| Attribute | Description |
|---|---|
| `name` | Variable name |
| `type` | Variable type |
| `value` | Variable value |

Suggested primitive types include:

- `string`;
- `integer`;
- `number`;
- `boolean`.

Variables are local to the current Module unless submodule parameter passing is explicitly defined by an engine or future TTML version.

A Cognitive Engine should fail validation if a variable reference cannot be resolved.

---

## 11. Iterators: `<Iterators>` and `<Iterator>`

Iterators define repeated execution ranges.

Example:

```xml
<Iterators>
  <Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
</Iterators>
```

If `ChapterCount` resolves to `12`, then `ChapterIterator` represents:

```text
1, 2, 3, ..., 12
```

Iterator values may be inserted into identifiers:

```xml
<File id="draft_chapter_{{ChapterIterator}}" extension="txt"/>
```

This represents:

```text
draft_chapter_1
draft_chapter_2
...
draft_chapter_12
```

### 11.1 Active iterators

An iterator becomes active for an Operation when it appears directly in:

- a scalar `<FileRef>` inside the Operation;
- a scalar `<File>` inside the Operation output;
- a `<CollectionRef>` identifier;
- the Operation description or attributes, if the engine supports substitution there.

An active iterator causes the Operation to expand into concrete Operation instances.

Example:

```xml
<Operation id="DraftChapter" type="TextCompletion" desc="Draft chapter {{ChapterIterator}}.">
  <FileRef id="plot_outline"/>
  <Output>
    <File id="draft_chapter_{{ChapterIterator}}" extension="txt"/>
  </Output>
</Operation>
```

may expand to:

```text
DraftChapter[ChapterIterator=1]
DraftChapter[ChapterIterator=2]
...
DraftChapter[ChapterIterator=12]
```

### 11.2 Multiple active iterators

If an Operation references multiple active iterators, the default behaviour is Cartesian expansion.

Example:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="beta_reader_persona_{{PersonaIterator}}"/>
<Output>
  <File id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}" extension="txt"/>
</Output>
```

If there are 12 chapters and 3 personas, the engine may create 36 Operation instances.

A Cognitive Engine should make iterator expansion inspectable before execution, especially when execution may be costly.

---

## 12. Collections: `<Collections>`, `<Collection>` and `<CollectionRef>`

A Collection is a named group of Data Units.

Collections allow multiple related Data Units to be treated as one logical input or output.

Example:

```xml
<Collections>
  <Collection id="all_revised_chapters" orderBy="ChapterIterator">
    <FileRef id="revised_chapter_{{ChapterIterator}}"/>
  </Collection>
</Collections>
```

If `ChapterIterator` ranges from 1 to 12, this Collection represents:

```text
revised_chapter_1
revised_chapter_2
...
revised_chapter_12
```

An Operation may consume a Collection using `<CollectionRef>`:

```xml
<CollectionRef id="all_revised_chapters"/>
```

A `CollectionRef` should not automatically cause the consuming Operation to run once per Collection member.

Instead, the Cognitive Engine resolves the Collection into an ordered group of Data Units and supplies that group to the Operation as grouped context.

Collections are useful for:

- aggregating iterator outputs;
- passing many files into one Operation;
- compiling document sections;
- grouping feedback;
- avoiding accidental output collisions;
- making grouped dependencies explicit.

---

## 13. Operations: `<Operations>` and `<Operation>`

The `<Operations>` section defines the executable transformations in the Module.

Example:

```xml
<Operations>
  <Operation
    id="DraftOutline"
    type="TextCompletion"
    desc="Draft a plot outline from the story requirements.">
    <FileRef id="story_requirements"/>
    <Output>
      <File id="plot_outline" extension="txt"/>
    </Output>
  </Operation>
</Operations>
```

An Operation has:

| Attribute / child | Description |
|---|---|
| `id` | Operation identifier |
| `type` | Operation type |
| `desc` | Description of intended transformation |
| `<FileRef>` | Input Data Unit reference |
| `<CollectionRef>` | Input Collection reference |
| `<Output>` | Declared outputs |
| `function` | Function name for `ExecuteFunction` |
| `target` | Target Module for `PreExisting` |

The Operation description should be specific enough to guide execution.

Poor:

```xml
desc="Make it better."
```

Better:

```xml
desc="Review the draft document for completeness, consistency, unsupported claims, missing requirements and unclear structure. Produce a correction plan with concrete edits."
```

---

## 14. Operation Types

The current TTML draft recognises the following Operation types.

### 14.1 `TextCompletion`

A `TextCompletion` Operation invokes an LLM to perform semantic work.

Suitable for:

- summarisation;
- analysis;
- drafting;
- rewriting;
- review;
- critique;
- classification;
- extraction;
- synthesis;
- planning.

Example:

```xml
<Operation
  id="SummariseArticle"
  type="TextCompletion"
  desc="Create a concise summary of the source article, preserving the main argument, key evidence and important caveats.">
  <FileRef id="source_article"/>
  <Output>
    <File id="article_summary" extension="txt"/>
  </Output>
</Operation>
```

A Cognitive Engine may construct the prompt from:

- Operation description;
- input Data Units;
- Collection contents;
- Module metadata;
- variables;
- active iterator values;
- output declarations;
- semantic contracts, where supported;
- engine-level prompt policy.

### 14.2 `ExecuteFunction`

An `ExecuteFunction` Operation invokes deterministic code.

Suitable for:

- copying files;
- concatenating files;
- validating XML;
- converting formats;
- querying databases;
- resizing images;
- calculating checksums;
- creating archives.

Example:

```xml
<Operation
  id="CompileDocument"
  type="ExecuteFunction"
  desc="Concatenate all drafted sections into a single document."
  function="ConcatenateFiles">
  <CollectionRef id="all_document_sections"/>
  <Output>
    <File id="compiled_document" extension="md"/>
  </Output>
</Operation>
```

The `function` attribute identifies the deterministic function to execute.

A Cognitive Engine should validate that the requested function exists before execution.

### 14.3 `PreExisting`

A `PreExisting` Operation executes another Thought Tree Module.

Example:

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

The `target` attribute identifies the Module to execute.

A Cognitive Engine should:

1. load the target Module;
2. validate it;
3. map parent inputs to submodule inputs;
4. execute the submodule;
5. map submodule outputs back to parent outputs;
6. record the mapping in the execution trace.

Mapping may be explicit, inferred by matching identifiers, or defined by engine-specific rules. Ambiguous mappings should require validation or review.

### 14.4 `ProjectCompletion`

A `ProjectCompletion` Operation allows a Cognitive Engine to generate a new submodule to complete a complex or ambiguous task.

Example:

```xml
<Operation
  id="DevelopConcepts"
  type="ProjectCompletion"
  desc="Develop character, setting and plot concepts from the story requirements.">
  <FileRef id="story_requirements"/>
  <Output>
    <File id="character_profiles" extension="txt"/>
    <File id="setting_overview" extension="txt"/>
    <File id="plot_outline" extension="txt"/>
  </Output>
</Operation>
```

A recommended execution pattern is:

```text
ProjectCompletion Operation
↓
Generate proposed submodule
↓
Validate generated submodule
↓
Optional human approval
↓
Execute submodule
↓
Map outputs back to parent Operation
↓
Record generated submodule in trace
```

Generated submodules should be preserved as execution artefacts.

### 14.5 `DynamicCompletion`

A `DynamicCompletion` Operation allows the Cognitive Engine to choose the execution strategy.

It may resolve into:

- a direct `TextCompletion`;
- an `ExecuteFunction`;
- a `PreExisting` Module call;
- a generated `ProjectCompletion` submodule;
- a sequence of smaller Operations;
- a combination of these.

Because this introduces ambiguity, the engine should record:

- the chosen strategy;
- any generated plan;
- any generated submodule;
- validation results;
- human approval, if required.

DynamicCompletion should not bypass validation, traceability or security controls.

---

## 15. Operation Outputs

Each Operation declares the Data Units or Collections it is expected to produce.

Example:

```xml
<Output>
  <File id="draft_chapter_{{ChapterIterator}}" extension="txt"/>
</Output>
```

An Operation should be considered complete only when:

1. execution has finished;
2. all declared outputs exist;
3. outputs are stored or referenced in the workspace;
4. required validation has passed, where supported;
5. no unrecovered errors remain.

A Cognitive Engine should verify that declared outputs are produced.

---

## 16. Final Outputs

The root-level `<Output>` element declares the final outputs of the Module.

Example:

```xml
<Output>
  <FileRef id="final_summary"/>
</Output>
```

A Module is complete when:

- all required Operations have completed;
- all final output references can be resolved;
- final outputs are available in the workspace;
- required validation has passed;
- no unrecovered execution errors remain;
- the execution trace has been recorded.

The final output section should only reference Data Units or Collections that the Module can actually produce or resolve.

---

## 17. Output Collisions

An output collision occurs when two or more Operation instances attempt to write to the same logical output identifier.

Example problem:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}"/>
<Output>
  <File id="revised_chapter_{{ChapterIterator}}" extension="txt"/>
</Output>
```

If `PersonaIterator` has multiple values, several Operation instances may attempt to produce the same output:

```text
revised_chapter_1
```

By default, silent overwriting should be invalid.

Valid resolutions include:

1. include all active iterators in the output identifier;
2. aggregate the extra iterator dimension into a Collection;
3. insert a separate aggregation Operation;
4. create versioned outputs;
5. explicitly declare overwrite behaviour;
6. require human or automated review.

Preferred aggregation pattern:

```xml
<Collection id="feedback_for_chapter_{{ChapterIterator}}" orderBy="PersonaIterator">
  <FileRef id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}"/>
</Collection>
```

Then:

```xml
<Operation
  id="ReviseChapter"
  type="TextCompletion"
  desc="Revise each chapter using all beta reader feedback for that chapter.">
  <FileRef id="draft_chapter_{{ChapterIterator}}"/>
  <CollectionRef id="feedback_for_chapter_{{ChapterIterator}}"/>
  <Output>
    <File id="revised_chapter_{{ChapterIterator}}" extension="txt"/>
  </Output>
</Operation>
```

---

## 18. Execution Semantics

A Cognitive Engine should not treat TTML as a simple prompt list.

It should compile TTML into an executable cognitive transformation graph.

A typical execution lifecycle is:

```text
Load TTML
↓
Parse XML
↓
Validate schema
↓
Initialise workspace
↓
Resolve inputs and variables
↓
Resolve FileRefs and CollectionRefs
↓
Expand iterators
↓
Detect missing dependencies
↓
Detect output collisions
↓
Build dependency graph
↓
Create execution plan
↓
Execute Operations
↓
Validate outputs
↓
Resolve final outputs
↓
Record execution trace
```

By default, Operations may execute in document order. However, a more advanced engine may execute independent Operations in parallel if doing so does not change the logical result.

---

## 19. Validation Levels

TTML workflows should distinguish between three levels of correctness.

### 19.1 Schema correctness

The TTML document is structurally valid XML and conforms to the TTML schema.

This answers:

```text
Is this a valid TTML document?
```

### 19.2 Execution correctness

The Cognitive Engine can resolve dependencies, expand iterators, call required functions, execute submodules and produce declared outputs.

This answers:

```text
Can this Module run?
```

### 19.3 Semantic correctness

The produced Data Units satisfy their intended meaning, structure, quality and downstream purpose.

This answers:

```text
Did this Module produce the right kind of artefact?
```

Schema validation alone is not sufficient.

A TTML document may be schema-valid but still contain:

- unresolved `FileRef`s;
- missing functions;
- unavailable submodules;
- output collisions;
- weak Operation descriptions;
- missing validation;
- unusable outputs.

---

## 20. Semantic Types and Contracts

The current TTML draft primarily defines structural workflow execution.

Future TTML versions should support explicit semantic types and contracts.

Possible future syntax:

```xml
<File
  id="plot_outline"
  extension="md"
  semanticType="PlotOutline"
  contract="ThreeActPlotOutlineContract"/>
```

A semantic type identifies what kind of artefact a Data Unit represents.

Examples:

- `SourceDigest`;
- `Summary`;
- `PlotOutline`;
- `CharacterProfile`;
- `RequirementsRegister`;
- `RiskRegister`;
- `ReviewReport`;
- `CorrectionPlan`;
- `TechnicalDesignDocument`;
- `TTMLModule`.

A contract defines what must be true for the artefact to be considered valid.

A `PlotOutline` contract might require:

- premise;
- major characters;
- act structure;
- central conflict;
- climax;
- resolution;
- continuity notes.

A `TechnicalRequirementsRegister` contract might require:

- functional requirements;
- non-functional requirements;
- assumptions;
- dependencies;
- risks;
- open questions;
- source traceability.

Contracts may be validated using:

- deterministic functions;
- schema checks;
- LLM review rubrics;
- human approval;
- hybrid review workflows.

This feature is not fully stabilised in the current draft, but it is central to the long-term Thought Tree model.

---

## 21. Workspace and Artefact Resolution

A TTML execution occurs inside a workspace.

A workspace stores or references:

- root inputs;
- intermediate Data Units;
- Collections;
- generated prompts;
- generated submodules;
- function outputs;
- validation reports;
- human review decisions;
- final outputs;
- execution traces.

The workspace maps logical identifiers to concrete assets.

Example:

```xml
<File id="plot_outline" extension="txt"/>
```

may map to:

```text
/workspaces/run_001/outputs/plot_outline.txt
```

or:

```text
asset_id: plot_outline
run_id: run_001
version: 1
mime_type: text/plain
semantic_type: PlotOutline
```

The storage mechanism is implementation-specific, but logical identifiers must remain traceable.

---

## 22. Execution Trace

Every TTML execution should produce an execution trace.

A trace may include:

- source TTML file;
- TTML version;
- Module ID;
- engine version;
- input identifiers;
- input hashes;
- resolved variables;
- iterator expansions;
- execution plan;
- Operation start and end times;
- Operation status;
- active iterator values;
- generated prompts;
- LLM provider and model;
- model settings;
- function calls;
- tool calls;
- submodule paths;
- generated submodules;
- output identifiers;
- output hashes;
- validation results;
- errors;
- retries;
- human review decisions;
- final outputs.

The goal of the trace is not always perfect deterministic reproduction, because LLM outputs may vary.

The goal is to make the process explicit, inspectable, repeatable and improvable.

---

## 23. Minimal TTML Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<TTML version="0.12.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:noNamespaceSchemaLocation="TTMLSchema.xsd">

  <Project
    id="ArticleSummaryModule"
    genre="Analysis"
    author="Example"
    date="2026-06-23"
    desc="Summarise an article through draft, review and revision." />

  <Inputs>
    <File id="source_article" folder="/inputs" extension="txt"/>
  </Inputs>

  <Vars/>

  <Iterators/>

  <Collections/>

  <Operations>
    <Operation
      id="DraftSummary"
      type="TextCompletion"
      desc="Create a concise structured summary of the source article. Include the main argument, key evidence, important caveats and unresolved questions. Do not introduce unsupported claims.">
      <FileRef id="source_article"/>
      <Output>
        <File id="draft_summary" extension="txt"/>
      </Output>
    </Operation>

    <Operation
      id="ReviewSummary"
      type="TextCompletion"
      desc="Review the draft summary against the source article. Identify omissions, inaccuracies, unsupported claims, unclear phrasing and opportunities to improve structure. Produce a correction plan.">
      <FileRef id="source_article"/>
      <FileRef id="draft_summary"/>
      <Output>
        <File id="summary_review_correction_plan" extension="txt"/>
      </Output>
    </Operation>

    <Operation
      id="ReviseSummary"
      type="TextCompletion"
      desc="Revise the draft summary using the correction plan. Produce the final summary only. Ensure it is accurate, concise, well-structured and faithful to the source article.">
      <FileRef id="source_article"/>
      <FileRef id="draft_summary"/>
      <FileRef id="summary_review_correction_plan"/>
      <Output>
        <File id="final_summary" extension="txt"/>
      </Output>
    </Operation>
  </Operations>

  <Output>
    <FileRef id="final_summary"/>
  </Output>
</TTML>
```

This expresses:

```text
source_article
↓
DraftSummary
↓
draft_summary
↓
ReviewSummary
↓
summary_review_correction_plan
↓
ReviseSummary
↓
final_summary
```

Even this small example preserves intermediate artefacts and separates drafting from review and revision.

---

## 24. Conformance

Conformance levels are not final, but the following draft levels are suggested.

### 24.1 Basic TTML Engine

A basic TTML-conformant Cognitive Engine should support:

- loading TTML documents;
- validating XML structure;
- resolving root-level inputs;
- resolving `FileRef`s;
- executing Operations in document order;
- supporting `TextCompletion`;
- supporting `ExecuteFunction` where functions are registered;
- producing declared Operation outputs;
- resolving final outputs;
- detecting unresolved dependencies;
- detecting output collisions;
- recording an execution trace.

### 24.2 Intermediate TTML Engine

An intermediate engine should additionally support:

- variables;
- iterators;
- Collections;
- `CollectionRef`;
- iterator expansion;
- Collection ordering;
- submodule execution through `PreExisting`;
- workspace versioning;
- basic validation reports;
- dry-run execution planning.

### 24.3 Advanced Cognitive Engine

An advanced engine may support:

- semantic types;
- contracts;
- validation gates;
- human review;
- `ProjectCompletion`;
- `DynamicCompletion`;
- generated submodules;
- tool calls;
- model selection;
- caching;
- parallel execution;
- automated repair;
- module testing;
- module improvement workflows;
- module registries;
- function registries;
- security and governance policies.

---

## 25. Authoring Guidance

A good TTML Module should:

- start from a clear intended transformation;
- declare inputs explicitly;
- declare final outputs explicitly;
- preserve useful intermediate artefacts;
- break large tasks into meaningful Operations;
- use LLM Operations for semantic work;
- use deterministic functions for mechanical work;
- use Collections for grouped inputs;
- use iterators carefully;
- avoid silent output collisions;
- include review and revision steps where quality matters;
- design for validation;
- design for traceability;
- remain reusable where possible.

Poor pattern:

```text
source_notes → MakeFinalDocument → final_document
```

Better pattern:

```text
source_notes
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
review_and_correction_plan
↓
ReviseFinalDocument
↓
final_document
```

---

## 26. Known Limitations of the Current Draft

The current TTML draft is intentionally simple, but this creates limitations.

Known gaps include:

- no stable final XML schema;
- limited semantic type support;
- limited contract support;
- limited control flow;
- no standard human review element;
- no standard tool-call syntax;
- no standard model capability syntax;
- no standard security policy syntax;
- no standard module package format;
- no standard execution trace schema;
- file-centric Data Unit representation;
- incomplete conformance test suite.

These limitations should be addressed through future specification work.

---

## 27. Open Questions

Important open design questions include:

1. Should TTML remain XML-only, or should YAML/JSON become first-class source formats?
2. How should semantic contracts be represented?
3. Should `HumanReview` become a formal Operation type?
4. Should tool calls be represented as `ExecuteFunction`, a new `ToolCall` type, or both?
5. How should submodule input/output mapping be declared?
6. How should conditional execution be represented?
7. How should loops and scheduled execution be represented?
8. How should model capability requirements be declared without binding to providers?
9. How should generated submodules be validated and approved?
10. What conformance levels should be formalised?
11. What should the execution trace schema require?
12. How should security policies be attached to Modules?
13. Should Data Units remain file-based in TTML 1.0, or should richer artefact types be introduced?

---

## 28. Summary

TTML is the draft XML-based source format for Thought Tree Modules.

A TTML document defines a cognitive workflow made of:

- inputs;
- variables;
- iterators;
- Collections;
- Operations;
- intermediate artefacts;
- final outputs.

A Cognitive Engine compiles TTML into an executable transformation graph, executes the graph using LLMs, deterministic functions, tools, submodules and human review, then records the resulting artefacts and trace.

TTML’s purpose is not merely to organise prompts.

Its purpose is to make LLM-assisted work:

- programmable;
- inspectable;
- reusable;
- auditable;
- model-independent;
- validatable;
- improvable.

This document is a starting point for stabilising TTML and building a reference implementation.