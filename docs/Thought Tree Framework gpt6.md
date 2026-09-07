# Thought Tree Framework
## A Framework for Structured, Verifiable and Governed Cognitive Work

**Revised working draft**  
**Proposed specification identifier:** `0.13.0-draft.1`  
**Status:** Design proposal, not a released standard

---

## Contents

1. Purpose and Scope
2. Strategic Vision
3. Design Principles
4. Core Program Model
5. Contracts, Evidence and Acceptance
6. Compilation and Execution Semantics
7. Iteration, Collections and Control Flow
8. The Cognitive Engine
9. Context Construction and Model Execution
10. Reliability, Recovery and Reproducibility
11. Security, Privacy and Governance
12. TTML: Proposed Source Format
13. Authoring Modules
14. Reference Workflows
15. Testing, Evaluation and Improvement
16. Packages, Registries and Compatibility
17. Conformance and Delivery Roadmap
18. Commercial Strategy
19. Glossary

---

# 1. Purpose and Scope

The Thought Tree Framework is a framework for defining, executing, evaluating and improving processes that combine language models, conventional software, external tools and human judgment.

Its central abstraction is:

```text
Declared inputs
      ↓
Explicit transformation
      ↓
Identifiable outputs
      ↓
Evaluation and acceptance
```

Connected transformations form a **Module**: a reusable program for a bounded task.

A Module specifies:

- the information it requires;
- the transformations it performs;
- the intermediate artefacts it preserves;
- the dependencies between transformations;
- the conditions under which outputs may be used;
- the capabilities and permissions execution requires;
- the limits on cost, time and repetition;
- and the outputs it promises to return.

The **Cognitive Engine** compiles and executes that specification. It manages dependencies, state, model and tool invocation, validation, recovery, storage and execution records.

The framework’s purpose is not to make probabilistic systems infallible. It is to make their use explicit, bounded, inspectable and empirically improvable.

## 1.1 The problem being addressed

A model can produce fluent output while:

- misunderstanding its source material;
- omitting a critical constraint;
- inventing a supporting fact;
- returning malformed data;
- applying an inappropriate tool;
- or presenting an incomplete result as finished.

Dividing a task into multiple calls does not automatically solve these problems. A poorly designed workflow can propagate errors, amplify assumptions and incur unnecessary cost.

Thought Tree addresses this by treating process design, artefact identity, evidence, evaluation and execution control as first-class concerns.

The objective is not “more steps.” It is **better-defined responsibility at each step**.

## 1.2 Appropriate uses

The framework is particularly useful when work requires several of the following:

- multiple source documents or modalities;
- reusable intermediate results;
- source-to-output traceability;
- structured extraction or synthesis;
- review and revision;
- parallel production across many items;
- human approval;
- integration with external systems;
- repeated execution;
- comparison between process versions.

Examples include technical documentation, project recovery, creative production, policy analysis, evidence preparation, research reporting and module generation.

## 1.3 When not to use it

A single model call may be preferable when the task is small, low-risk and easy to inspect.

Conventional code is preferable when the transformation is fully specified and can be implemented reliably without a model.

An existing workflow platform may already provide most of the required orchestration. In that case, Thought Tree can supply an artefact, contract and evaluation layer rather than replacing the platform.

## 1.4 Non-goals

The framework does not claim to:

- formally prove arbitrary natural-language statements;
- expose a model’s private internal reasoning;
- guarantee identical model outputs across executions;
- replace accountable professional judgment;
- make generated programs safe merely by validating their syntax;
- or guarantee that additional review calls improve quality.

These are limitations to manage, not properties to assume away.

## 1.5 Reading conventions

This draft distinguishes three kinds of statement:

- **Normative requirements:** expected behaviour of a conforming implementation.
- **Recommendations:** preferred design choices that may have justified alternatives.
- **Examples:** illustrations, not evidence of an existing implementation.

**MUST**, **MUST NOT**, **SHOULD** and **MAY** express these requirements.

The proposed version identifier and TTML extensions in this document are working design choices. They do not imply that a corresponding schema, reference engine or conformance suite has already been released.

---

# 2. Strategic Vision

## 2.1 Cognitive programming

Within this framework, **cognitive programming** means defining software processes that operate over meaning-bearing artefacts: documents, requirements, plans, designs, stories, claims, reviews and decisions.

The term describes the application domain. It does not imply that the engine understands those artefacts in a human sense.

A cognitive program combines:

```text
Semantic transformations
    Models interpret, generate, compare and synthesise.

Mechanical transformations
    Software parses, calculates, converts and assembles.

External interactions
    Tools retrieve information or perform authorised actions.

Judgment and authority
    Humans resolve ambiguity and approve consequential decisions.
```

The program makes the coordination between these forms of work explicit.

## 2.2 From prompts to maintained processes

A prompt is an instruction to a model.

A maintained process additionally defines:

- an interface;
- dependencies;
- resource limits;
- acceptance criteria;
- recovery behaviour;
- evaluation cases;
- version history;
- and accountable ownership.

Thought Tree places prompts inside such a process.

## 2.3 Structured execution and bounded agency

Explicit workflows and agentic behaviour are not mutually exclusive.

A Module may contain an operation that selects a strategy or proposes a new submodule. That flexibility remains inside a declared boundary:

```text
Task and permitted capabilities
              ↓
Proposed plan
              ↓
Structural, policy and interface checks
              ↓
Approval where required
              ↓
Bounded execution
              ↓
Evaluation and trace
```

The engine remains responsible for permissions, budgets, state and acceptance. A model proposal does not grant itself authority.

## 2.4 Model independence without false equivalence

A Module should avoid unnecessary dependence on one provider. However, different models may produce materially different results.

The framework therefore separates:

1. **Program requirements:** what capabilities and outcomes are needed.
2. **Execution bindings:** which implementations satisfy those requirements for a particular run.
3. **Qualification evidence:** which combinations have been tested successfully.

Portability means the process can be rebound and reevaluated. It does not mean model substitution is behaviourally neutral.

## 2.5 Relationship to existing systems

Thought Tree builds on established ideas from:

- workflow orchestration;
- dataflow programming;
- build systems;
- design by contract;
- software supply-chain management;
- provenance systems;
- and human approval workflows.

Its proposed contribution is a coherent application of these ideas to uncertain semantic transformations.

The framework should be judged by implementation clarity, interoperability and measured usefulness—not by claiming that every underlying technique is new.

---

# 3. Design Principles

## 3.1 Explicit dependencies

An operation MUST declare the information it consumes.

It MUST NOT depend on undeclared conversation history, ambient workspace contents or another operation’s hidden state.

External retrieval is permitted only through an explicit capability whose results are recorded.

## 3.2 Immutable artefact versions

Once committed, an artefact version MUST NOT be modified in place.

Revision creates a new version or a new logical artefact. Human edits follow the same rule.

## 3.3 One producer per declared output

Each concrete output slot MUST have one designated producer.

Multiple candidate producers require an explicit selection or merge operation. Completion order MUST NOT determine which result silently wins.

## 3.4 Evaluation is evidence, not proof

A successful validator establishes that a particular check passed under particular conditions.

An LLM judgment of factual accuracy is not a proof of factual accuracy.

Acceptance records MUST preserve the distinction between:

- mechanically established properties;
- model-assessed properties;
- human-assessed properties;
- and unresolved questions.

## 3.5 Permissions are separate from instructions

An operation description can request an action. It cannot authorise that action.

Permissions come from the execution environment and approved policy.

## 3.6 Bounded execution

Retries, repair cycles, dynamic expansion and nested module calls MUST have explicit limits.

Budgets apply to all work, including internal model calls, validation and recovery.

## 3.7 Preserve source access

Derived summaries SHOULD NOT become the sole factual basis for downstream work when primary sources remain relevant.

A source digest is a navigation aid and compression artefact, not an authoritative replacement for the source.

## 3.8 Prefer the simplest adequate process

Add an operation when it provides a useful boundary for validation, reuse, context management, responsibility or recovery.

Do not add operations merely to create the appearance of rigor.

## 3.9 Make failures visible

Missing outputs, unsupported capabilities, inconclusive checks and failed approvals MUST NOT be silently converted into success.

## 3.10 Improvement requires measurement

A revised Module is a candidate improvement until supported by evaluation evidence.

Cleaner structure, longer instructions and more review stages are not sufficient evidence by themselves.

---

# 4. Core Program Model

The program model is independent of TTML or any other source syntax.

