# AUTHORING_GUIDE.md

# Authoring Thought Tree Modules

This guide explains how to design and write Thought Tree Modules.

A Thought Tree Module is a reusable cognitive program: a structured process that transforms input Data Units into output Data Units through explicit, inspectable Operations.

At the simplest level, a Thought Tree workflow follows the pattern:

```text
Data Units → Operations → Data Units
```

A good module does not merely ask an LLM to produce an answer. It defines the process by which an answer should be produced, reviewed, revised, validated and traced.

Thought Tree authoring is therefore closer to workflow design or programming than ordinary prompt writing.

> Status note: TTML and the Cognitive Engine are still draft/prototype work. This guide describes the intended authoring model and should evolve alongside the specification.

---

## 1. What You Are Authoring

When you author a Thought Tree Module, you are defining:

- what inputs are required;
- what intermediate artefacts should be produced;
- what operations transform one artefact into another;
- what outputs should be reviewed or validated;
- what final outputs should be returned;
- and what execution trace should be preserved.

A module should be understandable before it runs, inspectable while it runs, and reviewable after it completes.

---

## 2. Start With the Intended Transformation

Before writing TTML or any other source format, describe the transformation in plain language.

Use this pattern:

```text
Given [input Data Units],
produce [output Data Units],
by performing [major transformations],
subject to [constraints, contracts or review criteria].
```

Example:

```text
Given a collection of video game design notes,
produce a complete Technical Design Document,
by creating a source digest, extracting requirements, designing a system decomposition,
drafting document sections, reviewing the assembled draft,
and revising the final document,
while preserving source-supported details and clearly marking assumptions.
```

This statement should guide the module design.

If the transformation cannot be stated clearly, the module is probably not ready to author.

---

## 3. Identify Inputs and Final Outputs First

Start by defining the boundaries of the module.

### Inputs

Inputs are the Data Units the module needs before execution begins.

Example:

```xml
<Inputs>
  <File id="video_game_design_notes" folder="/inputs" extension="txt"/>
</Inputs>
```

Use meaningful input names.

Prefer:

```text
story_requirements
technical_design_notes
policy_documents
customer_feedback_export
source_article
```

Avoid vague names:

```text
input1
document
file
data
```

The identifier should tell a human, an LLM and a Cognitive Engine what role the Data Unit plays.

### Final outputs

Final outputs are the Data Units the module promises to return.

Example:

```xml
<Output>
  <FileRef id="video_game_technical_design_document"/>
</Output>
```

Only declare artefacts as root-level outputs if they are intended to be part of the module’s public result.

Intermediate artefacts should still be preserved by the engine, but they do not all need to be final outputs.

---

## 4. Design Useful Intermediate Artefacts

Intermediate artefacts are one of the main reasons to use Thought Tree rather than a single prompt.

Poor design:

```text
notes → final_document
```

Better design:

```text
notes
→ source_digest
→ requirements_register
→ system_decomposition
→ drafted_sections
→ assembled_draft
→ review_and_correction_plan
→ final_document
```

Good intermediate artefacts are:

- meaningful;
- inspectable;
- reusable;
- useful to downstream operations;
- named clearly;
- small enough to review, validate or replace.

Examples:

```text
source_digest
requirements_register
game_intent_and_scope
system_decomposition
draft_tdd_sections
draft_tdd
review_and_correction_plan
technical_design_document
```

A good intermediate artefact acts like a stable stepping stone in the workflow.

---

## 5. Decompose the Task Into Operations

An Operation transforms one or more input Data Units into one or more output Data Units.

A good Operation usually does one main thing:

```text
CreateSourceDigest
ExtractRequirements
DraftPlotOutline
ReviewDraft
ReviseFinalDocument
ConcatenateFiles
ValidateTTML
```

Avoid operations that combine too many cognitive steps at once.

Poor:

```xml
<Operation
  id="MakeWholeProject"
  type="TextCompletion"
  desc="Read the notes, understand everything, create all documents, review them, fix them, and output the final result.">
```

Better:

```xml
<Operation
  id="CreateSourceDigest"
  type="TextCompletion"
  desc="Create a structured digest of the source notes, preserving important facts, constraints, assumptions and open questions.">
```

