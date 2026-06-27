
# Semantic Contracts

**Status:** Draft / handoff document  
**Scope:** Thought Tree Framework semantic validation model  
**Related documents:** `PROGRAM_MODEL.md`, `COGNITIVE_ENGINE.md`, `EXECUTION_SEMANTICS.md`, `TTML_DRAFT.md`, `CONTRACT_SCHEMA_DRAFT.json`

Semantic Contracts define what Thought Tree outputs are supposed to **mean**, not just what files are supposed to exist.

A Thought Tree workflow should not be considered successful merely because it produced a file with the expected identifier. In cognitive workflows, the more important question is whether the produced artefact is meaningful, complete, usable and fit for its downstream purpose.

For example, an operation may successfully create:

```text
plot_outline.txt
```

but that file may still fail as a plot outline if it:

- omits the climax;
- contradicts character profiles;
- ignores the story requirements;
- lacks a coherent narrative arc;
- is too vague to support chapter drafting.

At the file-system level, the output exists.  
At the cognitive-program level, the output may have failed.

Semantic Contracts address this problem.

---

## 1. What Is a Semantic Contract?

A **Semantic Contract** defines the expected meaning, structure, quality and usage requirements of a Data Unit or Operation output.

A contract may specify:

- expected semantic type;
- expected physical format;
- required sections;
- required fields;
- required relationships;
- completeness criteria;
- consistency criteria;
- source-grounding requirements;
- formatting rules;
- quality criteria;
- prohibited content;
- downstream usage expectations;
- deterministic validation functions;
- LLM-assisted review rubrics;
- human approval requirements;
- failure handling behaviour.

In short:

```text
A Semantic Contract defines what must be true for an artefact to be accepted as valid.
```

---

## 2. Why Semantic Contracts Matter

LLM-assisted workflows can produce fluent but weak outputs.

A generated artefact may:

- look plausible;
- be written well;
- have the expected filename;
- use the expected format;

while still being:

- incomplete;
- unsupported by source material;
- internally inconsistent;
- too vague for downstream use;
- structurally malformed;
- unsuitable for review or publication.

Semantic Contracts allow a Cognitive Engine to distinguish between:

```text
An output exists.
An output has the correct format.
An output satisfies the cognitive requirements of the workflow.
```

This is central to the Thought Tree Framework because Thought Tree is not only about producing outputs. It is about producing outputs through an inspectable, validatable and improvable process.

---

## 3. Three Levels of Correctness

Thought Tree validation should distinguish between three levels of correctness.

---

### 3.1 Schema Correctness

Schema correctness means the source program is structurally valid.

For TTML, this means the XML conforms to the TTML schema.

Schema correctness answers:

```text
Is this a valid Thought Tree program document?
```

Example checks:

- Is the XML well-formed?
- Does the document contain a valid root `<TTML>` element?
- Are required sections present?
- Are operation types valid?
- Are required attributes present?
- Are variables and iterators structurally valid?

Schema correctness is necessary, but not sufficient.

A TTML file may be schema-valid while still containing unresolved dependencies, missing functions, output collisions or poorly designed operations.

---

### 3.2 Execution Correctness

Execution correctness means the Cognitive Engine can run the program.

Execution correctness answers:

```text
Can this Thought Tree program actually execute?
```

Example checks:

- Are required input Data Units available?
- Can every `FileRef` be resolved?
- Can every `CollectionRef` be resolved?
- Do referenced variables exist?
- Are iterator ranges valid?
- Does iterator expansion create output collisions?
- Are required deterministic functions available?
- Are referenced submodules available?
- Can final outputs be produced?
- Does the engine have access to required models, tools and permissions?

Execution correctness confirms that the workflow can produce the declared artefacts.

However, it still does not guarantee that those artefacts are good enough.

---

### 3.3 Semantic Correctness

Semantic correctness means produced Data Units satisfy their intended cognitive purpose.

Semantic correctness answers:

```text
Is this output meaningful, complete and fit for use in the workflow?
```

