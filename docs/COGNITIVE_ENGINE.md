# Cognitive Engine

> Status: Draft handoff documentation  
> Intended repository path: `docs/COGNITIVE_ENGINE.md`  
> Documentation license: CC0, if used within the proposed Thought Tree Framework handoff repository  
> Related code license: MIT, where applicable to implementations

---

## 1. Overview

A **Cognitive Engine** is the compiler, runtime, workspace manager and trace system for Thought Tree programs.

It takes a human-readable cognitive workflow definition, such as a TTML Module, and turns it into executable work. It parses the source definition, validates its structure, resolves references, expands iterators, builds an executable transformation graph, invokes LLMs and deterministic functions, manages intermediate artefacts, validates outputs and records an execution trace.

A Thought Tree program describes:

```text
what cognitive process should happen
```

The Cognitive Engine determines:

```text
how that process is compiled, executed, monitored, validated and recorded
```

The Cognitive Engine is therefore not merely a prompt runner. It is the operational centre of the Thought Tree Framework.

At a high level:

```text
Thought Tree Source Definition
TTML / future YAML / visual editor / generated module
        ↓
Cognitive Engine
parse → validate → compile → plan → execute → verify → trace
        ↓
Execution Backends
LLMs / deterministic functions / tools / submodules / human review
        ↓
Workspace
inputs / intermediate artefacts / outputs / logs / traces
        ↓
Final Outputs
validated Data Units, reports, generated modules, execution records
```

---

## 2. Why the Cognitive Engine Exists

Most LLM workflows are currently implemented as:

```text
User → Prompt → LLM → Output
```

or as informal prompt chains:

```text
Prompt 1 → Output 1 → Prompt 2 → Output 2 → Prompt 3 → Final Output
```

or as autonomous agent loops:

```text
Goal → Agent plans → Agent acts → Agent observes → Agent continues
```

These approaches can be useful, but they often lack:

- explicit structure;
- inspectable intermediate artefacts;
- repeatable execution;
- modular reuse;
- dependency management;
- output validation;
- provenance;
- error handling;
- audit trails;
- separation between process definition and execution backend.

The Cognitive Engine addresses this by treating LLM-assisted work as an executable cognitive program.

A Thought Tree program is not just a set of prompts. It is a structured definition of transformations over Data Units:

```text
Data Units → Operations → Data Units
```

The Cognitive Engine turns that definition into a controlled execution process.

---

## 3. Core Responsibilities

A Cognitive Engine is responsible for:

- loading a Thought Tree source definition;
- parsing the source representation;
- validating the source structure;
- converting the source into the Thought Tree Program Model;
- resolving variables, inputs, Data Units, Collections and module references;
- expanding iterators into concrete operation instances;
- detecting unresolved dependencies;
- detecting output collisions;
- compiling the workflow into a Cognitive Transformation Graph;
- creating an execution plan;
- executing operations in the correct order;
- invoking LLMs for semantic transformations;
- invoking deterministic functions for mechanical transformations;
- invoking tools or external systems where supported;
- executing submodules;
- supporting generated submodules where supported;
- pausing for human review where required;
- validating outputs against declared contracts where supported;
- preserving intermediate artefacts;
- handling errors and retries;
- resolving final outputs;
- recording an execution trace;
- returning or storing the completed outputs.

The exact implementation may vary between engines, but the intended meaning of a Thought Tree program should remain stable across conformant engines.

---

## 4. The Cognitive Engine Is Not the LLM

A key distinction in the Thought Tree Framework is:

```text
The LLM is not the Cognitive Engine.
```

The LLM is one execution backend used by the Cognitive Engine.

The Cognitive Engine provides:

- structure;
- state management;
- dependency resolution;
- operation planning;
- workspace management;
- validation;
- error handling;
- traceability;
- model selection;
- function and tool orchestration.

The LLM provides semantic transformation capability, such as:

- summarisation;
- analysis;
- drafting;
- review;
- revision;
- synthesis;
- classification;
- planning;
- extraction;
- module generation.

This gives the framework a hybrid execution model:

```text
LLMs handle ambiguity.
Code handles structure.
The Cognitive Engine coordinates both.
```

---

## 5. Major Engine Roles

The Cognitive Engine performs four broad roles.

### 5.1 Compiler

The engine compiles a source definition into an executable internal representation.

Compilation may include:

- parsing;
- schema validation;
- normalisation;
- symbol resolution;
- variable substitution;
- iterator expansion;
- Collection resolution;
- dependency analysis;
- output collision detection;
- contract attachment;
- execution planning.

### 5.2 Runtime

The engine executes the planned operations.

Runtime execution may involve:

- LLM calls;
- deterministic function calls;
- tool calls;
- submodule execution;
- generated module execution;
- human review;
- retry and repair logic;
- validation gates.