Its core entities are:

| Entity | Role |
|---|---|
| Module | A reusable program with a declared interface |
| Data Unit | A logical artefact supplied or produced |
| Artefact Version | An immutable concrete instance of a Data Unit |
| Operation | A declared transformation |
| Operation Instance | A concrete invocation after binding and expansion |
| Collection | An ordered grouping of artefact references |
| Contract | A versioned set of acceptance criteria |
| Evaluation Record | Results and evidence from applying checks |
| Acceptance Record | A policy decision about permitted use |
| Execution Plan | The resolved executable structure |
| Run | One execution of a bound plan |
| Trace | The event record of that execution |

## 4.1 Concepts and semantic types

A **Concept** is a meaningful entity in the application domain.

A **Semantic Type** names the role an artefact is intended to fulfil, such as:

- `RequirementsRegister`;
- `PlotOutline`;
- `EvidenceMap`;
- `ReviewReport`;
- `TechnicalDesignDocument`.

Concepts need not exist as separate runtime objects. They are represented through artefact content, semantic types and relationships.

A semantic type is not a certificate of quality. Naming a document `RiskRegister` does not establish that it contains an adequate risk register.

## 4.2 Data Units and artefact versions

A **Data Unit** is a logical artefact, such as `draft_summary`.

An **Artefact Version** identifies exact content produced or supplied for that Data Unit.

An artefact version SHOULD record:

```text
logical identifier
version identifier
content digest and digest algorithm
media type
semantic type, if declared
storage reference
producing operation instance or import record
source artefact version references
creation time
sensitivity and retention metadata
```

The logical identifier, version identifier and storage path are distinct.

For example:

```text
Logical identifier: final_summary
Version identifier: run-42/revise/attempt-1/summary
Media type: text/markdown
Storage reference: object-store://workspace/...
```

Moving the content to another storage location does not change its logical identity. Changing the content creates a new artefact version.

Validation and acceptance SHOULD be represented as separate records referencing that version, rather than as a mutable “valid” flag embedded in the content.

## 4.3 Operation definitions

An Operation declares:

- an identifier;
- an execution kind;
- input and output ports;
- instructions or implementation reference;
- relevant parameters;
- capability requirements;
- effect classification;
- validation and acceptance requirements;
- timeout and budget limits;
- retry policy;
- and optional explicit control dependencies.

Inputs and outputs use named ports. Port mapping MUST NOT depend on unspecified positional ordering or model inference.

## 4.4 Execution kinds

This draft proposes four primitive execution kinds:

| Kind | Purpose |
|---|---|
| `Model` | Invoke a generative or interpretive model |
| `Function` | Invoke a registered software implementation |
| `Module` | Invoke a declared submodule |
| `Human` | Obtain a structured human response or approval |

A tool is a registered external capability, normally invoked through a `Function` adapter.

A planner is usually a `Model` operation that produces a plan or Module artefact. Executing that proposal is a separate, governed step.

This avoids mixing three different questions into one operation-type vocabulary:

1. What performs the work?
2. How is the work planned?
3. What effects may it have?

## 4.5 Effect classification

Execution kind does not determine determinism or safety.

A software function may read a changing database. A model call may transmit sensitive information to an external provider.

Each operation MUST declare or inherit an effect classification:

- **Pure:** depends only on declared inputs and fixed implementation details.
- **ReadExternal:** observes external state.
- **WriteExternal:** changes external state.
- **HumanInteraction:** requests or records a human response.

Provider communication, data egress and storage permissions are also governed by policy.

Unknown effects MUST be treated conservatively. A registry entry must not label a database query “deterministic” merely because it is implemented in code.

## 4.6 Modules

A Module contains:

```text
Identity and version
Public input ports
Public output ports
Parameters
Dependencies
Operations and control structures
Contracts
Capability requests
Resource limits
Tests and known limitations
```

A submodule has a private namespace.

It receives only explicitly mapped inputs and delegated capabilities. It returns only its declared exports.

It MUST NOT acquire access to the parent’s entire workspace by default.

## 4.7 Parameters

Parameters configure a run. Examples include:

- maximum summary length;
- target language;
- chapter count;
- permitted repair count;
- review policy.

Parameters are typed and immutable during a run.

Mutable process state belongs in explicit state artefacts, not variables whose values change invisibly.

## 4.8 Collections

A Collection is an ordered sequence of artefact references.

It has explicit rules for:

- member selection;
- ordering;
- duplicate handling;
- completeness;
- and empty results.

Passing a Collection as an input does not repeat the consuming operation.

Repeating an operation requires an explicit iteration construct.

## 4.9 Relationships

The framework distinguishes:

**Execution relationships**

- consumes;
- produces;
- waits for;
- calls;
- selects.

**Provenance relationships**

- derived from;
- revised from;
- copied from;
- imported from.

**Semantic relationships**

- reviews;
- supports;
- contradicts;
- implements;
- addresses.

Only declared execution relationships determine scheduling. A semantic annotation such as “supports” does not automatically create an executable dependency.

## 4.10 Transformation graphs

The static core is a directed acyclic graph of operations and artefacts.

Structured loops and dynamic expansion extend that model, but every concrete artefact version still has an identifiable production event.

The term **Thought Tree** remains the framework name. The execution structure is a graph, not necessarily a tree, and is distinct from any particular “tree-of-thought” model prompting technique.

---

# 5. Contracts, Evidence and Acceptance

## 5.1 Four separate questions

A useful validation system asks four different questions:

| Layer | Question |
|---|---|
| Source validity | Is the program structurally well formed? |
| Plan validity | Are its bindings, dependencies and declared capabilities valid? |
| Execution integrity | Did execution follow the declared process and produce identifiable results? |
| Output acceptance | Do the available checks and policy permit the output’s intended use? |

These questions should not be collapsed into a single “correctness” label.

Even an accepted output can contain an error that the checks failed to detect.

## 5.2 Contract definition

A Contract is a versioned set of criteria applied to one or more artefacts.

A criterion specifies:

- a stable identifier;
- the property being assessed;
- the required evidence;
- the evaluation method;
- its severity;
- and whether it blocks acceptance.

Example:

```text
Contract: GroundedSummary@1

S1 — Structure
The summary contains the required sections.
Method: deterministic section validator.
Blocking: yes.

S2 — Length
The summary contains no more than the configured word limit.
Method: versioned word-count function.
Blocking: yes.

S3 — Source support
Material factual claims are supported by the source,
or clearly identified as uncertainty.
Method: source-linked review.
Blocking: yes.

S4 — Readability
The summary is understandable to the intended audience.
Method: rubric-based review.
Blocking: no.
```

Contracts MAY apply to inputs, outputs, collections or relationships between artefacts.

For example, a coverage check may compare a requirements register with an implementation plan.

## 5.3 Evaluation outcomes

Each check returns one of:

- **Pass:** the evaluator found the criterion satisfied.
- **Fail:** the evaluator found a violation.
- **Inconclusive:** available evidence or evaluator capability was insufficient.
- **Error:** the check could not complete correctly.
- **NotApplicable:** permitted only under a declared applicability rule.

An evaluation record MUST identify:

```text
target artefact versions
contract and criterion versions
evaluator implementation or model binding
evidence references
outcome
findings
timestamp
```

A numerical score MAY supplement these outcomes. It MUST NOT obscure a blocking failure.

Uncalibrated model confidence MUST NOT be presented as a probability of correctness.

## 5.4 Acceptance policy

An Acceptance Record answers:

> May this exact artefact version be used for this declared purpose?

By default, every blocking criterion must pass. `Inconclusive` and `Error` block acceptance.

Possible dispositions are:

- **Accepted**
- **AcceptedWithWarnings**
- **NeedsReview**
- **Rejected**

Warnings, exceptions and authorising identities remain attached to the acceptance record.

A human override, where permitted, MUST record the failed criterion and the reason for accepting the residual risk. Non-overridable policy controls remain enforced.

Acceptance is scoped. A draft may be acceptable for internal review but not for publication.

## 5.5 Candidate and accepted artefacts

Generated content first exists as a **candidate**.

Candidate artefacts can be consumed by explicitly authorised evaluation and repair operations. Ordinary production consumers require accepted inputs unless their interface explicitly permits candidates.

This distinction avoids a deadlock in which an invalid draft cannot be inspected by the very operation intended to repair it.