Example checks:

- Does the output satisfy its Semantic Contract?
- Does it contain required sections?
- Is it consistent with its inputs?
- Does it avoid unsupported claims?
- Is it detailed enough for downstream operations?
- Does it preserve necessary constraints?
- Does it follow the required format?
- Does it meet quality criteria?
- Has it passed required review or approval gates?

Semantic correctness prevents a workflow from silently passing weak, incomplete or misleading artefacts into later stages.

---

## 4. Semantic Types

A **Semantic Type** describes what kind of cognitive artefact a Data Unit represents.

A Semantic Type is different from a file extension.

For example:

```text
character_1_profile.txt
```

has:

```text
Physical format: txt
Semantic type: CharacterProfile
```

Examples of Semantic Types include:

- `SourceDigest`
- `Summary`
- `PlotOutline`
- `CharacterProfile`
- `ChapterDraft`
- `ContinuityReview`
- `TechnicalRequirementsRegister`
- `RiskRegister`
- `ComplianceGapAssessment`
- `EvidenceMap`
- `GameMechanicSpecification`
- `PolicySummary`
- `TestCaseSpecification`
- `TechnicalDesignDocument`
- `TTMLModule`
- `ValidationReport`
- `ReviewReport`
- `CorrectionPlan`

Semantic Types allow Cognitive Engines, authoring tools and review modules to reason about what a Data Unit is supposed to be, rather than treating it as an anonymous file.

Example future TTML-style syntax:

```xml
<File
  id="plot_outline"
  extension="md"
  semanticType="PlotOutline"
  contract="ThreeActPlotOutlineContract"/>
```

---

## 5. Data Contracts

A **Data Contract** defines the expected properties of a Data Unit.

A Data Contract may specify:

- semantic type;
- physical format;
- required sections;
- required fields;
- required relationships;
- formatting requirements;
- completeness criteria;
- consistency criteria;
- source traceability requirements;
- quality criteria;
- validation functions;
- LLM-assisted review rubrics;
- human approval requirements;
- downstream usage expectations.

A Data Contract may be strict, loose or advisory depending on the workflow.

Structured JSON outputs may require strict machine validation.  
Creative or analytical outputs may require LLM-assisted or human review.

---

## 6. Example: Plot Outline Contract

A Plot Outline contract might require:

```yaml
id: PlotOutlineContract
semantic_type: PlotOutline
format: markdown

required_sections:
  - premise
  - genre
  - target_audience
  - main_characters
  - central_conflict
  - act_1
  - act_2
  - act_3
  - climax
  - resolution
  - unresolved_questions

quality_criteria:
  - The outline must be consistent with the story requirements.
  - The protagonist's motivation must be clear.
  - The central conflict must escalate across the story.
  - The climax must resolve the primary conflict.
  - The outline must provide enough detail to support chapter drafting.

validation:
  machine:
    - ValidateMarkdownSections
  llm_review:
    - ReviewNarrativeCompleteness
    - ReviewContinuityAgainstRequirements
  human_review:
    required: false
```

This contract tells the Cognitive Engine that producing a Markdown file is not enough. The output must contain the expected structure and satisfy narrative requirements.

---

## 7. Example: Technical Requirements Register Contract

A Technical Requirements Register contract may require stronger traceability:

```yaml
id: TechnicalRequirementsRegisterContract
semantic_type: TechnicalRequirementsRegister
format: markdown

required_sections:
  - functional_requirements
  - non_functional_requirements
  - platform_requirements
  - data_requirements
  - integration_requirements
  - assumptions
  - dependencies
  - risks
  - open_questions

required_fields_per_requirement:
  - id
  - description
  - source_basis
  - status
  - priority
  - verification_method

allowed_status_values:
  - explicit
  - inferred
  - unresolved

quality_criteria:
  - Requirements must not invent unsupported technical claims.
  - Inferred requirements must be labelled as inferred.
  - Unresolved points must be captured as open questions.
  - Requirements should be specific enough to support implementation planning.

validation:
  machine:
    - ValidateRequirementTableStructure
  llm_review:
    - ReviewForUnsupportedClaims
    - ReviewRequirementsCompleteness
  human_review:
    required: true
    reason: High-impact technical planning artefact.
```