```xml
<Operation
  id="ExtractRequirements"
  type="TextCompletion"
  desc="Extract a technical requirements register from the source digest. Distinguish explicit requirements, inferred requirements and unresolved questions.">
```

```xml
<Operation
  id="ReviewDraftDocument"
  type="TextCompletion"
  desc="Review the assembled draft for completeness, consistency, unsupported claims, missing dependencies and unclear requirements. Produce a correction plan.">
```

The goal is not to create as many operations as possible. The goal is to make the process understandable, inspectable and improvable.

---

## 6. Choose the Right Operation Type

Thought Tree is a hybrid framework. Not every step should be performed by an LLM.

### `TextCompletion`

Use `TextCompletion` for semantic or cognitive work.

Good uses include:

- summarisation;
- analysis;
- drafting;
- synthesis;
- review;
- revision;
- classification;
- extraction from unstructured text;
- creative generation;
- planning.

Example:

```xml
<Operation
  id="AnalyzeStoryRequirements"
  type="TextCompletion"
  desc="Analyze the story requirements and extract the core premise, constraints, genre expectations, target audience and success criteria.">
  <FileRef id="story_requirements"/>
  <Output>
    <File id="story_requirements_analysis" extension="txt"/>
  </Output>
</Operation>
```

### `ExecuteFunction`

Use `ExecuteFunction` for deterministic work that ordinary software can perform more reliably than an LLM.

Good uses include:

- copying files;
- concatenating files;
- converting formats;
- validating XML;
- querying a database;
- resizing images;
- creating archives;
- generating checksums.

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

Do not ask an LLM to concatenate files, validate XML or copy assets if code can do it safely and deterministically.

### `PreExisting`

Use `PreExisting` when the operation should call another Thought Tree Module.

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

Use this when a process is reusable or complex enough to deserve its own module.

### `ProjectCompletion`

Use `ProjectCompletion` when a high-level task should be expanded into a generated submodule.

This is useful but should be treated carefully. Generated submodules should be preserved, validated and, where appropriate, reviewed before execution.

### `DynamicCompletion`

Use `DynamicCompletion` when the engine must decide how best to complete an operation.

This introduces ambiguity. Use it sparingly in workflows where predictability, auditability or cost control matter.

A good dynamic execution pattern is:

```text
Ambiguous Operation
↓
Generate proposed plan or submodule
↓
Validate proposed plan
↓
Optional human approval
↓
Execute approved plan
↓
Record generated plan in trace
```

---

## 7. Write Good Operation Descriptions

The `desc` attribute is not just a label. It is the semantic instruction for the operation.

A good description should specify:

- the action to perform;
- the intended output;
- relevant constraints;
- quality expectations;
- source-grounding expectations;
- what not to invent;
- how the output will be used downstream.

Poor:

```xml
desc="Make it better."
```

Better:

```xml
desc="Review the draft Technical Design Document for completeness, internal consistency, unsupported claims, duplicated content, missing dependencies, weak assumptions and unclear requirements. Produce a correction plan with concrete edits."
```

Poor:

```xml
desc="Create characters."
```

Better:

```xml
desc="Generate a distinct and detailed profile for the indexed major character using the unified character system. Include role, motivation, conflict, personality, background, relationships, arc, voice, vulnerabilities, strengths and narrative function."
```

Operation descriptions should be specific enough to guide execution, but not so over-specified that they prevent adaptation to the actual input.

---

## 8. Declare Dependencies Explicitly

Each operation should reference the Data Units it needs.

Example:

```xml
<Operation
  id="DraftPlotOutline"
  type="TextCompletion"
  desc="Draft a coherent plot outline using the story requirements analysis, character system brief and setting overview draft.">
  <FileRef id="story_requirements_analysis"/>
  <FileRef id="character_system_brief"/>
  <FileRef id="draft_setting_overview"/>
  <Output>
    <File id="draft_plot_outline" extension="txt"/>
  </Output>
</Operation>
```

Avoid hidden assumptions.

If an operation needs an artefact, declare it with a `FileRef` or `CollectionRef`.

Explicit dependencies improve:

- validation;
- traceability;
- debugging;
- reproducibility;
- execution planning;
- safe parallelisation.

A Cognitive Engine should be able to determine why each operation runs and which artefacts it depends on.

