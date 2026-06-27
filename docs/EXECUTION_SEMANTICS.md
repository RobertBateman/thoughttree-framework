
# Execution Semantics

**Status:** Draft  
**Applies to:** Thought Tree Framework / TTML draft  
**Intended audience:** Cognitive Engine implementers, module authors, reviewers, contributors

## 1. Purpose

Execution semantics define how a Cognitive Engine interprets, compiles and executes a Thought Tree program.

A Thought Tree program describes a cognitive workflow in terms of:

- input Data Units;
- intermediate artefacts;
- Collections;
- Operations;
- Modules;
- variables;
- iterators;
- semantic contracts;
- validation gates;
- final outputs;
- execution traces.

The Cognitive Engine is responsible for turning that declarative program into an executable process.

It does this by:

1. loading the source definition;
2. parsing it into the Thought Tree Program Model;
3. resolving inputs, variables, Data Units and Collections;
4. expanding iterators;
5. building a dependency graph;
6. planning execution;
7. invoking LLMs, deterministic functions, tools, submodules or human review;
8. validating produced outputs;
9. handling errors and retries;
10. resolving final outputs;
11. recording an execution trace.

A conformant Cognitive Engine should not treat a Thought Tree program as a simple list of prompts. It should treat it as a structured cognitive program whose operations consume, produce, validate and transform identifiable artefacts.

---

## 2. Core Execution Model

At the smallest scale, execution follows the pattern:

```text
Input Data Units → Operation → Output Data Units
```

At larger scales, this becomes a graph of transformations:

```text
Inputs
↓
Operations
↓
Intermediate Artefacts
↓
Review / Validation / Revision
↓
Final Outputs
↓
Execution Trace
```

A Thought Tree execution is therefore not merely an LLM conversation. It is an artefact-producing process.

The Cognitive Engine should preserve the relationship between:

- the source program;
- each operation;
- the inputs consumed;
- the outputs produced;
- validation results;
- execution decisions;
- errors and retries;
- final outputs.

---

## 3. Execution Lifecycle

A typical Thought Tree execution follows this lifecycle:

```text
Load Source Definition
↓
Parse Source
↓
Validate Source Structure
↓
Initialise Workspace
↓
Resolve Inputs, Variables and References
↓
Expand Iterators
↓
Resolve Collections
↓
Build Dependency Graph
↓
Validate Execution Requirements
↓
Create Execution Plan
↓
Execute Operations
↓
Validate Outputs
↓
Handle Errors, Retries and Review Gates
↓
Resolve Final Outputs
↓
Record Execution Trace
↓
Return or Store Final Outputs
```

Different Cognitive Engines may implement these stages differently, but they should preserve the intended meaning of the Thought Tree program.

---

## 4. Source Definition and Program Model

A Thought Tree program may be authored in TTML or another future source format such as YAML, JSON or a visual graph representation.

The source format is not itself the execution model.

Before execution, the Cognitive Engine should normalise the source definition into an internal Thought Tree Program Model.

The internal model should represent:

- Module metadata;
- root-level inputs;
- variables;
- iterators;
- Data Units;
- Collections;
- Operations;
- operation inputs;
- operation outputs;
- semantic types, where supported;
- contracts, where supported;
- final output declarations;
- dependencies;
- references to functions, tools, models or submodules.

For TTML, the mapping is broadly:

| TTML Element | Program Model Meaning |
|---|---|
| `<TTML>` | Thought Tree Module |
| `<Project>` | Module metadata |
| `<Inputs>` | Required initial Data Units |
| `<Vars>` | Variables |
| `<Iterators>` | Repeated execution dimensions |
| `<Collections>` | Grouped Data Units |
| `<Operations>` | Transformations |
| `<Operation>` | Executable transformation |
| `<File>` | Declared Data Unit |
| `<FileRef>` | Reference to a Data Unit |
| `<Collection>` | Declared group of Data Units |
| `<CollectionRef>` | Reference to a Collection |
| root-level `<Output>` | Final Module outputs |

The Cognitive Engine may retain the original source file as part of the execution trace.

---

## 5. Workspace Initialisation

Every execution occurs inside a workspace.

The workspace is the execution context where the Cognitive Engine stores or references:

- supplied inputs;
- intermediate Data Units;
- Collections;
- generated prompts;
- generated submodules;
- deterministic function outputs;
- tool outputs;
- validation reports;
- human review decisions;
- error reports;
- final outputs;
- execution traces.

A workspace may be implemented as:

- a local directory;
- a database;
- cloud object storage;
- an in-memory context;
- a version-controlled project;
- a project asset store;
- a hybrid storage system.

The workspace maps logical identifiers from the Thought Tree program to concrete assets.

Example logical Data Unit:

```xml
<File id="plot_outline" extension="txt"/>
```

Possible file-backed workspace representation:

```text
/workspaces/run_001/outputs/plot_outline.txt
```

Possible metadata-backed representation:

```text
asset_id: plot_outline
run_id: run_001
version: 1
mime_type: text/plain
semantic_type: PlotOutline
produced_by: GeneratePlotOutline
```

The exact storage mechanism is implementation-specific, but the mapping must remain traceable.

---

## 6. Data Unit Resolution

A Data Unit is a discrete artefact that may be supplied to, produced by, referenced by, validated by or transformed by an operation.

Before executing an operation, the Cognitive Engine must resolve every required Data Unit reference.

A Data Unit reference may resolve to:

1. a root-level input;
2. an output produced by an earlier operation;
3. a workspace asset;
4. an output produced by a submodule;
5. an external asset resolved by the engine;
6. a generated or recovered asset produced by an explicit recovery strategy.

If a required Data Unit cannot be resolved, the operation must not execute unless the engine has an explicit strategy for obtaining, generating or substituting that input.

Silent creation of missing required inputs should be invalid by default.

If the engine generates, requests, substitutes or repairs a missing Data Unit, this must be recorded in the execution trace.

---

## 7. Variables

Variables are named values available during execution.

Variables may be used for:

- iterator ranges;
- file identifiers;
- Collection identifiers;
- operation descriptions;
- output names;
- model configuration;
- tool configuration;
- contract parameters;
- module configuration.

Example:

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

The Cognitive Engine should resolve variables before execution planning.

If a required variable cannot be resolved, validation should fail.

Unless otherwise specified, variables are local to the current Module.

---

## 8. Iterator Expansion

Iterators define repeated execution dimensions.

Example:

```xml
<Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
```

If `ChapterCount` resolves to `12`, then `ChapterIterator` expands to:

```text
1, 2, 3, ..., 12
```

Iterator values may be inserted into identifiers:

```xml
<File id="draft_chapter_{{ChapterIterator}}" extension="txt"/>
```

This represents the concrete Data Units:

```text
draft_chapter_1
draft_chapter_2
...
draft_chapter_12
```

An operation expands over its active iterators.

An iterator becomes active for an operation when it appears directly in:

- a scalar `FileRef` used by the operation;
- a scalar output `File` produced by the operation;
- a `CollectionRef` identifier used by the operation;
- operation attributes or descriptions, where supported by the engine.

An iterator used only inside the internal definition of a Collection does not automatically cause the consuming operation to expand once per Collection member.

This distinction is important because Collections are often used to aggregate iterator-generated artefacts.

---

## 9. Multiple Iterators

If an operation references multiple active iterators, the default behaviour is Cartesian expansion.

Example:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="beta_reader_persona_{{PersonaIterator}}"/>

<Output>
  <File id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}" extension="txt"/>
</Output>
```

If:

```text
ChapterIterator = 1..12
PersonaIterator = 1..3
```

then the operation expands into 36 concrete operation instances:

```text
Chapter 1, Persona 1
Chapter 1, Persona 2
Chapter 1, Persona 3
Chapter 2, Persona 1
...
Chapter 12, Persona 3
```

The Cognitive Engine should make this expansion inspectable before execution, especially where execution may be expensive.

If Cartesian expansion is not intended, the module author should use Collections or another explicit aggregation strategy.

---

## 10. Collections

A Collection is a named group of Data Units.

Collections allow multiple related artefacts to be treated as one logical input or output.

Example:

```xml
<Collection id="all_revised_chapters" orderBy="ChapterIterator">
  <FileRef id="revised_chapter_{{ChapterIterator}}"/>