This contract supports both machine validation and judgement-based review.

---

## 8. Operation Contracts

Semantic Contracts may apply not only to Data Units, but also to Operations.

An **Operation Contract** defines what must be true before and after an operation executes.

It may include:

- preconditions;
- expected inputs;
- expected outputs;
- required semantic types;
- postconditions;
- validation gates;
- failure handling;
- retry behaviour;
- review requirements.

Example:

```yaml
operation: ReviseChapter

preconditions:
  - draft_chapter exists
  - feedback_for_chapter is non_empty
  - plot_outline satisfies PlotOutlineContract

inputs:
  draft_chapter:
    semantic_type: ChapterDraft
  feedback_for_chapter:
    semantic_type: FeedbackCollection

outputs:
  revised_chapter:
    semantic_type: ChapterDraft
    contract: RevisedChapterContract

postconditions:
  - revised_chapter incorporates relevant feedback
  - revised_chapter remains consistent with plot_outline
  - revised_chapter does not introduce unresolved continuity errors

on_contract_failure:
  - retry_with_repair_prompt
  - generate_validation_report
  - request_human_review
```

This makes the operation more like a typed function in a conventional programming language, while still allowing for the ambiguity of cognitive work.

---

## 9. Validation Gates

A **Validation Gate** is a point in the workflow where an output must be checked before downstream execution continues.

An operation should be considered complete only when:

1. the declared output exists;
2. the output is accessible in the workspace;
3. required format checks pass;
4. the output satisfies its Semantic Contract, where one is declared;
5. required review or approval steps have passed.

If validation fails, the Cognitive Engine may:

- retry the operation;
- ask an LLM to repair the output;
- run a specialised review module;
- generate a diagnostic report;
- request human intervention;
- branch to an error-handling module;
- mark the workflow as failed;
- preserve the failed output for audit purposes.

Validation Gates are especially important before outputs are used by later operations.

For example:

```text
source_notes
↓
CreateSourceDigest
↓
source_digest
↓
ValidateSourceDigest
↓
ExtractRequirements
```

The `source_digest` should not be passed into requirements extraction merely because the file exists. It should first be good enough to support the next transformation.

---

## 10. Validation Methods

Semantic validation can be performed in several ways.

---

### 10.1 Structural Validation

Structural validation checks whether an output has the required form.

Examples:

- Does the Markdown document contain required headings?
- Does the JSON parse correctly?
- Does the XML conform to a schema?
- Does a table contain required columns?
- Does a file use the expected extension?
- Are required sections present?

Structural validation is often deterministic.

---

### 10.2 Content Validation

Content validation checks whether the output contains required information.

Examples:

- Does a character profile include motivation, conflict and arc?
- Does a requirements register include assumptions and open questions?
- Does a review report identify specific issues?
- Does a policy summary include obligations, risks and evidence gaps?

Content validation may require deterministic checks, LLM-assisted review or human judgement.

---

### 10.3 Consistency Validation

Consistency validation checks whether an output agrees with other Data Units.

Examples:

- Does a chapter contradict the plot outline?
- Does a character profile contradict the character system brief?
- Does a technical design document introduce unsupported features?
- Does a compliance report accurately reflect the source policy?
- Does a generated TTML module preserve the intent of the original operation?

Consistency validation is often semantic and may require LLM review or human judgement.

---

### 10.4 Source Grounding Validation

Source grounding validation checks whether claims in an output are supported by the input material.

This is especially important for analytical, compliance, technical and factual workflows.

Examples:

- Requirements should be traceable to design notes.
- Compliance obligations should be traceable to source policy text.
- Risk statements should be derived from supplied evidence.
- Technical assumptions should be labelled as assumptions, not facts.