```text
Generate candidate
        ↓
Evaluate candidate
        ↓
Pass ───────────────→ Accept
        │
        └─ Fail → Repair candidate → Evaluate again
```

Every repair produces a new version. The evaluation of an earlier version does not carry over automatically.

## 5.6 Evidence and source grounding

Factual workflows SHOULD use source references that survive intermediate transformations.

A source reference can identify:

- source artefact version;
- page or section;
- character span, record identifier or timestamp;
- and, where useful, an excerpt.

For extracted claims, record their epistemic status:

- **SourceStated:** the source explicitly states it;
- **Inferred:** derived from source material;
- **Proposed:** introduced as a recommendation or design choice;
- **Unresolved:** evidence is absent or conflicting.

“SourceStated” does not mean independently true. Sources can be mistaken.

A complete evidence system distinguishes:

1. whether a cited location exists;
2. whether it supports the associated statement;
3. whether the source is credible for that statement;
4. whether contradictory evidence exists.

These are different checks.

## 5.7 Review quality

Review operations SHOULD:

- receive the relevant primary sources;
- use concrete criteria;
- identify specific defects and evidence;
- distinguish critical errors from preferences;
- and allow an inconclusive result.

Multiple model reviewers do not automatically provide independent evidence. Shared models, prompts and source omissions can produce correlated mistakes.

Review mechanisms should themselves be evaluated against examples with known defects.

## 5.8 Downstream readiness

A good contract reflects how the output will be used.

A requirements register may need stable IDs and verification methods because downstream planning consumes those fields.

A chapter plan may need entry state, exit state and continuity constraints because drafting depends on them.

The practical test is not merely:

> Does this resemble the requested artefact?

It is:

> Does this contain enough reliable information for the next declared use?

---

# 6. Compilation and Execution Semantics

## 6.1 Lifecycle

```text
Load source
    ↓
Parse and validate source
    ↓
Resolve package dependencies and execution bindings
    ↓
Bind inputs and parameters
    ↓
Construct graph and explicit control dependencies
    ↓
Expand static iteration
    ↓
Check types, references, capabilities and limits
    ↓
Produce inspectable execution plan
    ↓
Execute and record events
    ↓
Evaluate and accept outputs
    ↓
Resolve exports and produce run manifest
```

Execution events are recorded throughout the run, not reconstructed only at the end.

## 6.2 Static validation

The compiler MUST detect, where statically decidable:

- duplicate identifiers;
- missing producers;
- unresolved references;
- incompatible port types or cardinalities;
- output collisions;
- undeclared iterator use;
- cycles outside supported control structures;
- missing package dependencies;
- unsupported required features;
- invalid contract bindings;
- missing capability bindings;
- and impossible export references.

Checks that depend on runtime content MUST be identified as deferred checks.

Static validation must not claim to establish arbitrary semantic compatibility.

## 6.3 Execution plan

The plan MUST identify:

- Module and dependency digests;
- resolved parameters;
- input artefact versions;
- concrete operation instances or bounded expansion points;
- data and control dependencies;
- execution bindings;
- contracts and acceptance gates;
- requested permissions;
- resource limits;
- and applicable failure policies.

Cost estimates SHOULD distinguish known quantities from assumptions and unknown dynamic work.

A dry run MUST NOT invoke models, perform unapproved remote retrieval or execute side effects. Any dependency fetching required for planning must be explicit.

## 6.4 Scheduling

In the proposed core model, dependency edges determine legal execution order.

Document order is used only as a stable scheduling tie-breaker. It does not create an implicit dependency.

An author who requires ordering without data transfer must declare a control dependency.

A compatibility importer for older document-order Modules must preserve that behaviour by inserting explicit ordering edges.

Independent operations MAY run concurrently if resource access and effect ordering permit it.

## 6.5 Operation readiness

An operation instance is ready only when:

1. required inputs are bound;
2. required input acceptance conditions are satisfied;
3. control dependencies are complete;
4. required permissions and resources are available;
5. the instance remains within its limits;
6. its branch is active.

Missing data MUST NOT be fabricated to make the operation runnable.

## 6.6 Output capture and publication

An operation may produce one or more candidate outputs.

The engine MUST:

1. capture the response or function result;
2. map it to declared output ports;
3. check completeness and media representation;
4. store candidate versions;
5. execute required evaluation;
6. publish accepted bindings only when the acceptance policy permits.

For multi-output operations, successful publication is atomic by default: all required outputs become available together.

Partial publication requires an explicit interface and downstream policy.

Persisting a rejected candidate for diagnosis is not successful publication.

## 6.7 Operation state

A portable implementation should distinguish at least:

```text
Pending
Ready
Running
Evaluating
WaitingForHuman
Succeeded
Failed
Blocked
Skipped
Cancelled
```

Attempts are separate records beneath an operation instance. Retrying does not erase the failed attempt.

- **Failed:** the operation encountered an unrecovered failure.
- **Blocked:** a prerequisite failed or remained unavailable.
- **Skipped:** the operation was deliberately excluded by valid control flow.
- **Cancelled:** execution was terminated by request or policy.

## 6.8 Run completion

A run succeeds only when:

- all required active operations and gates have completed successfully;
- declared exports resolve to permitted artefact versions;
- required approvals are current;
- no unrecovered required failure remains;
- and the run manifest and required trace records are durable.

A run MAY return diagnostic or partial artefacts after failure, but MUST identify the run as unsuccessful or incomplete.

## 6.9 Module calls

Module invocation requires explicit input, parameter and output mapping.

The callee’s effective permissions MUST NOT exceed those delegated by the caller and allowed by environment policy.

Budgets are allocated from the parent budget, not created afresh.

Unbounded recursive module invocation is outside the core profile.

---

# 7. Iteration, Collections and Control Flow

## 7.1 Explicit iteration

An operation repeats only over iterators explicitly declared for that operation or its enclosing map construct.

Mentioning an iterator in an instruction MUST NOT activate repetition.

This replaces inference-based expansion, in which editing descriptive text could unexpectedly multiply execution.

## 7.2 Static ranges

A range iterator declares:

- start;
- inclusive end;
- nonzero step.

This draft’s core profile uses ascending integer ranges with a positive step. A start greater than the end produces an empty range.

The compiler MUST calculate static cardinality and enforce configured expansion limits before execution.

## 7.3 Multiple dimensions

An operation with several dimensions MUST declare its combination mode:

- **Product:** all combinations;
- **Zip:** corresponding positions, requiring equal lengths;
- **Nested:** explicit nested control structure.

No multi-dimensional default is assumed.

For 12 chapters and 3 reader personas:

```text
Product: 36 operation instances.
Zip: invalid because lengths differ.
```

## 7.4 Stable instance identity

Instance identifiers include the operation identifier and bound iterator keys.

For example:

```text
ReviewChapter[chapter=4,persona=2]
```

Runtime-generated collections SHOULD use stable item keys. Reordering a collection must not silently assign an old item’s identity to a different item.

## 7.5 Collection construction

A collection definition declares which dimensions it:

- **binds** to identify a collection instance;
- **collects** to enumerate members.

For example:

```text
feedback_for_chapter[chapter]
    binds: chapter
    collects: persona
```

A revision operation iterates over `chapter` and consumes that chapter’s feedback collection. It does not iterate over `persona`.

Collections are strict by default: all expected members must be available.

Optional partial collections require explicit rules, such as minimum member count and handling of missing feedback.

## 7.6 Ordering and empty collections

Collection order MUST be deterministic.

Numeric iterator order is numeric, not lexicographic.

Functions consuming collections must specify whether an empty collection is:

- valid and yields an identity result;
- valid but produces a warning;
- or invalid.

For example, concatenating zero sections should not produce a supposedly complete technical document unless the contract explicitly allows an empty document.

## 7.7 Conditional execution

Conditional execution is an extension to the static core.

A condition consumes an explicit decision artefact and selects a declared branch.

Model-generated decisions MUST be parsed into a finite permitted set. Invalid or ambiguous decisions follow a declared fallback, normally review or failure.

Outputs from alternative branches require an explicit merge or selection binding.

A skipped branch is not a successful producer.

## 7.8 Bounded loops

A loop declares:

- initial state;
- body Module;
- state transition mapping;
- stopping condition;
- maximum iterations;
- time and cost limits;
- and exhaustion behaviour.

A review-and-repair loop should normally stop when:

- acceptance criteria pass;
- the repair limit is reached;
- the budget is exhausted;
- or further repair is judged inappropriate under declared policy.