</Collection>
```

If `ChapterIterator` ranges from 1 to 12, this Collection represents:

```text
revised_chapter_1
revised_chapter_2
...
revised_chapter_12
```

An operation may consume the Collection as grouped input:

```xml
<CollectionRef id="all_revised_chapters"/>
```

A `CollectionRef` does not automatically cause the consuming operation to execute once per Collection member.

Instead, the Cognitive Engine resolves the Collection into an ordered set of Data Units and supplies that set to the operation as grouped context.

Collections are useful for:

- aggregating iterator outputs;
- passing many artefacts into one LLM operation;
- compiling multiple files;
- grouping feedback;
- preventing unintended output collisions;
- making grouped dependencies explicit.

If an order is declared, the engine should preserve that order when supplying the Collection to an operation or function.

---

## 11. Dependency Graph

Although the framework is called Thought Tree, execution is more accurately modelled as a directed transformation graph.

The graph contains:

- Data Unit nodes;
- Collection nodes;
- Operation nodes;
- Module nodes, where submodules are invoked;
- dependency edges;
- output edges;
- validation gates;
- review gates.

An operation depends on every Data Unit and Collection it references as input.

An operation produces one or more Data Units or Collections as output.

The Cognitive Engine should compile the program into a dependency graph before execution.

The dependency graph is used to determine:

- which operations can run;
- which operations must wait;
- which outputs are required downstream;
- whether dependencies are missing;
- whether output collisions exist;
- whether parallel execution is safe;
- whether final outputs can be produced.

By default, operations may execute in document order. However, an engine may execute independent operations in parallel if doing so does not change the logical meaning of the program.

---

## 12. Execution Planning

Before execution, the Cognitive Engine should create an execution plan.

The execution plan should identify:

- concrete operation instances;
- active iterator values for each instance;
- required input Data Units;
- required input Collections;
- expected outputs;
- operation type;
- target module, function or tool, where applicable;
- validation gates;
- semantic contracts, where supported;
- human review points;
- parallelisation opportunities;
- estimated cost or resource requirements, where available;
- error handling behaviour.

For small workflows, this plan may be implicit.

For large, expensive, sensitive or dynamically generated workflows, the plan should be inspectable before execution.

A Cognitive Engine may support a dry-run mode that validates and displays the execution plan without invoking LLMs or producing final outputs.

---

## 13. Operation Execution

Each operation is executed according to its operation type.

A basic Cognitive Engine may support:

1. `TextCompletion`
2. `ExecuteFunction`
3. `PreExisting`
4. `ProjectCompletion`
5. `DynamicCompletion`

Future engines may also support operation types such as:

- `HumanReview`;
- `ToolCall`;
- `ValidationGate`;
- `Condition`;
- `Loop`;
- `Schedule`;
- `WaitForInput`.

---

## 14. TextCompletion Operations

A `TextCompletion` operation invokes an LLM to produce one or more output Data Units.

Example:

```xml
<Operation id="DraftOutline" type="TextCompletion" desc="Draft a plot outline from the story requirements.">
  <FileRef id="story_requirements"/>
  <Output>
    <File id="plot_outline" extension="txt"/>
  </Output>