### 5.3 Workspace Manager

The engine manages the execution workspace.

The workspace stores or references:

- supplied inputs;
- intermediate Data Units;
- Collections;
- generated prompts;
- generated submodules;
- deterministic function outputs;
- tool results;
- validation reports;
- error reports;
- human review decisions;
- final outputs;
- execution traces.

### 5.4 Trace and Validation System

The engine records what happened and validates whether outputs are acceptable.

This includes:

- schema validation;
- execution validation;
- semantic validation where supported;
- provenance tracking;
- output hashes;
- model and function records;
- validation results;
- error and retry records;
- final output resolution.

---

## 6. Execution Lifecycle

A typical Thought Tree execution follows this lifecycle:

```text
1. Load source definition
2. Parse source
3. Validate source structure
4. Convert source into the internal program model
5. Initialise workspace
6. Resolve inputs, variables, Data Units and Collections
7. Expand iterators
8. Compile into a Cognitive Transformation Graph
9. Validate dependencies and output behaviour
10. Create execution plan
11. Execute operations
12. Validate produced outputs
13. Handle errors, retries and review gates
14. Resolve final outputs
15. Record execution trace
16. Return or store final artefacts
```

Different engines may implement these stages differently, but a conformant engine should preserve the intended semantics of the Thought Tree program.

---

## 7. Compilation Process

Before a Thought Tree program can be executed, it must be compiled into an executable form.

A recommended compilation pipeline is:

```text
Source Module
        ↓
Parse
        ↓
Normalise
        ↓
Resolve Symbols
        ↓
Expand Iterators
        ↓
Build Dependency Graph
        ↓
Validate Execution Requirements
        ↓
Create Execution Plan
```

---

## 8. Parsing

The engine loads the source document and converts it into structured objects.

For TTML, this means reading elements such as:

- `<TTML>`;
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

The result is an initial in-memory representation of the Module.

At this stage, the engine should preserve enough source information to produce useful error messages and execution traces.

---

## 9. Normalisation

Normalisation converts a source-specific representation into the engine’s internal Thought Tree Program Model.

This matters because TTML is only one possible source format.

Future source formats may include:

- YAML;
- JSON;
- a visual graph editor;
- a database-backed module definition;
- an LLM-generated module;
- a higher-level domain-specific language.

Normalisation allows different source syntaxes to compile into the same internal representation.

For example:

```text
TTML source
YAML source
Visual graph source
Generated module source
        ↓
Common Thought Tree Program Model
```

This separates the abstract meaning of the program from the syntax used to write it.

---

## 10. Symbol Resolution

The engine resolves all named references before execution.

This includes:

- `FileRef` references;
- `CollectionRef` references;
- variable references such as `{{ChapterCount}}`;
- iterator references such as `{{ChapterIterator}}`;
- deterministic function names;
- target module paths;
- contract names, where supported;
- semantic type references, where supported;
- tool references, where supported.

If a required reference cannot be resolved, the engine should report a validation error before execution begins unless the program explicitly allows dynamic resolution.

Silent creation of missing dependencies should be avoided.

If the engine generates, requests or substitutes a missing dependency, that event must be recorded in the execution trace.

---

## 11. Variable Resolution

Variables are named values available during execution.

Example TTML:

```xml
<Vars>
  <Var name="ChapterCount" type="integer" value="12"/>
</Vars>
```

A variable may be referenced using double curly braces:

```text
{{ChapterCount}}
```

Example:

```xml
<Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
```

The Cognitive Engine should resolve variables before iterator expansion and operation execution.

If a required variable cannot be resolved, validation should fail.

Unless otherwise specified, variables are local to the current Module.

---

## 12. Iterator Expansion

Iterators define repeated execution dimensions.

Example:

```xml
<Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
```

If `ChapterCount` resolves to `12`, then `ChapterIterator` expands to:

```text
1, 2, 3, ..., 12
```

Iterator values may be inserted into Data Unit identifiers:

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

The engine expands iterator-dependent operations into concrete operation instances.

For example:

```text
DraftChapter[ChapterIterator=1]
DraftChapter[ChapterIterator=2]
DraftChapter[ChapterIterator=3]
...
DraftChapter[ChapterIterator=12]
```

Iterator expansion should happen before execution so that the engine can detect:

- unresolved references;
- excessive execution cost;
- invalid iterator ranges;
- unintended Cartesian products;
- output collisions.

---

## 13. Active Iterators

An iterator becomes active for an operation when it appears directly in an executable field such as:

- a direct `FileRef` used by the operation;
- a direct output `File` produced by the operation;
- a `CollectionRef` identifier used by the operation;
- the operation description or attributes, if substitution is supported there.

An iterator used only inside the internal member definition of a Collection does not necessarily cause the consuming operation to expand.

This distinction is important.