---

## 9. Use Collections for Groups of Related Data Units

A Collection is a named group of Data Units.

Use Collections when an operation should consume a group as one logical input.

Example:

```xml
<Collection id="all_revised_chapters" orderBy="ChapterIterator">
  <FileRef id="revised_chapter_{{ChapterIterator}}"/>
</Collection>
```

Then pass the collection into an operation:

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

Collections are useful for:

- aggregating iterator outputs;
- passing many files into one review operation;
- compiling sections;
- grouping feedback;
- avoiding output collisions;
- making aggregation explicit.

A `CollectionRef` should not automatically cause an operation to run once per member. It should usually pass the collection as grouped context.

---

## 10. Use Iterators Carefully

Iterators allow one operation definition to expand into many concrete operation instances.

Example:

```xml
<Iterator id="ChapterIterator" from="1" to="{{ChapterCount}}"/>
```

Used in an output:

```xml
<File id="draft_chapter_{{ChapterIterator}}" extension="txt"/>
```

This may produce:

```text
draft_chapter_1
draft_chapter_2
draft_chapter_3
...
```

Before using iterators, ask:

- Which iterators are active for this operation?
- How many operation instances will be created?
- Will every output identifier be unique?
- Should the operation run once per item, or once over a collection?
- Is this creating a Cartesian product?
- Are outputs at risk of collision?

If an operation references multiple active iterators, the default behaviour may be Cartesian expansion.

Example:

```xml
<FileRef id="draft_chapter_{{ChapterIterator}}"/>
<FileRef id="beta_reader_persona_{{PersonaIterator}}"/>
<Output>
  <File id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}" extension="txt"/>
</Output>
```

If there are 12 chapters and 3 personas, this produces 36 feedback files.

That may be correct. But if the output omits one active iterator, the module may create collisions.

---

## 11. Avoid Silent Output Collisions

An output collision happens when multiple operation instances attempt to write the same logical output identifier.

Poor:

```xml
<Operation
  id="ReviseChapter"
  type="TextCompletion"
  desc="Revise each chapter using persona feedback.">
  <FileRef id="draft_chapter_{{ChapterIterator}}"/>
  <FileRef id="feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}"/>
  <Output>
    <File id="revised_chapter_{{ChapterIterator}}" extension="txt"/>
  </Output>
</Operation>
```

Here, `PersonaIterator` is active, but the output does not include it. Multiple persona-specific executions may try to write:

```text
revised_chapter_1
```

Valid solutions include:

1. include all active iterators in the output identifier;
2. aggregate the extra iterator dimension into a Collection;
3. add a separate aggregation operation;
4. create versioned outputs;
5. explicitly declare overwrite behaviour, if supported;
6. require human or automated review.

Preferred pattern:

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

Silent overwriting should be invalid by default.

---

## 12. Design for Review and Revision

High-quality cognitive workflows should usually include review and revision stages.

Common pattern:

```text
Draft → Review → Revise → Final
```

Example:

```xml
<Operation
  id="DraftDocument"
  type="TextCompletion"
  desc="Draft the document from the source digest and requirements register.">
  <FileRef id="source_digest"/>
  <FileRef id="requirements_register"/>
  <Output>
    <File id="draft_document" extension="md"/>
  </Output>
</Operation>
```

```xml
<Operation
  id="ReviewDraftDocument"
  type="TextCompletion"
  desc="Review the draft document for completeness, consistency, unsupported claims, missing requirements and unclear structure. Produce a correction plan.">
  <FileRef id="draft_document"/>
  <FileRef id="source_digest"/>
  <FileRef id="requirements_register"/>
  <Output>
    <File id="draft_review_correction_plan" extension="txt"/>
  </Output>
</Operation>
```

```xml
<Operation
  id="FinalizeDocument"
  type="TextCompletion"
  desc="Revise the draft document using the correction plan. Produce the final polished document only.">
  <FileRef id="draft_document"/>
  <FileRef id="draft_review_correction_plan"/>
  <Output>
    <File id="final_document" extension="md"/>
  </Output>
</Operation>
```

Review stages are especially important when:

- outputs are large;
- the workflow is analytical;
- the workflow is creative;
- source fidelity matters;
- unsupported claims would be harmful;
- outputs will be used commercially;
- compliance, safety or governance matters.