</Operation>
```

The Cognitive Engine should construct an LLM request using:

- operation description;
- resolved input Data Units;
- resolved Collections;
- relevant variables;
- active iterator values;
- Module metadata;
- declared output requirements;
- semantic contracts, where supported;
- implementation-specific system instructions;
- model selection rules.

The declared output tells the engine what artefact the LLM is expected to produce.

An engine may use internal LLM calls to improve prompt construction, validate output format or repair malformed responses. These internal calls are implementation details, but should be recorded in the execution trace where practical.

When a `TextCompletion` operation receives a Collection, the engine should supply the Collection as grouped context, preserving member identifiers and order where possible.

---

## 15. ExecuteFunction Operations

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

Functions are appropriate for tasks such as:

- copying files;
- concatenating files;
- converting formats;
- validating XML;
- querying databases;
- resizing images;
- generating checksums;
- archiving outputs;
- calling controlled APIs.

The Cognitive Engine should validate that the requested function exists before execution.

If a function receives a Collection, the Collection should be expanded into the function’s input list according to its declared order.

Function calls should be recorded in the execution trace, including:

- function name;
- function version, where available;
- input identifiers;
- output identifiers;
- start time;
- end time;
- status;
- errors, if any.

---

## 16. PreExisting Operations

A `PreExisting` operation executes an existing Thought Tree Module.

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

When executing a pre-existing submodule, the Cognitive Engine should:

1. load the target Module;
2. validate the target Module;
3. map parent operation inputs to submodule inputs;
4. execute the submodule;
5. map submodule final outputs back to the parent operation’s declared outputs;
6. record the mapping in the execution trace;
7. continue executing the parent Module.

Input/output mapping may be explicit, inferred by matching identifiers, or handled by engine-specific rules.

Ambiguous mappings should require validation or review.

---

## 17. ProjectCompletion Operations

A `ProjectCompletion` operation expands a complex operation into a newly generated submodule.

This is useful when a task is too large or ambiguous to execute as a single completion.

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

A Cognitive Engine may use an LLM to generate a new Thought Tree Module that decomposes the task into smaller operations.

The generated submodule should:

- accept mapped inputs from the parent operation;
- produce outputs satisfying the parent operation’s declared outputs;
- preserve the intent of the parent operation;
- be validated before execution;
- be stored in the workspace;
- be included in the execution trace.

Recommended execution pattern:

```text
ProjectCompletion Operation
↓
Generate Proposed Submodule
↓
Validate Submodule
↓
Review or Approve Submodule, if required
↓
Execute Submodule
↓
Map Outputs Back to Parent Operation
```

Generated submodules should not be treated as invisible implementation details. They are part of the execution history.

---

## 18. DynamicCompletion Operations

A `DynamicCompletion` operation allows the Cognitive Engine to choose the execution strategy.

A DynamicCompletion may resolve into:

- a direct `TextCompletion`;
- an `ExecuteFunction` call;
- a `PreExisting` Module call;
- a generated `ProjectCompletion` submodule;
- a sequence of smaller operations;
- a combination of the above.

DynamicCompletion is useful when the author wants to describe the intended cognitive transformation without specifying exactly how it should be executed.

Because DynamicCompletion introduces ambiguity, the engine should record:

- the chosen strategy;
- why the strategy was selected, where possible;
- any generated operations or submodules;
- validation results;
- human approval decisions, if required;
- final mapping of inputs and outputs.

DynamicCompletion must not bypass validation, traceability or security controls.

---

## 19. Output Resolution

Each operation declares the Data Units or Collections it is expected to produce.

An operation is considered complete only when:

1. execution has finished;
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

After all required operations have executed, the Cognitive Engine resolves each final output reference.

A Thought Tree execution is complete when:

- all required operations have completed;
- all final outputs can be resolved;
- final outputs are available in the workspace;
- required contracts have passed, where supported;
- no unrecovered errors remain;
- the execution trace has been recorded.

If a final output cannot be resolved, execution should be considered incomplete or failed.

---

## 20. Output Collisions

An output collision occurs when two or more operation instances attempt to write to the same logical output identifier.

Example:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}"/>

<Output>
  <File id="revised_chapter_{{ChapterIterator}}" extension="txt"/>
</Output>
```

If `PersonaIterator` has multiple values, several operation instances may attempt to produce:

```text
revised_chapter_1
```

By default, this is invalid.

A Cognitive Engine should detect output collisions during pre-execution validation.

Valid collision resolutions include:

1. including all active iterators in the output identifier;
2. aggregating the extra iterator dimension into a Collection;
3. inserting a separate aggregation operation;
4. creating versioned outputs;
5. explicitly declaring overwrite behaviour;
6. requiring human or automated review.

Silent overwriting should be invalid by default.

---

## 21. Versioning and Replacement

Outputs should be treated as versioned artefacts unless explicitly overwritten.

If an operation produces an output identifier that already exists, the Cognitive Engine should use one of the following strategies:

1. reject the operation as an output collision;
2. create a new version;
3. overwrite only if explicitly permitted;
4. preserve both outputs and require resolution;
5. invoke an error-handling or review process.

Possible file-based versioning pattern:

```text
draft_chapter_1.v1.txt
draft_chapter_1.v2.txt
draft_chapter_1.v3.txt
```