A Collection may aggregate repeated Data Units without causing the consuming operation to run once per Collection member.

---

## 14. Multiple Iterators

If an operation references multiple active iterators, the default behaviour is usually Cartesian expansion.

Example:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="beta_reader_persona_{{PersonaIterator}}"/>

<Output>
  <File id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}" extension="txt"/>
</Output>
```

If there are 12 chapters and 3 personas, the engine expands this into 36 operation instances:

```text
Chapter 1, Persona 1
Chapter 1, Persona 2
Chapter 1, Persona 3
Chapter 2, Persona 1
...
Chapter 12, Persona 3
```

This expansion should be inspectable before execution, especially where execution may be expensive.

---

## 15. Collections

A Collection is a named group of Data Units.

Example:

```xml
<Collection id="all_revised_chapters" orderBy="ChapterIterator">
  <FileRef id="revised_chapter_{{ChapterIterator}}"/>
</Collection>
```

If `ChapterIterator` ranges from 1 to 12, the Collection represents:

```text
revised_chapter_1
revised_chapter_2
...
revised_chapter_12
```

An operation may consume the Collection as a grouped input:

```xml
<CollectionRef id="all_revised_chapters"/>
```

A `CollectionRef` should not automatically cause an Operation to execute once per Collection member.

Instead, the engine resolves the Collection into an ordered set of Data Units and supplies that set to the operation as grouped context.

Collections are useful for:

- aggregating iterator outputs;
- passing many files into one LLM operation;
- compiling generated sections;
- supplying all feedback for one artefact;
- preventing unintended output collisions;
- making grouped dependencies explicit.

If an order is declared, the engine should preserve that order when supplying the Collection to an operation or deterministic function.

---

## 16. Cognitive Transformation Graph

Internally, the engine should represent the compiled program as a **Cognitive Transformation Graph**.

In this graph:

- Data Units are artefact nodes;
- Operations are transformation nodes;
- Collections are grouped artefact nodes or references;
- Modules are reusable subgraphs;
- Contracts are validation rules attached to nodes or edges;
- dependency relationships are edges;
- provenance relationships are recorded through trace metadata.

A simplified graph looks like:

```text
[Input Data Unit] ──used by──> [Operation] ──produces──> [Output Data Unit]
```

A larger graph may branch and merge:

```text
                   Character Profiles
                  ↗
Story Brief → Concept Development → Plot Outline → Chapter Drafts → Reviews → Revisions
                  ↘
                   Setting Overview
```

Although the framework is called “Thought Tree,” execution is often better understood as a directed graph.

This is especially true when workflows use:

- shared dependencies;
- Collections;
- iterators;
- review cycles;
- validation gates;
- submodules;
- generated modules;
- conditional execution;
- loops.

The graph allows the engine to reason about:

- execution order;
- dependency satisfaction;
- missing inputs;
- output collisions;
- safe parallelisation;
- caching;
- provenance;
- final output resolution.

---

## 17. Execution Planning

Once the graph is built and validated, the engine creates an execution plan.

The execution plan should identify:

- concrete operation instances;
- active iterator values for each instance;
- required input Data Units;
- required input Collections;
- expected outputs;
- operation types;
- target functions or modules, if applicable;
- required contracts;
- review gates;
- whether an operation can be parallelised;
- expected cost or resource usage, where available;
- failure and retry behaviour.

For small workflows, this plan may be implicit.

For large, expensive, high-impact or dynamically generated workflows, the execution plan should be inspectable before execution.

A useful implementation feature is a **dry run** mode:

```text
Validate and display execution plan without invoking LLMs or producing outputs.
```

---

## 18. Operation Execution

Each operation is executed according to its operation type.

Core operation types include:

- `TextCompletion`;
- `ExecuteFunction`;
- `PreExisting`;
- `ProjectCompletion`;
- `DynamicCompletion`.

Future operation types may include:

- `HumanReview`;
- `ToolCall`;
- `ValidationGate`;
- `Condition`;
- `Loop`;
- `Schedule`;
- `WaitForInput`.

---

## 19. TextCompletion Operations

A `TextCompletion` operation invokes an LLM to produce one or more output Data Units.

Example:

```xml
<Operation id="DraftOutline" type="TextCompletion" desc="Draft a plot outline.">
  <FileRef id="story_requirements"/>
  <Output>
    <File id="plot_outline" extension="txt"/>
  </Output>