Exhausting the limit is not success.

## 7.9 Runtime expansion

Some workflows cannot know their item count before processing input.

Runtime expansion is permitted only at declared expansion points. The engine MUST:

1. materialise a bounded item manifest;
2. validate item keys and cardinality;
3. check new dependencies and output identities;
4. enforce remaining budgets and permissions;
5. record the resulting plan extension;
6. schedule the new instances.

The original plan must clearly identify where such expansion may occur.

## 7.10 Scheduled work

Recurring monitoring should normally be implemented as repeated bounded runs, triggered by a scheduler.

Historical state is an explicit input and output.

Updating shared historical state requires version checking or another concurrency-control mechanism. Two reporting runs must not silently overwrite each other’s state.

---

# 8. The Cognitive Engine

## 8.1 Responsibilities

The Cognitive Engine is logically composed of:

1. **Source frontend**  
   Parses TTML or another supported representation.

2. **Compiler and planner**  
   Resolves symbols, constructs graphs and checks executable structure.

3. **Binding and policy service**  
   Selects approved implementations and enforces capability constraints.

4. **Scheduler and runtime**  
   Executes ready work and manages operation state.

5. **Artefact service**  
   Stores immutable versions and resolves logical references.

6. **Evaluation service**  
   Runs checks and creates evaluation records.

7. **Acceptance service**  
   Applies release and downstream-use policies.

8. **Trace service**  
   Records execution events and provenance.

9. **Human interaction service**  
   Handles review tasks, approvals, edits and resumptions.

These are logical responsibilities, not a requirement for nine separate services. A reference implementation may implement them in one process.

## 8.2 Intermediate representation

A Cognitive Intermediate Representation should contain:

```text
Module interface
Operation definitions
Named ports and bindings
Artefact declarations
Collection definitions
Data and control edges
Contract applications
Capability requests
Expansion boundaries
Resource limits
Source locations
```

Mutable run state is stored separately from this representation.

The intermediate representation should support graph inspection and validation before model execution.

## 8.3 Execution bindings

An execution binding resolves an abstract operation to an implementation.

A model binding records:

- provider;
- model identifier and revision, where available;
- request adapter version;
- supported modalities;
- output mode;
- configured parameters;
- context limits;
- relevant privacy and residency properties.

A function binding records:

- implementation identity and version;
- package or binary digest;
- input and output schemas;
- effect classification;
- required permissions;
- timeout behaviour;
- idempotency and retry properties.

## 8.4 Registries

Registries may catalogue Modules, functions, models, contracts, validators and tools.

Registry availability is not permission to execute.

Registry entries SHOULD include:

- identity and version;
- immutable digest;
- interface;
- required permissions;
- known limitations;
- qualification evidence;
- owner or publisher;
- deprecation status.

For a minimal engine, a local locked manifest is sufficient. A network marketplace is not a prerequisite.

## 8.5 Human review

Human interaction is a durable workflow state, not a blocking chat session.

A review request identifies:

- the exact artefact versions being reviewed;
- requested decision;
- relevant criteria and evidence;
- authorised reviewer role;
- expiry or timeout behaviour;
- and permitted actions.

Approval binds to artefact digests and the relevant action. Editing the artefact invalidates approval for the previous version.

A model-simulated persona MUST NOT be represented as a human reviewer.

---

# 9. Context Construction and Model Execution

## 9.1 Context is an execution input

The information actually sent to a model can differ from the logical input set because of extraction, retrieval, formatting or context limits.

The engine MUST therefore create a **Context Manifest** for each model request.

It identifies:

- input artefact versions;
- selected excerpts or records;
- transformations applied;
- ordering;
- omitted material;
- truncation or compression;
- template and adapter versions;
- and effective request parameters.

## 9.2 Instruction and data separation

The engine SHOULD construct model requests with distinct roles for:

1. engine-enforced instructions and policy;
2. trusted Module instructions;
3. output requirements;
4. input artefacts as data;
5. tool observations as data.

Documents and tool results must not be promoted into instruction authority merely because they contain imperative language.

Delimiters and warnings can help, but they are not sufficient security controls. Capability enforcement remains outside the model.

## 9.3 No silent truncation

If required context exceeds the selected model’s capacity, the engine MUST NOT silently omit it.

Permitted responses include:

- fail with a diagnostic;
- select an approved larger-context binding;
- use a declared retrieval strategy;
- or invoke an explicit partitioning or compression process.

Any compression creates a new artefact with provenance and known coverage limitations.

## 9.4 Context minimisation

Operations should receive enough context to perform their task, not the entire project by default.

Excessive context can increase:

- cost;
- latency;
- distraction;
- privacy exposure;
- and inconsistency.

Relevant primary sources should nevertheless remain accessible to factual reviewers and repair operations.

## 9.5 Structured output capture

For multiple outputs, the engine SHOULD use structured response mechanisms or an explicit output envelope.

It MUST NOT infer file boundaries from arbitrary prose headings unless a versioned parser defines that behaviour.

Model output is untrusted input to parsers and tools.

## 9.6 Internal model calls

An engine may perform internal calls for extraction, formatting or repair only within declared execution policy.

Every internal call consumes budget and produces a trace event.

The engine MUST NOT silently transform a one-call operation into an unrestricted agent loop.

## 9.7 Inspectability without private reasoning

The framework records observable work products:

- source selections;
- plans;
- claims;
- reviews;
- tool calls;
- concise decision explanations;
- outputs and evaluations.

It does not require access to a model’s private internal reasoning. Auditability should rely on evidence and observable actions rather than purported transcripts of hidden cognition.

---

# 10. Reliability, Recovery and Reproducibility

## 10.1 Failure categories

Failures should be classified because different failures require different responses.

| Category | Typical response |
|---|---|
| Invalid source or unresolved dependency | Correct the program or bindings |
| Transient provider failure | Bounded retry |
| Malformed response | Parse failure or bounded repair |
| Failed content criterion | Revision or human review |
| Permission denial | Stop or obtain authorised configuration change |
| Uncertain external write | Reconcile before retrying |
| Budget exhaustion | Stop with preserved diagnostics |
| Human rejection | Follow declared rejection path |

## 10.2 Retry and repair are different

A **retry** repeats an invocation after an execution failure.

A **repair** uses a candidate artefact and defect report to produce a revised candidate.

Retrying a provider timeout is not the same as repairing an unsupported claim.

Policies MUST specify limits separately and preserve each attempt.

Changing models during recovery is a binding change and must be recorded. It is allowed only by the declared fallback policy.

## 10.3 External side effects

External writes require special handling.

A publishing workflow should separate:

```text
Prepare action payload
          ↓
Validate and approve payload
          ↓
Execute authorised action
          ↓
Record receipt and resulting state
```

The approval binds to the exact payload and target.

Where supported, external writes SHOULD use idempotency keys.

If a timeout leaves it unknown whether an action occurred, the engine MUST reconcile with the external system before blindly retrying.

The framework does not promise universal exactly-once external execution. That property depends on the external service and adapter.

## 10.4 Recovery and resumability

The engine SHOULD checkpoint after durable operation transitions.

After restart, it reconstructs state from stored records and distinguishes:

- completed and committed work;
- incomplete local computation;
- uncertain external interactions;
- and pending human decisions.

Resume MUST NOT automatically repeat a side effect whose outcome is unknown.

## 10.5 Caching

A cache key should include all output-relevant dependencies, including:

- operation definition;
- input content digests;
- parameters;
- implementation and adapter versions;
- model configuration;
- context manifest;
- and relevant environment assumptions.

Pure functions may be cached when their determinism assumptions hold.

Model-result reuse must be an explicit policy choice.

External reads need snapshot or freshness rules. External writes must not be treated as ordinary reusable computations.

Generation caches and acceptance caches should be separate: changing a contract may require reevaluation without regeneration.

Cache access MUST respect tenant, permission and sensitivity boundaries.

## 10.6 Incremental execution

When an input, instruction or binding changes, the engine identifies affected downstream work.

Unaffected results may be reused only if their dependency and policy requirements still hold.

A human replacement of an intermediate artefact creates a forked execution lineage, not a retroactive rewrite of the original run.

## 10.7 Reproducibility levels

The framework distinguishes:

1. **Process reproducibility**  
   The same program and binding rules can be executed again.

2. **Recorded replay**  
   Stored responses and observations can be reused without repeating external generation.

3. **Computational reproducibility**  
   Fixed deterministic transformations produce identical results.