A Thought Tree workflow may require outputs to distinguish between:

- explicit source-supported claims;
- inferred claims;
- assumptions;
- unresolved questions.

---

### 10.5 Quality Validation

Quality validation checks whether the output is useful, coherent and fit for purpose.

Examples:

- Is the plot outline detailed enough to support drafting?
- Is the technical design document useful to engineers?
- Is the executive summary concise and decision-ready?
- Is the review report actionable?
- Is the generated TTML modular and inspectable?

Quality validation may be performed through:

- LLM review rubrics;
- human review;
- comparison against examples;
- scoring functions;
- test cases;
- downstream performance.

---

### 10.6 Downstream Readiness Validation

Downstream readiness validation checks whether an output is suitable for the operations that depend on it.

Examples:

- A `source_digest` must be detailed enough to support requirements extraction.
- A `character_system_brief` must be coherent enough to support individual character profiles.
- A `technical_requirements_register` must be specific enough to support architecture drafting.
- A `draft_generated_ttml` must be valid enough to pass executor validation.

This form of validation asks:

```text
Can the next step safely use this output?
```

---

## 11. Contract Outcomes

A contract check should produce a clear outcome.

Recommended outcomes include:

---

### Pass

The output satisfies the contract and execution may continue.

---

### Pass With Warnings

The output is usable, but issues were detected.

Warnings should be recorded in the execution trace and may be passed to downstream operations.

---

### Fail And Retry

The output does not satisfy the contract, but the engine may retry or repair the operation automatically.

---

### Fail And Request Review

The output requires human or specialised review before execution continues.

---

### Fail Terminally

The output cannot be accepted and the workflow should stop unless an explicit recovery path exists.

---

## 12. Contract Failure Handling

When a Semantic Contract fails, the Cognitive Engine should not silently continue unless the contract is advisory.

Possible failure handling strategies include:

### Retry the operation

Run the same operation again, possibly with adjusted model settings or clearer instructions.

### Repair the output

Ask an LLM to revise the failed output using the validation report.

### Run a review module

Execute a specialised Thought Tree Module designed to diagnose and fix the output.

### Fallback to deterministic correction

Use a function to fix formatting, structure or schema issues.

### Request human intervention

Pause execution for review, correction or approval.

### Create a diagnostic artefact

Generate a report explaining the failure and recommended next steps.

### Abort execution

Stop the workflow if continuing would compromise downstream outputs.

All contract failures and recovery attempts should be recorded in the execution trace.

---

## 13. Contracts and Execution Trace

Semantic validation should be traceable.

For each contract check, the execution trace should record:

- Data Unit identifier;
- semantic type;
- contract used;
- validation method;
- validation result;
- validation timestamp;
- validation function or review module used;
- LLM model used for review, if applicable;
- reviewer identity, if human review occurred;
- warnings;
- errors;
- repair attempts;
- final validation status.

This allows users to understand not only what was produced, but why the output was accepted.

---

## 14. Contracts and Versioning

When an output fails a contract and is repaired, the failed version should not necessarily be overwritten.

A recommended approach is to preserve versions:

```text
plot_outline.v1.md
plot_outline.v1.validation_report.md

plot_outline.v2.md
plot_outline.v2.validation_report.md
```

or through workspace metadata:

```yaml
data_unit: plot_outline

versions:
  - version: 1
    status: failed_contract
    issues:
      - missing climax
      - weak character motivation

  - version: 2
    status: passed
    repaired_from: 1
```

This supports auditability, comparison and improvement.

---

## 15. Contracts in TTML

The initial TTML schema may treat contracts as optional or implementation-specific. Future versions of TTML should support explicit contract references.

A simple form could be:

```xml
<File
  id="plot_outline"
  extension="md"
  semanticType="PlotOutline"
  contract="PlotOutlineContract"/>
```

An operation could also declare validation behaviour:

```xml
<Operation
  id="GeneratePlotOutline"
  type="TextCompletion"
  desc="Generate a coherent plot outline from the story requirements."
  validation="required">

  <FileRef id="story_requirements"/>

  <Output>
    <File
      id="plot_outline"
      extension="md"
      semanticType="PlotOutline"
      contract="PlotOutlineContract"/>
  </Output>
</Operation>
```

Alternatively, contracts could be declared in a separate section:

```xml
<Contracts>
  <Contract
    id="PlotOutlineContract"
    semanticType="PlotOutline"
    target="plot_outline">

    <RequiredSection name="Premise"/>
    <RequiredSection name="Central Conflict"/>
    <RequiredSection name="Act I"/>
    <RequiredSection name="Act II"/>
    <RequiredSection name="Act III"/>
    <RequiredSection name="Climax"/>
    <RequiredSection name="Resolution"/>

    <Validator function="ValidateMarkdownSections"/>
    <Review rubric="ReviewNarrativeCompleteness"/>
  </Contract>
</Contracts>
```

The exact syntax may evolve, but the conceptual requirement is clear:

```text
Important Data Units should be able to declare what makes them valid.
```

---

## 16. Contract Reuse

Contracts should be reusable across Modules.

For example, an organisation may define standard contracts for:

- `ExecutiveSummary`
- `RequirementsRegister`
- `RiskRegister`
- `ComplianceGapAssessment`
- `TechnicalDesignDocument`
- `CreativeBrief`
- `CharacterProfile`
- `PlotOutline`
- `ReviewReport`
- `TTMLModule`

A shared contract library would support:

- standardisation;
- quality control;
- module interoperability;
- reusable validation;
- organisational governance;
- benchmarking across engines and models.

---

## 17. Contract Strictness

Not all outputs require the same level of validation.

A Thought Tree program may use different contract strictness levels.

---

### Advisory

The contract provides guidance, but failure does not stop execution.

Useful for early ideation or low-impact creative outputs.

---

### Warning

Failure records warnings but allows execution to continue.

Useful when downstream operations can compensate for minor defects.

---

### Required

The contract must pass before downstream execution continues.

Useful for important intermediate artefacts.

---

### Approval Required

The contract must pass and receive human approval.

Useful for high-impact, regulated, commercial or safety-sensitive outputs.

Example:

```yaml
contract_policy:
  source_digest: required
  plot_outline: required
  beta_reader_feedback: advisory
  final_edited_novel: approval_required
  compliance_gap_assessment: approval_required
```

---

## 18. Human Review as Contract Validation

Some contracts require judgement that should not be delegated entirely to an LLM.

Human review may be appropriate when:

- the output has legal, financial, safety or reputational significance;
- the workflow makes strategic business decisions;
- creative quality is subjective and important;
- source material is ambiguous;
- an LLM review produces low confidence;
- a generated module changes execution behaviour;
- the output will be published externally.

Human review should be treated as a validation method, not as an informal external step.

If a human approves, rejects or edits an output, that decision should be recorded in the execution trace.

---

## 19. Contracts and Dynamic Modules

Semantic Contracts are especially important for `DynamicCompletion` and `ProjectCompletion` operations.

When the Cognitive Engine generates a submodule, the generated module should be validated before execution.

Validation should check:

- schema correctness;
- execution correctness;
- contract coverage;
- input/output compatibility;
- unresolved dependencies;
- unsafe tool or function use;
- output collision risks;
- preservation of the parent operation’s intent.

A dynamically generated module should satisfy the parent operation’s declared outputs and contracts.

In other words:

```text
Dynamic planning may change how an operation is executed,
but it should not change what the operation is required to produce.
```

---

## 20. Contracts and Module Improvement

Semantic Contracts also support module improvement.

A Thought Tree Module can be reviewed by comparing:

- its intended task;
- its declared inputs and outputs;
- its operation decomposition;
- its contracts;
- its validation results;
- its execution trace;
- the quality of outputs it produced.

An improvement process may then revise the Module to add:

- clearer intermediate artefacts;
- stronger contracts;
- better review operations;
- deterministic validation functions;
- improved output naming;
- safer dynamic planning;
- more useful final outputs.

This allows Thought Tree programs to improve not only their outputs, but also their own production processes.

---

## 21. Minimal Contract Object Model

A future `CONTRACT_SCHEMA_DRAFT.json` may formalise this, but a useful abstract contract model might include:

```yaml
id: string
description: string
semantic_type: string
target_kind: data_unit | operation | module
target_id: string | null

format:
  physical_format: string | null
  mime_type: string | null

structure:
  required_sections: []
  required_fields: []
  required_relationships: []

criteria:
  completeness: []
  consistency: []
  quality: []
  source_grounding: []
  prohibited_content: []
  downstream_readiness: []

validation:
  strictness: advisory | warning | required | approval_required
  machine_validators: []
  llm_reviews: []
  human_review:
    required: boolean
    reason: string | null

failure_handling:
  on_fail:
    - retry
    - repair
    - review
    - abort
  max_retries: integer | null

trace:
  record_validation_report: boolean
  preserve_failed_versions: boolean
```

This is not a final schema. It is a working model for discussion and implementation.

---

## 22. Minimal Engine Behaviour

A basic Cognitive Engine may ignore semantic contracts if it does not yet support them.

A contract-aware Cognitive Engine should, at minimum:

1. identify which outputs have declared contracts;
2. run applicable validation after each contracted output is produced;
3. record validation results;
4. prevent required failed outputs from silently continuing downstream;
5. preserve failed outputs or validation reports where practical;
6. expose contract failures clearly to the user.

A more advanced engine may also:

- auto-repair failed outputs;
- invoke specialised review modules;
- route outputs to human approval;
- use different models for generation and validation;
- compare versions;
- improve modules based on repeated contract failures.

---

## 23. Open Design Questions

This document is intentionally draft-level. Important open questions remain.

### Contract syntax

Should contracts be declared:

- inline on Data Units?
- in a root-level `<Contracts>` section?
- in external contract files?
- in a contract registry?
- all of the above?

### Contract schema

Should the contract schema be:

- JSON Schema?
- XML Schema?
- YAML-based?
- TTML-native?
- abstract and source-format-independent?

### Validation execution

Should validation be represented as:

- an engine-level process?
- explicit `ValidationGate` operations?
- deterministic functions?
- review modules?
- all of the above?

### Strictness model

What strictness levels should be standard?

Possible values:

```text
advisory
warning
required
approval_required
terminal
```

### LLM review reliability

How should LLM-assisted validation itself be reviewed?

Possible approaches:

- use separate review models;
- require structured validation reports;
- run deterministic checks first;
- require human approval for high-impact artefacts;
- compare multiple reviewers;
- preserve confidence and rationale.

### Contract registries

How should reusable contracts be versioned, shared and referenced?

Possible metadata:

- contract ID;
- semantic type;
- version;
- author;
- validation functions;
- compatible engines;
- examples;
- test cases;
- known limitations.

---

## 24. Summary

Semantic Contracts are the mechanism by which a Thought Tree program defines what its outputs are supposed to mean.

They allow the framework to move beyond:

```text
Did the operation produce a file?
```

towards:

```text
Did the operation produce the right kind of artefact,
with the right structure, quality, meaning and downstream usefulness?
```

Without Semantic Contracts, a Thought Tree workflow may appear to execute successfully while passing weak, incomplete or misleading outputs into later operations.

With Semantic Contracts, the Cognitive Engine can:

- validate outputs;
- trigger repair;
- request review;
- preserve provenance;
- improve reliability;
- support module testing;
- enable safer dynamic planning;
- make cognitive work more auditable.

Semantic Contracts connect several central goals of the Thought Tree Framework:

- modularity;
- inspectability;
- auditability;
- reusability;
- model independence;
- hybrid execution;
- quality control;
- self-improvement.

They are one of the key features that distinguish a cognitive program from a simple chain of prompts.