</Operation>
```

The Cognitive Engine constructs an LLM request using:

- the operation description;
- resolved input Data Units;
- resolved Collections;
- relevant variables;
- active iterator values;
- Module metadata;
- output declarations;
- semantic contracts, where supported;
- engine-level system instructions;
- model selection rules;
- prompt construction policy.

The declared output tells the engine what artefact the LLM is expected to produce.

The engine may use internal LLM calls to improve reliability, such as:

- prompt construction;
- model selection;
- output format repair;
- validation;
- review;
- correction planning.

These internal calls are implementation details, but where practical they should be recorded in the execution trace.

---

## 20. ExecuteFunction Operations

An `ExecuteFunction` operation invokes deterministic code rather than an LLM.

Example:

```xml
<Operation
  id="CompileNovel"
  type="ExecuteFunction"
  desc="Concatenate the revised chapter files into one manuscript."
  function="ConcatenateFiles">

  <CollectionRef id="all_revised_chapters"/>

  <Output>
    <File id="compiled_novel_manuscript" extension="txt"/>
  </Output>
</Operation>
```

The `function` attribute identifies the deterministic function to execute.

Functions are appropriate for:

- copying files;
- concatenating files;
- validating XML;
- converting formats;
- querying databases;
- resizing images;
- generating checksums;
- creating archives;
- parsing structured data;
- applying templates;
- calling controlled APIs.

The engine should validate that the requested function exists before execution.

If a function receives a CollectionRef, the Collection should be expanded into the function’s input list according to its declared order.

Function calls should be recorded in the execution trace, including:

- function name;
- function version, where available;
- input identifiers;
- output identifiers;
- errors;
- status;
- execution time.

Deterministic functions are important because they prevent LLMs from being used for work ordinary software can perform more reliably.

---

## 21. PreExisting Operations

A `PreExisting` operation executes another Thought Tree Module.

Example:

```xml
<Operation
  id="ConceptDevelopment"
  type="PreExisting"
  desc="Run the concept development subproject."
  target="/subprojects/ConceptDevelopment.ttml">

  <FileRef id="story_requirements"/>

  <Output>
    <File id="plot_outline" extension="txt"/>
  </Output>
</Operation>
```

When executing a pre-existing submodule, the Cognitive Engine should:

1. load the target Module;
2. validate the target Module;
3. map parent inputs to submodule inputs;
4. execute the submodule;
5. map submodule outputs back to parent outputs;
6. record the mapping in the execution trace;
7. continue executing the parent Module.

Input and output mapping may be:

- explicit;
- inferred by matching identifiers;
- determined by engine-specific rules.

Ambiguous mappings should require validation or review.

Submodule execution is central to the recursive nature of the Thought Tree Framework.

---

## 22. ProjectCompletion Operations

A `ProjectCompletion` operation expands a complex or ambiguous task into a generated submodule.

This is useful when a task is too large or ambiguous to execute as a single completion.

Example:

```xml
<Operation
  id="DevelopConcept"
  type="ProjectCompletion"
  desc="Develop the character, setting and plot concepts.">

  <FileRef id="story_requirements"/>

  <Output>
    <File id="character_profiles" extension="txt"/>
    <File id="setting_overview" extension="txt"/>
    <File id="plot_outline" extension="txt"/>
  </Output>
</Operation>
```

A Cognitive Engine may use an LLM to generate a new Thought Tree Module that decomposes the operation into smaller steps.

The generated submodule should:

- accept mapped inputs from the parent operation;
- produce outputs satisfying the parent operation’s declared outputs;
- be validated before execution;
- be stored in the workspace;
- be included in the execution trace.

A recommended execution pattern is:

```text
ProjectCompletion Operation
        ↓
Generate proposed submodule
        ↓
Validate generated submodule
        ↓
Review or approve submodule, if required
        ↓
Execute submodule
        ↓