4. **Statistical repeatability**  
   Repeated probabilistic runs exhibit comparable performance distributions.

A model seed or low temperature alone does not guarantee identical output.

## 10.8 Resource accounting

Budgets include:

- model input and output usage;
- tool calls;
- validation;
- retries;
- repair;
- dynamic planning;
- storage or compute where tracked.

The engine SHOULD reserve estimated capacity before dispatch and reconcile actual usage afterward.

Where an external service cannot guarantee a hard monetary bound, the engine must state the limitation rather than advertise an unenforceable ceiling.

---

# 11. Security, Privacy and Governance

Security is part of the core architecture, not a late-stage feature.

## 11.1 Threat model

Potentially hostile inputs include:

- source documents;
- retrieved pages;
- tool outputs;
- model responses;
- generated Modules;
- third-party packages;
- and compromised external services.

Relevant threats include:

- prompt injection;
- data exfiltration;
- unauthorised external actions;
- path traversal;
- malicious code execution;
- dependency substitution;
- approval bypass;
- denial of service;
- and leakage through traces or caches.

## 11.2 Least privilege

A Module requests capabilities. The environment grants a permitted subset.

Capabilities should be scoped by action and resource, for example:

```text
Read approved source collection.
Use approved model endpoint.
Write to run-local artefact storage.
Create a draft in a named publishing workspace.
```

Broad filesystem, network or account access should not be granted when narrower capabilities suffice.

## 11.3 Untrusted source handling

Source content may describe instructions relevant to the task, but it cannot alter engine policy or permissions.

An instruction discovered in a document such as “send all files to this URL” remains document content, not executable authority.

Generated plans are likewise untrusted until validated and authorised.

## 11.4 Sandboxing

Executable functions and generated code require appropriate isolation.

Controls may include:

- process or container isolation;
- filesystem restrictions;
- network allowlists;
- CPU, memory and time limits;
- read-only package environments;
- and denial of access to undeclared secrets.

TTML parsing MUST disable external entity resolution and unrestricted schema fetching.

Logical artefact identifiers MUST NOT be used directly as unrestricted filesystem paths.

## 11.5 Secrets

Secrets belong in a dedicated credential mechanism.

They MUST NOT be embedded in:

- Module source;
- model prompts;
- exported artefacts;
- or ordinary trace payloads.

Adapters should obtain scoped credentials at invocation time.

## 11.6 Data classification and egress

The engine SHOULD enforce sensitivity, residency and provider-use restrictions.

Derived artefacts conservatively inherit relevant source sensitivity unless an authorised declassification process says otherwise.

A generated summary is not automatically safe to export merely because it is shorter than its source.

## 11.7 Trace privacy and retention

Traceability does not require unrestricted permanent storage of every payload.

The engine should separate:

- operational event metadata;
- content payloads;
- secrets;
- and audit exports.

Access controls, redaction, retention and deletion apply to each.

If content is deleted under policy, the trace should retain an authorised tombstone or metadata record indicating that replay is no longer possible. Content hashes may themselves be sensitive and require protection.

## 11.8 Generated Modules

Before execution, a generated Module MUST undergo:

- source validation;
- interface checks;
- capability checks;
- bounded expansion checks;
- dependency resolution;
- policy evaluation;
- and approval where required.

It MUST NOT:

- grant itself additional permissions;
- weaken its parent’s acceptance requirements;
- replace the evaluator used to approve itself;
- or promote itself into a trusted registry release.

## 11.9 Governance ownership

Production Modules should identify owners for:

- process intent;
- technical maintenance;
- data handling;
- contract policy;
- and release approval.

No execution framework removes the need to identify who is accountable for deploying and using its outputs.

---

# 12. TTML: Proposed Source Format

## 12.1 Role and status

Thought Tree Markup Language is the framework’s XML-based authoring and interchange format.

TTML represents the program model; it does not define the entire runtime implementation.

This section proposes a more explicit successor to the file-oriented `0.12.0` examples in the source document. The changes are not assumed to be backward compatible.

The XML below illustrates the proposed design. A released standard requires an accompanying XSD, semantic validator, reference implementation and test suite.

The name TTML is also used outside this framework. Packages and tooling SHOULD therefore use an unambiguous namespace and the full name **Thought Tree Markup Language** where confusion is possible.

## 12.2 Proposed changes

The proposed format:

- uses `Artifact` rather than `File` as the primary declaration;
- separates `Description` from executable `Instruction`;
- uses named input and output ports;
- requires explicit iteration;
- separates execution kind from effect classification;
- introduces explicit contracts and release gates;
- separates Module version from language version;
- requires declared external dependencies and bindings;
- and rejects unsupported required features.

## 12.3 High-level structure

```xml
<TTML xmlns="urn:thought-tree:ttml:0.13-draft"
      languageVersion="0.13.0-draft.1">

  <Module id="..." moduleVersion="...">
    <Description>...</Description>

    <Requires>...</Requires>
    <Parameters>...</Parameters>
    <Inputs>...</Inputs>
    <Contracts>...</Contracts>
    <Iterators>...</Iterators>
    <Collections>...</Collections>
    <Operations>...</Operations>
    <Gates>...</Gates>
    <Exports>...</Exports>
  </Module>

</TTML>
```

Empty optional sections may be omitted.

Storage locations, credentials and provider-specific configuration belong in the execution binding manifest rather than portable Module source.

## 12.4 Proposed element meanings

| Element | Meaning |
|---|---|
| `Module` | Program identity, version and interface |
| `Requires` | Required profiles and external bindings |
| `Parameter` | Typed immutable run configuration |
| `Artifact` | Logical artefact declaration |
| `Input` | Named binding from an artefact or collection |
| `Instruction` | Trusted operation instruction |
| `Operation` | Primitive transformation |
| `Iterator` | Repetition domain |
| `Collection` | Explicit grouping of references |
| `Contract` | Criteria evaluated by named checks |
| `Gate` | Acceptance decision using check results |
| `Export` | Public output binding subject to required gates |

## 12.5 Example: source-grounded summary

The following example uses named external bindings. It is complete as an illustration of the Module structure, but execution requires the referenced model profiles and validator.

The `review` binding must return the structured findings expected by `GroundingChecks`.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<TTML xmlns="urn:thought-tree:ttml:0.13-draft"
      languageVersion="0.13.0-draft.1">

  <Module id="GroundedArticleSummary"
          moduleVersion="1.0.0-draft.1">

    <Description>
      Draft, review and revise a summary, then evaluate
      the exact revised version before export.
    </Description>

    <Requires>
      <Profile id="core"/>
      <Profile id="evaluation"/>
      <Binding id="writer" kind="Model"/>
      <Binding id="reviewer" kind="Model"/>
      <Binding id="summary_checks" kind="Function"/>
    </Requires>

    <Parameters>
      <Parameter name="max_words"
                 type="integer"
                 default="300"
                 min="50"
                 max="1000"/>
    </Parameters>

    <Inputs>
      <Artifact id="source_article"
                mediaType="text/plain"
                semanticType="Article"/>
    </Inputs>

    <Contracts>
      <Contract id="SummaryChecks" version="1">
        <Criterion id="structure" blocking="true"/>
        <Criterion id="length" blocking="true"/>
      </Contract>

      <Contract id="GroundingChecks" version="1">
        <Criterion id="source_support" blocking="true"/>
        <Criterion id="important_caveats" blocking="true"/>
      </Contract>
    </Contracts>

    <Operations>

      <Operation id="DraftSummary"
                 kind="Model"
                 binding="writer">
        <Input port="source" ref="source_article"/>
        <Instruction>
          Produce a summary of at most {{max_words}} words.
          Preserve the main argument, key evidence and
          important caveats. Do not add unsupported facts.
        </Instruction>
        <Output port="summary">
          <Artifact id="draft_summary"
                    mediaType="text/markdown"
                    semanticType="Summary"/>
        </Output>
      </Operation>

      <Operation id="ReviewDraft"
                 kind="Model"
                 binding="reviewer">
        <Input port="source" ref="source_article"/>
        <Input port="candidate" ref="draft_summary"/>
        <Instruction>
          Compare the draft against the source.
          Identify specific unsupported claims, omissions
          and lost caveats. Reference the relevant source
          locations. Distinguish defects from preferences.
        </Instruction>
        <Output port="review">
          <Artifact id="draft_review"
                    mediaType="application/json"
                    semanticType="ReviewReport"/>
        </Output>
      </Operation>

      <Operation id="ReviseSummary"
                 kind="Model"
                 binding="writer">
        <Input port="source" ref="source_article"/>
        <Input port="draft" ref="draft_summary"/>
        <Input port="review" ref="draft_review"/>
        <Instruction>
          Revise the draft using the source and review.
          Apply corrections supported by the source.
          Preserve correct material. Return only the summary,
          with at most {{max_words}} words.
        </Instruction>
        <Output port="summary">
          <Artifact id="final_candidate"
                    mediaType="text/markdown"
                    semanticType="Summary"/>
        </Output>
      </Operation>

      <Operation id="CheckFinalStructure"
                 kind="Function"
                 binding="summary_checks"
                 effect="Pure">
        <Input port="candidate" ref="final_candidate"/>
        <Argument name="max_words" value="{{max_words}}"/>
        <Output port="evaluation">
          <Artifact id="structure_evaluation"
                    mediaType="application/json"
                    semanticType="EvaluationRecord"/>
        </Output>
      </Operation>

      <Operation id="CheckFinalGrounding"
                 kind="Model"
                 binding="reviewer">
        <Input port="source" ref="source_article"/>
        <Input port="candidate" ref="final_candidate"/>
        <Instruction>
          Evaluate the exact final candidate against
          GroundingChecks. Return a structured outcome for
          each criterion with source-linked findings.
          Use Inconclusive where evidence is insufficient.
        </Instruction>
        <Output port="evaluation">
          <Artifact id="grounding_evaluation"
                    mediaType="application/json"
                    semanticType="EvaluationRecord"/>
        </Output>
      </Operation>

    </Operations>

    <Gates>
      <Gate id="ReleaseSummary" target="final_candidate">
        <Require contract="SummaryChecks"
                 result="structure_evaluation"/>
        <Require contract="GroundingChecks"
                 result="grounding_evaluation"/>
      </Gate>
    </Gates>

    <Exports>
      <Export port="summary"
              ref="final_candidate"
              requiresGate="ReleaseSummary"/>
      <Export port="grounding_report"
              ref="grounding_evaluation"/>
    </Exports>

  </Module>