---

## 13. Design for Semantic Contracts

A module should not only produce files. It should produce artefacts that satisfy their intended purpose.

A future or advanced Cognitive Engine may support explicit semantic types and contracts.

Examples:

```text
plot_outline
semantic type: PlotOutline
contract: includes premise, act structure, major turning points, climax and resolution
```

```text
technical_requirements_register
semantic type: RequirementsRegister
contract: each requirement is marked explicit, inferred or unresolved
```

```text
policy_gap_analysis
semantic type: ComplianceGapAnalysis
contract: each gap references a policy obligation and supporting evidence
```

Even if the current TTML schema does not fully encode these contracts, authors should design operations as if these expectations exist.

For example:

```xml
desc="Extract an implementation-focused technical requirements register. Include functional requirements, non-functional requirements, platform requirements, assumptions, dependencies, risks and open questions. Mark every item as explicit, inferred or unresolved."
```

This makes the semantic expectations clear.

---

## 14. Design for Reuse

A good module should be reusable across similar projects.

To improve reusability:

- avoid hard-coding unnecessary project-specific details;
- use generic input names where appropriate;
- separate domain-specific knowledge into input Data Units;
- use variables for counts, limits and configuration;
- use submodules for reusable processes;
- keep final outputs well-defined;
- make assumptions explicit.

Better:

```text
Input: video_game_design_notes
Output: video_game_technical_design_document
```

Less reusable:

```text
Input: sci_fi_racing_game_notes_for_project_nebula
Output: nebula_tdd
```

Project-specific details can still exist in the input content. The module structure should remain reusable where possible.

---

## 15. Design for Model Independence

Thought Tree Modules should describe what needs to be done, not which specific LLM provider must do it.

Avoid:

```text
Use GPT-4 to write this section.
```

Prefer:

```text
Draft this section using a high-quality long-context text completion model.
```

The Cognitive Engine should decide which model to use based on:

- availability;
- cost;
- privacy requirements;
- policy;
- capability;
- local configuration.

A module may eventually specify capability requirements, such as:

```text
requires long-context reasoning
requires structured output
requires high creative quality
requires low-temperature factual extraction
```

But the module should not normally depend on a specific vendor.

---

## 16. Design for Traceability

A module should make it possible to understand where each output came from.

To improve traceability:

- name operations clearly;
- name intermediate artefacts clearly;
- declare dependencies explicitly;
- preserve review outputs;
- preserve validation reports;
- preserve generated submodules;
- avoid silent overwrites;
- declare final outputs explicitly.

A reviewer should be able to answer:

- What produced this Data Unit?
- Which inputs were used?
- Which operation ran?
- Which model, function or tool was used?
- Was the output reviewed?
- Was the output revised?
- Was the output validated?
- Which final output depended on it?

Traceability is one of the main differences between a Thought Tree process and a single large prompt.

---

## 17. Design for Failure

Assume some operations may fail or produce weak outputs.

Common failure points include:

- missing inputs;
- invalid references;
- malformed LLM output;
- incomplete output;
- unsupported claims;
- output collisions;
- missing functions;
- invalid generated TTML;
- failed validation;
- excessive context length;
- ambiguous instructions.

A robust module may include:

- validation operations;
- review operations;
- correction plans;
- retry-compatible outputs;
- human review points where appropriate;
- clear intermediate artefacts that allow partial recovery.

For high-impact workflows, avoid designs where a single failed operation invalidates the whole process without producing diagnostic artefacts.

---

## 18. Recommended Authoring Process

Use this process when creating a new module.

### Step 1: Define the module purpose

Write one paragraph describing the intended transformation.

### Step 2: Declare inputs and outputs

Identify the required initial Data Units and final deliverables.

### Step 3: Identify intermediate artefacts

Decide which intermediate outputs will make the process inspectable and reliable.

### Step 4: Decompose into operations

Break the workflow into ordered transformations.

### Step 5: Choose operation types

Use:

- LLM operations for semantic work;
- deterministic functions for mechanical work;
- submodules for reusable work;
- dynamic generation only when necessary.

### Step 6: Define dependencies

Add `FileRef` and `CollectionRef` references so each operation has the context it needs.