Map outputs back to parent operation
```

Generated Modules should be preserved as execution artefacts.

They form part of the audit trail and may later be reviewed, reused or improved.

---

## 23. DynamicCompletion Operations

A `DynamicCompletion` operation allows the Cognitive Engine to choose the execution strategy.

A DynamicCompletion may resolve into:

- a direct `TextCompletion`;
- an `ExecuteFunction` call;
- a `PreExisting` Module call;
- a generated `ProjectCompletion` submodule;
- a sequence of smaller operations;
- a tool call;
- a human review workflow;
- a combination of the above.

DynamicCompletion is useful when the author wants to describe the intended cognitive transformation without specifying exactly how it should be executed.

Because DynamicCompletion introduces ambiguity, the engine should record:

- the chosen execution strategy;
- why that strategy was selected, where possible;
- any generated operations or submodules;
- validation results;
- human approval, if required.

DynamicCompletion should not bypass validation, traceability, cost control or security controls.

---

## 24. Human Review

A Cognitive Engine may support human review as part of execution.

Human review may be used when:

- a generated plan requires approval;
- an output has high impact;
- semantic contract validation fails;
- output collisions require resolution;
- dynamic submodules need inspection;
- quality is uncertain;
- policy requires human authorisation;
- an unrecoverable error occurs.

Human review can be represented as:

- a dedicated operation type;
- a pause in the runtime;
- an external approval workflow;
- a deterministic function;
- a review Module;
- an engine-level validation gate.

Human review decisions should be recorded in the execution trace.

If a human modifies an input, output, operation, Module, contract or final output, that change should be treated as part of the execution process, not as an invisible external correction.

---

## 25. Workspace Management

Every Thought Tree execution takes place inside a workspace.

The workspace maps logical identifiers from the Thought Tree program to concrete assets.

For example:

```xml
<File id="plot_outline" extension="txt"/>
```

may correspond to:

```text
/workspaces/run_001/outputs/plot_outline.txt
```

or to a database record:

```text
asset_id: plot_outline
run_id: run_001
version: 1
mime_type: text/plain
semantic_type: PlotOutline
```

A workspace may be implemented as:

- a local directory;
- cloud object storage;
- a database;
- an in-memory context;
- a version-controlled repository;
- a project asset store;
- a hybrid storage system.

The workspace should preserve:

- root inputs;
- resolved variables;
- expanded execution plans;
- intermediate outputs;
- final outputs;
- generated prompts;
- generated submodules;
- function outputs;
- tool outputs;
- validation reports;
- error reports;
- human review decisions;
- execution traces.

Preserving intermediate artefacts is one of the main advantages of the framework.

It allows users to inspect, revise, replace and rerun parts of a cognitive process without treating the entire workflow as a single opaque prompt.

---

## 26. Data Unit Resolution

Before executing an operation, the engine must resolve every required Data Unit reference.

A Data Unit may originate from:

- root-level inputs;
- outputs of previous operations;
- outputs of submodules;
- workspace assets;
- external references;
- tool calls;
- human uploads;
- dynamically generated content;
- recovered artefacts produced by an explicit recovery strategy.

When an operation references a Data Unit, the engine should determine:

- whether it exists;
- where it is stored;
- what version should be used;
- what its content type is;
- what semantic type it represents, where supported;
- whether it satisfies its contract, where supported;
- whether the operation has permission to use it.

A `FileRef` should not be treated as merely a filename.

It is a logical reference to an artefact in the execution workspace.

---

## 27. Output Resolution

Each operation declares the outputs it is expected to produce.

An operation is complete only when:

1. its execution has finished;
2. all declared outputs exist;
3. outputs are stored or referenced in the workspace;
4. required validation gates have passed;
5. no unrecovered operation errors remain.

The root-level final output declaration defines the final outputs of the Module.

Example:

```xml
<Output>
  <FileRef id="final_edited_novel"/>
</Output>
```

A Thought Tree execution is complete when:

- all required operations have completed;
- all final outputs can be resolved;
- final outputs are available in the workspace;
- required contracts have passed, where supported;
- no unrecovered execution errors remain;
- the execution trace has been recorded.

If a final output cannot be resolved, execution should be considered incomplete or failed.

---

## 28. Output Collisions

An output collision occurs when two or more operation instances attempt to write to the same logical output identifier.

Example problem:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}"/>

<Output>
  <File id="revised_chapter_{{ChapterIterator}}" extension="txt"/>
</Output>
```

If `PersonaIterator` has multiple values, several operation instances may attempt to produce the same output:

```text
revised_chapter_1
```

By default, this should be invalid.

The engine should detect output collisions during pre-execution validation.

Valid collision resolutions include:

1. include all active iterators in the output identifier;
2. aggregate the extra iterator dimension into a Collection;
3. insert a separate aggregation operation;
4. create versioned outputs;
5. explicitly declare overwrite behaviour;
6. require human or automated review.

Silent overwriting should be invalid by default.

---

## 29. Versioning and Replacement

Outputs should be treated as versioned artefacts unless explicitly overwritten.

If an operation produces an output identifier that already exists, the engine should use one of the following strategies:

1. reject the operation as an output collision;
2. create a new version;
3. overwrite only if explicitly permitted;
4. preserve both outputs and require resolution;
5. invoke an error-handling or review process.

A file-based versioning pattern may look like:

```text
draft_chapter_1.v1.txt
draft_chapter_1.v2.txt
draft_chapter_1.v3.txt
```

An internal metadata pattern may look like:

```text
Data Unit: draft_chapter_1
Version: 1
Version: 2
Version: 3
```

The specific versioning mechanism is implementation-specific, but provenance should be preserved.

---

## 30. Semantic Contracts and Validation

A Cognitive Engine should support validation beyond simple file existence.

A Thought Tree program may declare or imply contracts for outputs.

A contract defines what must be true for an artefact to be considered valid.

Contracts may specify:

- expected semantic type;
- expected physical format;
- required sections;
- required fields;
- completeness criteria;
- quality criteria;
- source traceability requirements;
- validation functions;
- LLM review rubrics;
- human approval requirements.