</TTML>
```

The gate MUST verify that each evaluation references the exact target artefact version and the expected criteria. A loosely related review document is not a valid substitute.

This example deliberately reevaluates the final revision. Reviewing only the initial draft would leave new revision errors unchecked.

## 12.6 Explicit iteration fragment

```xml
<Iterators>
  <Iterator id="chapter"
            from="1"
            to="{{chapter_count}}"
            step="1"/>

  <Iterator id="persona"
            from="1"
            to="{{persona_count}}"
            step="1"/>
</Iterators>

<Collections>
  <Collection id="feedback_for_{{chapter}}"
              binds="chapter"
              collect="persona"
              orderBy="persona"
              completeness="all">
    <ArtifactRef id="feedback_{{chapter}}_{{persona}}"/>
  </Collection>
</Collections>
```

Feedback production declares:

```xml
<Operation id="ReviewChapter"
           kind="Model"
           binding="reader_simulation"
           iterate="chapter persona"
           combine="product">
  ...
</Operation>
```

Chapter revision declares:

```xml
<Operation id="ReviseChapter"
           kind="Model"
           binding="writer"
           iterate="chapter">
  <Input port="draft" ref="chapter_draft_{{chapter}}"/>
  <Input port="feedback"
         collectionRef="feedback_for_{{chapter}}"/>
  ...
</Operation>
```

The second operation executes once per chapter.

## 12.7 Template substitution

The portable template mechanism performs typed value insertion, not arbitrary code execution.

This draft permits `{{name}}` references to bound parameters and explicitly active iterators.

It does not permit unrestricted expression evaluation, filesystem access or dynamic code within template expressions.

Invalid substitutions fail validation.

## 12.8 Schema and semantic validation

The XSD validates XML structure.

A separate semantic validator checks:

- reference resolution;
- type and port compatibility;
- iteration scope;
- graph validity;
- output uniqueness;
- contract bindings;
- and required feature support.

Passing the XSD alone is not sufficient for executable conformance.

## 12.9 Migration from earlier TTML

A migration tool should:

1. import `File` and `FileRef` as logical artefact declarations;
2. move storage hints into a binding manifest;
3. convert `desc` into a description and initial instruction;
4. infer historical iterator behaviour once, then emit explicit declarations;
5. preserve document-order semantics using control edges;
6. require explicit submodule port mappings;
7. convert `TextCompletion` into `Model`;
8. convert `ExecuteFunction` into `Function`, with reviewed effect classification;
9. convert `PreExisting` into `Module`;
10. decompose dynamic completion into proposal, validation and execution stages.

Ambiguous migration MUST produce a diagnostic for review rather than silently selecting new semantics.

---

# 13. Authoring Modules

## 13.1 Begin with a transformation statement

Use:

> Given **these inputs**, produce **these outputs**, for **this use**, subject to **these constraints**, with **these acceptance criteria**.

Include permitted uncertainty and what the Module must not invent.

## 13.2 Define success before decomposition

Specify:

- required deliverables;
- acceptable omissions;
- source fidelity requirements;
- maximum cost and latency;
- human review requirements;
- and failure behaviour.

Then select the simplest workflow likely to meet them.

## 13.3 Choose useful boundaries

Create an intermediate artefact when it supports:

- independent checking;
- reuse;
- context reduction;
- collaboration;
- stable handoff;
- or local recovery.

Avoid materialising every minor stylistic adjustment as a separate operation.

## 13.4 Separate extraction from proposal

A technical workflow should distinguish:

```text
What the sources say
What can reasonably be inferred
What the system proposes
What remains unresolved
```

Do not merge these into an authoritative-looking document without labels.

## 13.5 Preserve primary-source routes

When creating a source digest, retain source identifiers and locations.

Give reviewers and repair operations access to the relevant originals, not only previous summaries.

## 13.6 Review the final version

If revision can introduce defects, validate after revision.

A workflow that ends with “make final edits” after its last check has an unvalidated final transformation.

## 13.7 Select execution mechanisms deliberately

Use:

- models for ambiguity and synthesis;
- software for precise transformations;
- tools for explicit external access;
- humans for consequential authority and unresolved judgment.

Do not ask a model to concatenate files, calculate exact totals or validate XML when reliable software can do the work.

## 13.8 Specify failure paths

Every important gate should have a declared outcome:

- repair;
- retry;
- review;
- reject;
- or stop with diagnostics.

“Continue anyway” is an explicit risk decision, not a sensible default.

## 13.9 Test the smallest vertical slice

Before expanding to hundreds of operations:

1. run one representative input;
2. inspect every important artefact;
3. inject a known defect;
4. confirm the check detects it;
5. verify failure propagation;
6. measure cost and review effort.

Only then scale repetition.

## 13.10 Authoring checklist

A release candidate should answer:

- Are all inputs and exports explicit?
- Is each output uniquely produced?
- Are source facts separated from proposals?
- Are collections and iteration unambiguous?
- Are relevant primary sources available to reviewers?
- Are final revisions reevaluated?
- Are permissions and budgets bounded?
- Can rejected candidates be inspected safely?
- Are external actions separately authorised?
- Are human approvals bound to exact versions?
- Are tests representative?
- Is the process simpler than its alternatives while meeting its requirements?

---

# 14. Reference Workflows

## 14.1 Technical design documentation

### Goal

Convert incomplete design material into an engineering review package without presenting invented decisions as settled requirements.

### Workflow

```text
Source files
    ↓
Extract text and preserve source locations
    ↓
Create source index and digest
    ↓
Extract requirements, constraints and open questions
    ↓
Validate source links and classify epistemic status
    ↓
Propose architecture alternatives
    ↓
Resolve consequential choices with authorised reviewers
    ↓
Draft sections from accepted requirements and decisions
    ↓
Assemble deterministically
    ↓
Review coverage, consistency and unsupported claims
    ↓
Revise affected sections
    ↓
Reassemble and reevaluate
    ↓
Release approved design package
```

### Exports

- technical design document;
- requirements coverage matrix;
- decision log;
- assumptions and unresolved questions;
- evaluation and approval records.

The architecture proposal depends on accepted requirements and scope. It should not be generated independently merely because parallel execution is convenient.

## 14.2 Creative production

### Goal

Produce a coherent creative work while preserving authorial control.

### Workflow

```text
Creative brief
    ↓