Possible metadata-based pattern:

```text
Data Unit: draft_chapter_1
Version: 1
Version: 2
Version: 3
```

The versioning mechanism is implementation-specific, but provenance should be preserved.

---

## 22. Semantic Contracts and Validation Gates

Schema validation confirms that a source definition is structurally valid.

Execution validation confirms that dependencies can be resolved and operations can run.

Semantic validation confirms that produced outputs are fit for their intended purpose.

A Thought Tree program may define semantic contracts for Data Units or operation outputs.

A contract may specify:

- expected semantic type;
- required structure;
- required sections;
- formatting rules;
- completeness criteria;
- quality criteria;
- validation functions;
- LLM review rubrics;
- human approval requirements;
- downstream usage expectations.

An operation output should be considered semantically valid only when it satisfies its associated contract.

This creates three levels of correctness:

## 22.1 Schema Correctness

The source definition is structurally valid.

Example question:

```text
Is this valid TTML?
```

## 22.2 Execution Correctness

The Cognitive Engine can resolve dependencies, execute operations and produce declared outputs.

Example question:

```text
Can this Module run?
```

## 22.3 Semantic Correctness

The produced outputs satisfy the meaning, structure and quality required by the workflow.

Example question:

```text
Is this output good enough for its intended downstream use?
```

If a semantic contract fails, the Cognitive Engine may:

- retry the operation;
- repair the output;
- invoke a review operation;
- request human intervention;
- produce a diagnostic report;
- fail the workflow.

Contract validation results should be included in the execution trace.

---

## 23. Error Handling

Execution may fail for many reasons, including:

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
- non-terminating loops;
- human review rejection.

A Cognitive Engine should report errors with enough context for diagnosis, including:

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

A Cognitive Engine may attempt recovery through:

1. retrying the operation;
2. regenerating malformed outputs;
3. invoking a repair operation;
4. invoking an error-handling Module;
5. requesting human review;
6. skipping optional operations, if explicitly supported;
7. rolling back partial outputs;
8. producing a diagnostic report.

Errors and recovery attempts must be recorded in the execution trace.

Silent failure should be avoided.

---

## 24. Human Review

A Cognitive Engine may support human review steps.

Human review may be implemented as:

- a specialised operation type;
- a pause in execution;
- a deterministic function;
- an external approval workflow;
- a review Module;
- an engine-level validation gate.

Human review is useful when:

- generated plans require approval;
- outputs are high-impact;
- semantic contract validation fails;
- output collisions require resolution;
- generated submodules need inspection;
- quality is uncertain;
- the workflow affects compliance, safety, finance, publication or reputation.

If human review changes a Data Unit, Operation, Module, contract or final output, the change should be recorded in the execution trace.

Human intervention should be treated as part of the execution process, not as an invisible external correction.

---

## 25. Parallel and Asynchronous Execution

The default execution model is sequential document-order execution.

However, a Cognitive Engine may execute independent operations asynchronously or in parallel where doing so does not change the logical result.

Parallel execution is safe when:

- operations do not depend on each other’s outputs;
- operations do not write to the same output identifiers;
- operation ordering does not affect later results;
- shared resources are handled safely;
- execution trace ordering remains clear.

Example:

Generating feedback from three beta reader personas may be parallelisable.

Revising a chapter using all feedback is not parallelisable until the feedback Collection has been produced.

Correctness and traceability should take priority over speed.

---

## 26. Loops and Conditional Execution

Simple Thought Tree programs execute in a finite ordered sequence with optional iterator expansion.

More advanced engines may support loops, conditions, scheduling or decision points.

Where loops or conditional execution are supported, they should be represented explicitly through operation types, function calls, generated submodules or future schema extensions.

A loop should define:

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

Because loops and conditionals can turn a tree into a directed graph, the Cognitive Engine should make the resulting execution structure inspectable.

---

## 27. Execution Trace

Every Thought Tree execution should produce an execution trace.

The execution trace is a record of what happened during execution. It allows the process to be inspected, debugged, reviewed, reproduced and improved.

An execution trace may include:

- source definition identifier;
- source definition version;
- Module ID;
- Module metadata;
- TTML version or source format version;
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
- model provider;
- model name;
- model version, where available;
- model settings;
- generated prompts, where appropriate;
- function calls;
- tool calls;
- submodule paths;
- generated submodules;
- output identifiers;
- output hashes;
- semantic contract results;
- validation reports;
- errors;
- retries;
- human review decisions;
- final outputs.

The goal is not always exact deterministic reproduction, because LLM behaviour may vary. The goal is to make the process explicit, inspectable, repeatable and improvable.

---

## 28. Determinism and Reproducibility

Thought Tree programs make the structure of cognitive work reproducible. They define what operations should occur, what inputs they should use and what outputs they should produce.

However, LLM-based operations may not produce identical outputs on every run unless the Cognitive Engine controls and records relevant execution settings.

For stronger reproducibility, the Cognitive Engine should record:

- model provider;
- model name;
- model version;
- temperature;
- seed, where available;
- system prompt;
- generated prompt;
- input content hashes;
- tool versions;
- function versions;
- contract versions;
- execution timestamp;
- retry count;
- validation results.

A deterministic function should produce the same output for the same inputs, assuming the same function version and environment.

An LLM operation should be treated as probabilistic unless the engine and model provider support deterministic execution.

---

## 29. Execution Completion

A Thought Tree execution is complete when:

1. all required operations have completed;
2. all required validation gates have passed;
3. all declared final outputs can be resolved;
4. final outputs are available in the workspace;
5. no unrecovered errors remain;
6. the execution trace has been recorded.

If any final output cannot be resolved, the execution should be considered incomplete or failed.

If a semantic contract fails and no recovery strategy succeeds, the execution should be considered semantically failed even if files were produced.

---

## 30. Security and Governance Considerations

A Cognitive Engine may read files, write outputs, invoke models, call tools, execute functions and generate submodules. Therefore, execution should be governed by explicit safety rules.

Important governance concerns include:

- file system permissions;
- tool access permissions;
- sandboxing deterministic functions;
- controlling side effects;
- protecting API credentials;
- preventing prompt injection;
- validating untrusted Modules;
- limiting dynamic execution;
- enforcing human approval policies;
- controlling cost;
- protecting confidential data;
- recording audit logs.

Engines should distinguish between:

- trusted and untrusted Modules;
- read-only and side-effecting operations;
- local and external model execution;
- safe and unsafe tools;
- deterministic functions and external APIs;
- advisory and required validation gates.

Dynamic or generated Modules should be validated before execution.

---

## 31. Conformance Requirements

A basic Cognitive Engine conforms to the core Thought Tree execution model if it can:

- load a Thought Tree source definition;
- parse it into the program model;
- validate its structure;
- initialise a workspace;
- resolve inputs, variables and Data Unit references;
- execute operations in document order;
- support `TextCompletion` operations;
- support `ExecuteFunction` operations where functions are registered;
- produce declared outputs;
- resolve final outputs;
- detect unresolved dependencies;
- detect output collisions;
- record an execution trace.

A more advanced Cognitive Engine may additionally support:

- Collections;
- iterator expansion;
- semantic types;
- semantic contracts;
- validation gates;
- `PreExisting` Module execution;
- `ProjectCompletion`;
- `DynamicCompletion`;
- generated submodules;
- parallel execution;
- caching;
- human review;
- module libraries;
- model selection;
- tool use;
- loop and conditional execution;
- automated testing and improvement.

A Thought Tree program should not execute if pre-execution validation detects:

- unresolved required inputs;
- invalid iterator expansion;
- missing functions;
- missing target modules;
- unintended output collisions;
- unavailable required permissions;
- unavailable required execution backends.

---

## 32. Summary

Execution semantics turn a Thought Tree program from a human-readable cognitive process definition into an executable, inspectable and traceable production process.

A Cognitive Engine should not simply send prompts to an LLM. It should:

- parse and compile the program;
- resolve dependencies;
- build an execution graph;
- execute transformations;
- preserve intermediate artefacts;
- validate outputs;
- handle errors;
- support review;
- resolve final outputs;
- record what happened.

This allows LLM-assisted work to move beyond isolated prompts and unstructured agents toward cognitive programming: modular, auditable, reusable and improvable execution of transformations over concepts and artefacts.