The engine should distinguish between three levels of correctness.

### 30.1 Schema Correctness

The source program is structurally valid.

For TTML, this means the XML conforms to the TTML schema.

### 30.2 Execution Correctness

The engine can resolve dependencies, execute operations and produce declared outputs.

### 30.3 Semantic Correctness

The produced outputs satisfy their intended meaning, structure, quality and downstream purpose.

Without semantic validation, a workflow may appear successful while passing weak or incomplete outputs into downstream operations.

Where contracts are declared, an operation should not be considered complete until its outputs satisfy the required contracts or an explicit failure/review path is triggered.

---

## 31. Contract Failure Handling

If a semantic contract fails, the engine may:

- retry the operation;
- repair the output;
- invoke a review operation;
- invoke an error-handling Module;
- request human intervention;
- produce a diagnostic report;
- fail the workflow safely.

A contract check should produce a clear outcome, such as:

```text
Pass
Pass with warnings
Fail and retry
Fail and request review
Fail terminally
```

All contract failures and recovery attempts should be recorded in the execution trace.

Failed versions should generally be preserved rather than silently overwritten.

---

## 32. Error Handling

Execution may fail for many reasons.

Common failure types include:

- invalid source syntax;
- schema validation failure;
- missing inputs;
- unresolved FileRefs;
- unresolved CollectionRefs;
- invalid variables;
- invalid iterators;
- output collisions;
- missing deterministic functions;
- unavailable target modules;
- unavailable model provider;
- LLM API failure;
- malformed LLM output;
- failed semantic validation;
- failed tool call;
- permission errors;
- storage errors;
- human rejection;
- final output resolution failure.

The engine should report errors with enough context for diagnosis, including:

- Module ID;
- operation ID;
- operation type;
- active iterator values;
- input references;
- expected outputs;
- error message;
- timestamp;
- retry attempts;
- validation results.

The engine may attempt recovery through:

1. retrying the operation;
2. using a different model;
3. regenerating malformed outputs;
4. invoking a repair operation;
5. invoking an error-handling Module;
6. requesting human review;
7. skipping optional operations, if explicitly supported;
8. rolling back partial outputs;
9. producing a diagnostic report;
10. failing the run safely.

Silent failure should not be allowed.

All errors and recovery attempts should be recorded in the execution trace.

---

## 33. Execution Trace

Every Thought Tree execution should produce an execution trace.

The trace records what happened during execution and allows the process to be inspected, debugged, reviewed, reproduced and improved.

An execution trace may include:

- source program identifier;
- source program version;
- schema version;
- engine version;
- input identifiers;
- input content hashes;
- resolved variables;
- iterator expansions;
- dependency graph;
- execution plan;
- operation start and end times;
- operation statuses;
- active iterator values;
- model provider;
- model name;
- model version, where available;
- model settings;
- generated prompts, where appropriate;
- deterministic function calls;
- tool calls;
- submodule executions;
- generated submodules;
- output identifiers;
- output versions;
- output hashes;
- validation reports;
- contract results;
- retries;
- errors;
- human review decisions;
- final outputs.

The goal of the trace is not always exact deterministic reproduction, because LLM behaviour may vary.

The goal is to make the process explicit, inspectable, repeatable and improvable.

---

## 34. Model Independence

A key purpose of the Cognitive Engine is to separate Thought Tree programs from specific LLM providers.

A Module should describe the intended cognitive process, not hard-code a particular model unless necessary.

Instead of specifying:

```text
Use Model X from Provider Y.
```

a Module may specify capability requirements such as:

```text
Requires long-context reasoning.
Requires high-quality creative synthesis.
Requires structured JSON output.
Requires low-cost summarisation.
Requires local execution due to data sensitivity.
```

The Cognitive Engine can then map those requirements to an available model.

This allows the same Thought Tree program to run on different backends as:

- models improve;
- costs change;
- privacy requirements change;
- organisational policies evolve;
- local models become more capable;
- provider APIs change.

---

## 35. Engine Registries

A Cognitive Engine may maintain registries of available execution resources.

### 35.1 Module Registry

A catalogue of reusable Thought Tree Modules.

May store:

- module identifier;
- version;
- description;
- author;
- required inputs;
- declared outputs;
- supported schema version;
- dependencies;
- validation status;
- evaluation history.

### 35.2 Function Registry

A catalogue of deterministic functions.

May store:

- function name;
- purpose;
- input requirements;
- output behaviour;
- supported file types;
- deterministic status;
- side effects;
- error conditions;
- permissions required;
- version.

### 35.3 Model Registry

A catalogue of available LLMs and model capabilities.

May store:

- provider;
- model name;
- context window;
- supported modalities;
- cost profile;
- quality profile;
- latency profile;
- structured output support;
- privacy constraints;
- tool support;
- version information.