Story principles and continuity model
    ↓
Characters, setting and plot development
    ↓
Chapter plans with entry and exit states
    ↓
Draft chapters
    ↓
Local reviews and cross-chapter continuity review
    ↓
Editorial decisions
    ↓
Revise affected chapters
    ↓
Deterministic assembly
    ↓
Final continuity and editorial checks
```

Chapter drafting may run in parallel where chapter plans provide sufficient shared state. Otherwise, dependencies should be sequential or grouped.

Simulated reader personas provide different critique prompts, not evidence from actual readers.

Creative quality remains subjective. Contracts should protect required structure and constraints without forcing all work into a rigid stylistic template.

## 14.3 Compliance preparation

### Goal

Prepare reviewable evidence and gap analysis for qualified professionals.

### Workflow

```text
Versioned policies, obligations and evidence
    ↓
Extract candidate obligations with source references
    ↓
Review applicability and jurisdiction assumptions
    ↓
Map evidence to obligations
    ↓
Classify supported, missing and uncertain evidence
    ↓
Draft gap analysis
    ↓
Prepare remediation proposals
    ↓
Qualified human review
    ↓
Export approved preparation pack
```

The workflow MUST distinguish:

- evidence not found in supplied material;
- evidence known not to exist;
- possible nonconformance;
- and a professionally reviewed conclusion.

It must not convert missing retrieval results into a definitive legal finding.

## 14.4 Research monitoring

### Goal

Produce recurring updates with explicit historical state and source freshness.

Each scheduled run receives:

- topic definition;
- previous accepted state;
- source policy;
- time window;
- retrieval limits.

It produces:

- source snapshot;
- change summary;
- significance assessment;
- uncertainty register;
- report;
- proposed next state.

State is updated only after required acceptance checks, using concurrency control.

## 14.5 Module generation

### Goal

Turn a bounded task into a candidate executable Module.

```text
Task + interface + permitted capabilities
                  ↓
Extract requirements
                  ↓
Design candidate Module
                  ↓
Generate source
                  ↓
Validate syntax, graph, permissions and limits
                  ↓
Run controlled tests
                  ↓
Review intended behaviour
                  ↓
Approve exact Module digest
                  ↓
Execute under delegated permissions
```

Generation and execution remain distinct.

A generated Module that passes syntax checks but lacks sufficient tests should be labelled accordingly, not described as verified.

---

# 15. Testing, Evaluation and Improvement

## 15.1 Separate program tests from output evaluation

A program test examines the engine or Module behaviour.

An output evaluation examines produced content.

Both are necessary, but they answer different questions.

## 15.2 Test layers

### Structural tests

Check parsing, schemas, identifiers and declared interfaces.

### Compilation tests

Check dependency construction, cycle rejection, output collisions and iteration expansion.

### Runtime tests

Check scheduling, state transitions, failure propagation, checkpointing and resumption.

### Contract tests

Check validators against known valid and invalid artefacts.

### Semantic evaluations

Assess grounding, completeness, usefulness and task quality.

### Security tests

Exercise hostile source instructions, unauthorised capability requests, path traversal, malformed outputs and generated-module privilege escalation.

### Operational tests

Measure cost, latency, concurrency, resource use and recovery from provider outages.

## 15.3 Test cases

A test case should specify:

```text
Input bundle
Expected interfaces and invariants
Known defects or edge conditions
Required acceptance behaviour
Permitted uncertainty
Resource bounds
Evaluation method
```

Include normal, minimal, contradictory, incomplete, oversized and adversarial inputs.

## 15.4 Design review is not execution evidence

A reviewer may predict that a Module will handle a scenario.

That is useful design analysis, but it is not a passed test.

Evaluation reports MUST distinguish:

- inspected;
- simulated;
- executed;
- and independently reviewed.

## 15.5 Baselines

Candidate workflows should be compared with relevant simpler alternatives:

- a single model call;
- a short prompt chain;
- deterministic processing;
- an existing workflow;
- or a human-assisted process.

Measure accepted outcome quality, not merely the number of checks passed.

Useful metrics include:

- acceptance rate;
- source-grounding error rate;
- omission rate;
- human correction time;
- cost per accepted deliverable;
- end-to-end latency;
- rejection and escalation frequency;
- validator false acceptance and false rejection rates.

## 15.6 Statistical evaluation

Probabilistic Modules should be evaluated across repeated runs and representative input sets.

Reports should include sample sizes, variability and material limitations.

Do not claim meaningful superiority from one favourable demonstration.

## 15.7 Improvement lifecycle

```text
Freeze baseline
      ↓
Identify observed failure or cost problem
      ↓
Form a specific improvement hypothesis
      ↓
Create candidate change
      ↓
Run structural and security tests
      ↓
Evaluate on development cases
      ↓
Evaluate on held-out cases
      ↓
Compare quality, cost and operational risk
      ↓
Approve, reject or continue testing
      ↓