### Step 7: Add iterators and collections

Use iterators for repeated structures and Collections for aggregation.

### Step 8: Add review and validation

Insert review, correction and validation stages where quality matters.

### Step 9: Check execution semantics

Validate that:

- all `FileRef`s can be resolved;
- all `CollectionRef`s can be resolved;
- all functions exist;
- all submodules are available;
- iterator expansion is correct;
- outputs do not collide;
- final outputs are actually produced.

### Step 10: Test and improve

Run the module on representative inputs, inspect outputs and revise the module.

---

## 19. Good Module Checklist

A Thought Tree Module is well-authored when:

- [ ] the module purpose is clear;
- [ ] inputs are explicit;
- [ ] final outputs are explicit;
- [ ] intermediate artefacts are meaningful;
- [ ] each operation has a clear purpose;
- [ ] operation descriptions are specific enough to guide execution;
- [ ] LLMs are used for semantic work;
- [ ] deterministic functions are used for mechanical work;
- [ ] dependencies are declared with `FileRef` or `CollectionRef`;
- [ ] Collections are used for grouped inputs;
- [ ] iterators do not create unintended output collisions;
- [ ] review stages exist where quality matters;
- [ ] validation expectations are clear;
- [ ] generated submodules are retained where applicable;
- [ ] output names are consistent;
- [ ] the workflow is reusable;
- [ ] the workflow is inspectable;
- [ ] the workflow can be executed by a conformant Cognitive Engine;
- [ ] the execution trace will be understandable after the run.

---

## 20. Common Anti-Patterns

Avoid these patterns where possible.

### One giant prompt

```text
input → MakeEverything → final_output
```

This hides the process and makes failures hard to diagnose.

### Vague operation descriptions

```xml
desc="Improve this."
```

The operation should specify what improvement means.

### Hidden dependencies

If an operation needs a Data Unit, reference it explicitly.

### Using LLMs for deterministic work

Do not ask an LLM to concatenate files, validate XML or copy assets.

### Unclear output names

Avoid names like:

```text
result
output2
new_file
thing
```

Prefer names that describe semantic purpose.

### Missing review stages

High-impact outputs should usually be reviewed before finalisation.

### Iterator collisions

If multiple iterators are active, ensure output identifiers are unique or aggregate inputs into Collections.

### Silent overwrites

A module should not rely on overwriting outputs unless that behaviour is explicit and traceable.

### Overusing `DynamicCompletion`

Dynamic planning is powerful, but explicit structure is easier to inspect, validate and audit.

---

## 21. Minimal TTML Example

The following simplified module takes an article, drafts a summary, reviews it and produces a final revised summary.

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
    desc="Summarise an article through a draft, review and revision process."/>

  <Inputs>
    <File id="source_article" folder="/inputs" extension="txt"/>
  </Inputs>

  <Vars/>

  <Iterators/>

  <Operations>

    <Operation
      id="DraftSummary"
      type="TextCompletion"
      desc="Create a concise structured summary of the source article. Include main argument, key evidence, important caveats and unresolved questions. Do not introduce unsupported claims.">
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

This illustrates the basic authoring pattern:

```text
Input → Draft → Review → Revise → Final Output
```

Even for a simple task, the module preserves intermediate artefacts and makes the process inspectable.

---

## 22. Larger Authoring Pattern

For larger workflows, use a more explicit production pipeline.

Example: technical design document generation.

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
draft_tdd_sections
↓
AssembleDraft
↓
draft_tdd
↓
ReviewDraft
↓
review_and_correction_plan
↓
ReviseFinalDocument
↓
technical_design_document
```

This style makes each major cognitive transformation visible.

---

## 23. Summary

Authoring Thought Tree Modules is the practice of designing executable cognitive processes.

A well-authored module should:

- express a clear transformation;
- decompose complex work into inspectable operations;
- preserve useful intermediate artefacts;
- use LLMs, functions, tools and submodules appropriately;
- declare dependencies explicitly;
- support validation and review;
- avoid hidden state and silent overwrites;
- produce traceable final outputs;
- be reusable across similar tasks.

The purpose of Thought Tree authoring is not merely to automate prompting.

It is to create cognitive programs: structured, inspectable and improvable processes for transforming concepts and information into validated outputs.