### 35.4 Tool Registry

A catalogue of external tools.

May store:

- tool name;
- purpose;
- permissions;
- input schema;
- output schema;
- side effects;
- authentication requirements;
- rate limits;
- audit requirements.

### 35.5 Contract Registry

A catalogue of reusable semantic contracts.

May store:

- contract identifier;
- semantic type;
- validation criteria;
- required structure;
- machine validators;
- LLM review rubrics;
- human approval requirements;
- compatible validators.

Registries make Thought Tree programs more portable, reusable and robust.

---

## 36. Parallel and Optimised Execution

The default execution model may be sequential document-order execution.

However, once the engine has built a dependency graph, it may safely optimise execution.

Possible optimisations include:

- running independent operations in parallel;
- caching previously generated outputs;
- skipping unchanged deterministic steps;
- reusing validated artefacts;
- selecting cheaper models for low-risk steps;
- selecting stronger models for high-risk steps;
- batching similar LLM calls;
- rerunning only affected downstream operations after an input change.

Optimisation must not change the intended semantics of the program.

Correctness, traceability and safety should take priority over speed.

Parallel execution is safe when:

- operations do not depend on each other’s outputs;
- operations do not write to the same output identifiers;
- operation ordering does not affect later results;
- shared resources are handled safely;
- trace ordering remains clear.

---

## 37. Loops and Conditional Execution

Simple Thought Tree programs execute as finite operation graphs.

More advanced engines may support:

- loops;
- conditions;
- schedules;
- event-driven execution;
- stateful monitoring workflows.

Where supported, loops should define:

- loop condition;
- maximum iteration count;
- stopping criteria;
- state carried between iterations;
- delay or scheduling behaviour, if applicable;
- error handling behaviour;
- review or approval requirements, if applicable.

Indefinite loops should require explicit author intent and engine-level safeguards.

Conditional execution should define:

- the condition being evaluated;
- the operation or function that evaluates it;
- possible branches;
- default behaviour;
- traceable decision output.

Because loops and conditionals can turn a tree into a directed graph, the engine should make the resulting execution structure inspectable.

---

## 38. Security and Governance

Because a Cognitive Engine may execute functions, call tools, read files, write outputs, generate modules and use external models, it must include safeguards.

Important governance concerns include:

- file access permissions;
- tool access permissions;
- sandboxing deterministic functions;
- controlling side effects;
- managing API credentials;
- preventing prompt injection;
- validating untrusted Modules;
- limiting dynamic execution;
- enforcing human approval policies;
- controlling cost;
- protecting confidential data;
- recording audit logs;
- trace retention;
- model provider privacy.

A Cognitive Engine should be able to distinguish between:

- trusted and untrusted Modules;
- safe and unsafe tools;
- local and external model execution;
- read-only and side-effecting operations;
- advisory and required validation;
- low-impact and high-impact outputs.

Dynamic or generated Modules should be validated before execution.

High-impact workflows should support human approval gates.

---

## 39. Basic Conformance

A basic Cognitive Engine should support:

- loading a Thought Tree source document;
- validating its structure;
- resolving root-level inputs;
- resolving variables;
- resolving Data Units;
- executing operations in document order;
- supporting `TextCompletion`;
- supporting `ExecuteFunction` where functions are registered;
- producing declared outputs;
- resolving final outputs;
- detecting unresolved dependencies;
- detecting output collisions;
- recording an execution trace.

---

## 40. Intermediate Conformance

An intermediate Cognitive Engine should additionally support:

- iterator expansion;
- Collections;
- CollectionRefs;
- dependency graph construction;
- execution planning;
- workspace artefact management;
- submodule execution through `PreExisting`;
- function registry;
- model provider abstraction;
- error handling and retry policies;
- dry-run validation;
- basic graph visualisation.

---

## 41. Advanced Conformance

An advanced Cognitive Engine may support:

- semantic types;
- semantic contracts;
- validation gates;
- human review;
- model selection;
- tool use;
- generated submodules;
- `DynamicCompletion`;
- `ProjectCompletion`;
- module search;
- module registries;
- contract registries;
- parallel execution;
- caching;
- automated repair;
- test harnesses;
- module improvement workflows;
- cost tracking;
- governance policies;
- audit export.

Conformance levels may be defined separately to distinguish simple executors from full cognitive programming runtimes.

---

## 42. Minimal Reference Engine Target

For an initial open-source reference implementation, the recommended minimal target is:

```text
A CLI-based Cognitive Engine that can:
1. load a TTML file;
2. validate basic structure;
3. resolve root inputs;
4. execute TextCompletion operations;
5. execute registered deterministic functions;
6. store intermediate outputs in a workspace;
7. resolve final outputs;
8. record an execution trace.
```

A minimal implementation does not need to support every advanced feature.