Release a new version
```

Examples of testable hypotheses:

- primary-source access reduces unsupported corrections;
- structured review findings reduce repair failures;
- eliminating an unnecessary critique step lowers cost without harming quality;
- a coverage matrix reduces requirement omissions.

## 15.8 Protect against evaluator gaming

An improvement process MUST NOT silently weaken acceptance criteria to make its candidate pass.

Changes to contracts, datasets or evaluators are separate versioned changes requiring their own review.

Where practical, held-out evaluation material should not be exposed to the process generating candidates.

## 15.9 Promotion and rollback

Stable Modules are immutable releases.

An improvement workflow produces a candidate package, comparison evidence and release recommendation. Promotion requires an authorised policy decision.

Rollback changes the selected release for future runs. It does not erase traces or undo already completed external actions.

## 15.10 Continuous improvement without uncontrolled self-modification

The framework supports assisted improvement of its own Modules.

It does not require a runtime to rewrite its active program, evaluator and policy during execution.

Production improvement should normally occur in a separate development and release process.

---

# 16. Packages, Registries and Compatibility

## 16.1 Module package

A distributable package should include:

```text
Module source
Package manifest
Dependency lockfile
Contracts and schemas
Tests and fixtures, where distributable
Documentation
Licence
Known limitations
Evaluation summary
Release notes
```

Package identity includes version and immutable digest.

## 16.2 Lockfiles

Execution resolves exact versions of:

- submodules;
- function adapters;
- validators;
- contracts;
- source schemas;
- prompt templates;
- and relevant model bindings.

Model services may not expose immutable versions. The lockfile and trace must record that limitation.

## 16.3 Compatibility dimensions

Compatibility has several dimensions:

- source-language compatibility;
- interface compatibility;
- semantic-contract compatibility;
- capability compatibility;
- operational compatibility;
- empirical quality compatibility.

A Module can remain structurally compatible while its output quality changes materially.

## 16.4 Version policy

A release policy should distinguish:

- breaking interface or acceptance changes;
- backward-compatible capability additions;
- implementation corrections;
- instruction changes requiring reevaluation.

Prompt changes must not be treated as inconsequential merely because input and output field names remain unchanged.

## 16.5 Unknown features

An engine MUST reject a Module that requires an unsupported semantic feature.

Unknown optional metadata may be preserved without interpretation.

Silently ignoring a validation gate, permission restriction or loop bound is nonconforming behaviour.

## 16.6 Interoperability

Two engines are interoperable when they preserve the declared program semantics and exchange compatible artefacts, plans and records.

They are not required to generate identical natural-language content.

Portable interchange should prioritise:

- exact identities;
- named bindings;
- declared dependencies;
- contract outcomes;
- effect and permission declarations;
- and trace structure.

---

# 17. Conformance and Delivery Roadmap

## 17.1 Conformance profiles

Profiles define supported semantics, not vague levels of sophistication.

### Core

Requires:

- source parsing and validation;
- explicit interfaces and dependencies;
- immutable artefact versions;
- static iteration and collections;
- `Model`, `Function` and static `Module` invocation;
- collision detection;
- bounded execution;
- durable trace and run manifest;
- rejection of unsupported required features;
- basic permission enforcement.

### Evaluation

Adds:

- versioned contracts;
- structured evaluation records;
- acceptance gates;
- candidate handling;
- final-output release policy.

### Human Workflow

Adds:

- durable review tasks;
- authorised decisions;
- approval expiry;
- exact-version approval binding;
- safe resumption.

### Effectful Integration

Adds:

- external reads and writes;
- idempotency declarations;
- action approval;
- receipts;
- uncertain-outcome reconciliation.

### Adaptive Execution

Adds:

- runtime map expansion;
- conditionals;
- bounded loops;
- generated Module proposals;
- validation and permission checks before expansion.

An engine publishes its supported profiles and passes the corresponding conformance tests.

## 17.2 Required conformance cases

The initial suite should include:

| Case | Required result |
|---|---|
| Missing input | Reject or block before consuming operation runs |
| Duplicate output producer | Compilation failure |
| Iterator mentioned only in prose | No implicit repetition |
| Product expansion above limit | Reject before dispatch |
| Strict collection missing a member | Block consumer |
| Final revision fails its contract | No successful release |
| Validator returns inconclusive | Block required gate |
| Approval refers to old digest | Reject approval for new version |
| Unsupported required extension | Reject Module |
| Generated Module requests extra capability | Deny escalation |
| External write times out | Reconcile rather than blind retry |
| Restart after committed output | Reuse committed state without duplication |

## 17.3 Delivery sequence

### Milestone 1 — Semantic kernel

Deliver:

- program model;
- minimal intermediate representation;
- identifier and version rules;
- explicit scheduling and iteration rules;
- basic security model;
- draft schemas and semantic validator.

**Exit criterion:** small Modules compile predictably, and invalid cases fail with useful diagnostics.

### Milestone 2 — Reference vertical slice

Deliver:

- local CLI engine;
- one model adapter;
- a small function library;
- immutable artefact storage;
- trace and run manifest;
- source-grounded summary workflow;
- deterministic test doubles.

**Exit criterion:** the complete workflow can run, fail, resume and export traceable results.

### Milestone 3 — Acceptance and evaluation

Deliver:

- contract execution;
- candidate and acceptance records;
- bounded repair;
- human review;
- evaluator test fixtures;
- baseline comparison harness.

**Exit criterion:** known defective outputs are blocked, and quality claims are supported by measured results.

### Milestone 4 — Reliable integrations

Deliver:

- effect-aware adapters;
- idempotency and reconciliation;
- incremental reruns;
- cache policy;
- package locking;
- privacy and retention controls.

**Exit criterion:** restarts and provider failures do not silently corrupt state or repeat consequential actions.

### Milestone 5 — Adaptive execution

Deliver:

- runtime expansion;
- conditional execution;
- bounded loops;
- generated Module validation;
- inherited capability and budget enforcement.

**Exit criterion:** dynamic behaviour remains inspectable and cannot bypass declared limits.

### Milestone 6 — Ecosystem and production tooling

Deliver:

- graph inspection;
- authoring assistance;
- package registry;
- qualification dashboards;
- cross-engine conformance tests;
- domain-specific libraries.

**Exit criterion:** third parties can author, test and exchange Modules without depending on undocumented engine behaviour.

## 17.4 Deliberate deferrals

The initial implementation should defer:

- unrestricted autonomous planning;
- arbitrary self-modification;
- a public marketplace;
- distributed scheduling at large scale;
- multiple new authoring languages;
- and elaborate visual editors.

A small, coherent, testable engine is more valuable than a broad feature list with ambiguous semantics.

---

# 18. Commercial Strategy

## 18.1 Sell outcomes with evidence

The framework’s commercial value is not the existence of an XML workflow.

It is the ability to produce useful outputs with lower total effort, controlled risk and inspectable evidence.

Promising initial applications include:

- technical documentation from project material;
- legacy project recovery;
- structured game-content production;
- evidence preparation;
- recurring internal reporting.

## 18.2 Choose a narrow initial market

An initial product should focus on one recurring, bounded workflow with:

- accessible representative inputs;
- measurable acceptance criteria;
- an identifiable buyer;
- meaningful human review cost;
- and limited external side effects.

Documentation and project recovery are plausible starting points because deliverables can be inspected before they affect production systems.

## 18.3 Measure total cost

Commercial evaluation should include:

```text
Model and tool cost
+ infrastructure cost
+ human review time
+ correction and rework
+ maintenance
+ integration and governance overhead
```

A workflow that generates quickly but requires extensive correction may be less valuable than a slower, simpler alternative.

## 18.4 Product forms

Possible offerings include:

- a vertical application for a specific deliverable;
- a private enterprise runtime;
- managed workflow execution;
- authoring and evaluation tools;
- domain Modules and validators;
- governance and audit exports;
- implementation services.

These are options to validate, not evidence of guaranteed demand.

## 18.5 Defensibility

Durable value may accumulate in:

- tested domain workflows;
- high-quality evaluation datasets;
- calibrated validators;
- trusted integrations;
- proprietary process knowledge;
- operational reliability;
- and customer-specific acceptance standards.

A Module library becomes valuable through evidence and maintenance, not merely through its size.

## 18.6 Responsible positioning

The strongest positioning is:

> Automate repeatable production work, preserve the evidence needed to inspect it, and keep consequential authority explicit.

Avoid promises of automatic legal compliance, guaranteed factual correctness or universal model interchangeability.

---

# 19. Glossary

**Acceptance Record**  
A decision that an exact artefact version may be used for a declared purpose under a specified policy.

**Artefact Version**  
An immutable concrete instance of a logical Data Unit.

**Binding**  
The resolution of an abstract operation or capability to a specific implementation and configuration.

**Candidate**  
An artefact version that exists but has not yet satisfied the acceptance requirements for its intended use.

**Capability**  
A scoped permission to access a resource or perform an action.

**Cognitive Engine**  
The compiler and runtime that executes Thought Tree programs and enforces their declared semantics.

**Cognitive Programming**  
The practice of defining maintained software processes over meaning-bearing artefacts using models, software, tools and human judgment.

**Collection**  
An ordered group of artefact references with explicit membership and completeness rules.

**Concept**  
A meaningful entity in the application domain, represented through content, types and relationships.

**Context Manifest**  
A record of the information and transformations used to construct a model request.

**Contract**  
A versioned set of criteria used to evaluate artefacts or relationships.

**Control Dependency**  
An explicit ordering requirement that does not necessarily carry data.

**Data Unit**  
A logical artefact supplied, produced or referenced by a program.

**Effect Classification**  
A declaration of whether an operation is pure, observes external state, changes external state or interacts with a human.

**Evaluation Record**  
Structured results and evidence from applying a check to exact artefact versions.

**Execution Plan**  
The resolved graph, bindings, gates, limits and expansion boundaries for a run.

**Export**  
A public Module output, optionally subject to acceptance or approval gates.

**Gate**  
A control point that applies acceptance policy before downstream use or release.

**Human Review**  
A durable request for an authorised person to assess, approve, reject or edit specified artefact versions.

**Idempotency Key**  
An identifier used by an external service or adapter to recognise repeated requests for the same intended action.

**Intermediate Representation**  
The syntax-independent representation used for compilation and execution planning.

**Iterator**  
An explicitly declared repetition domain.

**Module**  
A reusable bounded program with declared inputs, outputs, operations, contracts and capability requirements.

**Operation**  
A declared transformation with named inputs and outputs.

**Operation Instance**  
A concrete invocation after parameter binding and iteration expansion.

**Provenance**  
Recorded relationships identifying how an artefact version was supplied or produced.

**Repair**  
A transformation that uses a candidate and findings to produce a new candidate.

**Replay**  
Execution using previously recorded responses or observations rather than repeating the original external calls.

**Retry**  
Another attempt at an invocation after an execution failure.

**Run**  
One execution of a bound Thought Tree program.

**Semantic Type**  
The intended conceptual role of an artefact, distinct from its media representation.

**Trace**  
The durable event record of execution, subject to access and retention policy.

**TTML**  
Thought Tree Markup Language, the proposed XML representation of the Thought Tree program model.

**Workspace**  
The controlled environment that stores or references inputs, artefacts, evaluations, approvals and execution records.

---

# Closing Statement

The Thought Tree Framework treats LLM-assisted work as a maintained production process rather than a sequence of disposable prompts.

Its foundation is deliberately small:

```text
Explicit interfaces
        +
Identifiable artefacts
        +
Declared transformations
        +
Evidence-based acceptance
        +
Bounded authority
        +
Traceable execution
```

From that foundation, larger workflows can be composed, inspected, tested and improved.

The framework succeeds when it makes failures easier to detect, outputs easier to review, processes easier to reuse and improvements easier to demonstrate. Its value is not that it makes uncertain computation certain, but that it makes the uncertainty—and the decisions made around it—manageable.