The purpose of a reference engine is to demonstrate the core idea:

```text
Thought Tree source definition
        ↓
compiled execution plan
        ↓
LLM/function execution
        ↓
intermediate artefacts
        ↓
final outputs
        ↓
trace
```

---

## 43. Suggested Initial Implementation Components

A practical reference implementation may include:

```text
engine/
  parser/
  program_model/
  compiler/
  runtime/
  workspace/
  providers/
  functions/
  validation/
  trace/
  cli/
```

### Parser

Responsible for loading TTML and converting it into source objects.

### Program Model

Defines internal classes or records for:

- Module;
- Operation;
- DataUnit;
- Collection;
- Iterator;
- Variable;
- Contract;
- OperationInstance;
- ExecutionPlan.

### Compiler

Responsible for:

- normalisation;
- symbol resolution;
- iterator expansion;
- dependency graph construction;
- collision detection;
- execution planning.

### Runtime

Responsible for:

- executing operation instances;
- invoking LLMs;
- invoking deterministic functions;
- executing submodules;
- handling retries;
- recording status.

### Workspace

Responsible for:

- storing artefacts;
- mapping logical IDs to files or records;
- versioning outputs;
- preserving intermediate artefacts.

### Providers

Adapters for LLM providers such as:

- OpenAI;
- Anthropic;
- local OpenAI-compatible APIs;
- KoboldCPP or other local inference systems.

### Functions

Registered deterministic functions such as:

- `ConcatenateFiles`;
- `CopyFile`;
- `ConvertDocumentToText`;
- `ValidateXMLAgainstXSD`;
- `CreateArchive`.

### Validation

Responsible for:

- schema validation;
- execution validation;
- contract validation, where supported.

### Trace

Responsible for producing execution trace records.

### CLI

A command-line interface for:

```text
thoughttree validate module.ttml
thoughttree dry-run module.ttml
thoughttree run module.ttml --workspace ./run_001
```

---

## 44. Suggested Execution Trace Format

A basic trace record may be JSON:

```json
{
  "run_id": "run_001",
  "module_id": "ArticleSummaryModule",
  "module_version": "0.12.0",
  "engine_version": "0.1.0",
  "started_at": "2026-01-01T12:00:00Z",
  "completed_at": "2026-01-01T12:05:00Z",
  "inputs": [
    {
      "id": "source_article",
      "path": "inputs/source_article.txt",
      "hash": "..."
    }
  ],
  "operations": [
    {
      "id": "DraftSummary",
      "type": "TextCompletion",
      "status": "completed",
      "started_at": "2026-01-01T12:00:10Z",
      "completed_at": "2026-01-01T12:01:00Z",
      "inputs": ["source_article"],
      "outputs": ["draft_summary"],
      "model": {
        "provider": "example-provider",
        "name": "example-model"
      }
    }
  ],
  "outputs": [
    {
      "id": "final_summary",
      "path": "outputs/final_summary.txt",
      "hash": "..."
    }
  ],
  "errors": []
}
```

This can evolve into a formal trace schema later.

---

## 45. Open Design Questions

The Cognitive Engine specification remains open in several areas.

Important questions include:

- What is the minimum formal TTML 1.0 feature set?
- Should the reference engine be implemented in Python, C#, TypeScript or another language?
- Should execution graphs use a standard graph format?
- How should contracts be represented in the first implementation?
- Should semantic validation be part of core conformance or an advanced feature?
- How should generated submodules be sandboxed?
- How should model capability requirements be declared?
- How should human review be represented in TTML?
- How should module versioning and dependency resolution work?
- What is the simplest useful execution trace format?
- How should prompt construction be standardised, if at all?
- How much behaviour belongs in the source specification versus engine policy?

These are intentionally left open for future contributors, implementers and maintainers.

---

## 46. Summary

The Cognitive Engine is the operational centre of the Thought Tree Framework.

It turns declarative cognitive programs into executable, inspectable and improvable processes.

It acts as:

- a compiler, by transforming source definitions into executable graphs;
- a runtime, by coordinating LLMs, functions, tools, submodules and humans;
- a workspace manager, by preserving Data Units and intermediate artefacts;
- a validator, by checking structure, execution and semantic correctness;
- a trace system, by recording what happened and why.

This separation is essential to the framework’s strategic purpose.

Thought Tree programs define cognitive work independently of any particular model provider.

The Cognitive Engine executes that work using the best available combination of models, software functions, tools and review processes.

In this way, the Cognitive Engine makes cognitive programming practical:

```text
concepts + relationships + transformations
        ↓
compiled execution graph
        ↓
LLMs + functions + tools + humans
        ↓
validated artefacts + execution trace
```

The result is a framework for moving LLM-assisted work beyond isolated prompts and opaque agent behaviour toward structured, reusable, auditable and improvable cognitive software.