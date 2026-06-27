# Contributing to the Thought Tree Framework

Thank you for your interest in contributing to the Thought Tree Framework.

This project is currently in **handoff / early specification** status. The original author has developed the core concepts and early prototypes but is stepping back from active development due to personal, family, work, health, and capacity constraints.

The purpose of this repository is to make the idea available for others to explore, critique, implement, fork, document, and continue.

Contributions of many kinds are welcome.

---

## What Is Thought Tree?

Thought Tree is an open framework for describing complex LLM-assisted work as executable, inspectable cognitive programs.

Instead of relying on a single prompt, an opaque agent loop, or an informal chain of prompts, a Thought Tree defines:

- required inputs;
- intermediate artefacts;
- operations that transform artefacts;
- review stages;
- validation criteria;
- final outputs;
- execution traces.

The core pattern is:

```text
Data Units → Operations → Data Units
```

A Cognitive Engine compiles and executes Thought Tree Modules using LLMs, deterministic functions, tools, submodules, semantic contracts, human review, and workspace traces.

The long-term aim is to support **cognitive programming**: structured, reusable, inspectable, model-independent workflows for transforming concepts, documents, requirements, plans, reviews, policies, stories, designs, and other semantic artefacts.

---

## Project Context

This is not currently a mature production framework.

It is an open handoff of:

- a developed conceptual framework;
- a draft source format called TTML;
- early prototype code;
- examples and design patterns;
- a roadmap for future implementation;
- an invitation for others to take the idea further.

Please approach the repository with that in mind.

The project needs help with:

- documentation;
- specification clarity;
- examples;
- reference implementation;
- comparisons with existing tools;
- schema design;
- testing;
- visualisation;
- validation;
- engine architecture;
- module libraries;
- governance and safety.

---

# Ways to Contribute

You do not need to be an expert in every part of the framework to contribute.

Useful contributions include:

## Documentation

- Improve the README.
- Convert sections of the framework PDF into Markdown.
- Create diagrams.
- Clarify terminology.
- Improve the glossary.
- Write shorter explanations.
- Write tutorials.
- Create examples.
- Explain how Thought Tree differs from existing tools.

## Specification Work

- Refine the Thought Tree Program Model.
- Improve the TTML draft.
- Propose schema changes.
- Define operation semantics.
- Define iterator expansion rules.
- Define Collection behaviour.
- Define execution trace schema.
- Define semantic contract schema.
- Define conformance levels.

## Examples

- Create minimal TTML examples.
- Create larger workflow examples.
- Create invalid examples for validation testing.
- Create examples for:
  - article summarisation;
  - technical documentation;
  - compliance analysis;
  - creative writing;
  - project recovery;
  - module improvement;
  - research monitoring.

## Engine Implementation

- Build or improve a reference Cognitive Engine.
- Implement TTML parsing.
- Implement workspace management.
- Implement `TextCompletion`.
- Implement deterministic functions.
- Implement `ExecuteFunction`.
- Implement iterator expansion.
- Implement Collections.
- Implement output collision detection.
- Implement execution traces.
- Implement graph compilation.
- Implement dry-run planning.

## Tooling

- TTML linter.
- TTML formatter.
- TTML validator.
- Graph visualiser.
- CLI runner.
- YAML-to-TTML converter.
- Visual editor prototype.
- Execution trace viewer.
- Module testing harness.

## Research and Comparison

- Compare Thought Tree with:
  - LangGraph;
  - LangChain;
  - AutoGen;
  - CrewAI;
  - Semantic Kernel;
  - workflow engines;
  - agent frameworks;
  - prompt chaining systems.
- Identify overlap, differences, and opportunities for integration.
- Suggest terminology improvements.
- Suggest academic or technical framing.

## Governance and Safety

- Define permission models.
- Suggest sandboxing approaches.
- Define human review semantics.
- Identify prompt injection risks.
- Suggest trace and audit requirements.
- Propose cost-control models.
- Define safe execution modes.

---

# Good First Contributions

If you are new to the project, good starting points include:

- Convert one section of the framework PDF into Markdown.
- Create a minimal `article-summary-review.ttml` example.
- Improve the one-page overview.
- Add a simple diagram of the Cognitive Engine architecture.
- Write a short comparison with one existing AI workflow framework.
- Draft an execution trace JSON example.
- Implement a basic `ConcatenateFiles` function.
- Create invalid TTML examples for testing.
- Create a Mermaid diagram for `Data Units → Operations → Data Units`.
- Improve glossary definitions.
- Review the TTML draft and identify ambiguities.

---

# Contribution Principles

Please try to preserve the core design principles of the framework.

## 1. Make cognitive work explicit

Thought Tree is about defining the process, not just asking for an output.

Prefer:

```text
Input → Draft → Review → Revise → Final
```

over:

```text
Input → Final
```

when quality, traceability, or inspection matters.

---

## 2. Preserve intermediate artefacts

Intermediate Data Units are a major part of the framework.

They allow workflows to be:

- inspected;
- debugged;
- reviewed;
- reused;
- repaired;
- improved.

Avoid hiding too much work inside a single large operation.

---

## 3. Use LLMs for semantic work

LLMs are useful for:

- analysis;
- summarisation;
- drafting;
- review;
- synthesis;
- classification;
- extraction;
- planning;
- revision.

---

## 4. Use deterministic functions for mechanical work

Do not use an LLM for tasks that ordinary software can perform more reliably.

Use deterministic functions for:

- copying files;
- concatenating files;
- validating XML;
- converting formats;
- calculating hashes;
- creating archives;
- parsing structured data.

---

## 5. Keep workflows inspectable

A reader should be able to understand:

- what inputs are needed;
- what operations happen;
- what intermediate artefacts are produced;
- what outputs are returned;
- what validation or review occurs.

---

## 6. Avoid silent overwrites

Output collisions should be detected.

If multiple operation instances write to the same output identifier, the engine should reject the workflow or require an explicit resolution strategy.

Valid strategies may include:

- adding active iterators to output names;
- aggregating inputs into Collections;
- creating versioned outputs;
- explicit overwrite declarations;
- human review.

---

## 7. Treat TTML as a source format, not the whole framework

TTML is currently the proposed XML-based serialisation format.

The underlying framework is broader:

```text
Thought Tree Program Model
↓
Source representation such as TTML
↓
Cognitive Engine
↓
Executable transformation graph
↓
LLMs / functions / tools / humans
↓
Outputs and traces
```

Future contributors may propose YAML, JSON, visual graph editors, or other authoring formats, provided they preserve the underlying model.

---

## 8. Design for model independence

A Thought Tree Module should usually describe what transformation is needed, not which specific LLM provider must perform it.

Prefer:

```text
requires high-quality long-context reasoning
```

over:

```text
use model X from provider Y
```

unless the dependency is intentional and justified.

---

## 9. Support validation and contracts

The framework should distinguish between:

1. schema correctness;
2. execution correctness;
3. semantic correctness.

A workflow should not be considered successful merely because a file exists.

Important outputs should be validated against their intended purpose.

---

## 10. Support human review where judgement matters

Human review is not a failure of automation.

It is an important part of reliable cognitive workflows, especially for:

- compliance;
- legal;
- safety;
- finance;
- publication;
- high-impact decision-making;
- subjective creative quality.

Human decisions should be recorded in the execution trace.

---

# Repository Structure

The repository may evolve, but a suggested structure is:

```text
thoughttree-framework/
  README.md
  HANDOFF.md
  STATUS.md
  ROADMAP.md
  CONTRIBUTING.md
  LICENSE

  docs/
    ONE_PAGE_OVERVIEW.md
    WHY_THIS_MATTERS.md
    ARCHITECTURE.md
    PROGRAM_MODEL.md
    COGNITIVE_ENGINE.md
    EXECUTION_SEMANTICS.md
    SEMANTIC_CONTRACTS.md
    AUTHORING_GUIDE.md
    POSITIONING.md
    ThoughtTreeFramework.pdf

  spec/
    TTML_DRAFT.md
    TTMLSchema.xsd
    EXECUTION_TRACE_SCHEMA_DRAFT.json
    CONTRACT_SCHEMA_DRAFT.json

  examples/
    article-summary-review/
    novel-generation-pipeline/
    video-game-tdd/
    compliance-gap-analysis/
    module-improvement/

  engine/
    README.md
    src/
    tests/

  prototypes/
    unity-novel-poc/
    unity-cognitive-engine/

  tools/
    ttml-lint/
    ttml-graph/
    ttml-format/

  tests/
    conformance/
```

Do not worry if the repository does not yet match this structure. Contributions that move it in a clearer direction are welcome.

---

# Development Status

The project is not yet stable.

Expect that:

- terminology may change;
- TTML may change;
- examples may be incomplete;
- engine behaviour may be undefined;
- the schema may not be final;
- documentation may contain inconsistencies;
- prototypes may not represent the ideal future architecture.

If you notice ambiguity, please open an issue or submit a clarification.

---

# Before Opening a Pull Request

Before submitting a pull request, please check:

- Does the contribution align with the framework’s core principles?
- Is the change explained clearly?
- Does it improve clarity, executability, validation, traceability, or usefulness?
- If it changes terminology, does it update related documentation?
- If it changes TTML syntax, does it include an example?
- If it changes engine behaviour, does it include or suggest tests?
- If it adds an example, is the example understandable to a new reader?
- If it introduces a new concept, is it defined in the glossary or relevant docs?

---

# Pull Request Guidelines

When opening a pull request, please include:

## Summary

Briefly describe what you changed.

## Motivation

Explain why the change is useful.

## Type of Contribution

Choose one or more:

- documentation;
- specification;
- example;
- engine implementation;
- tooling;
- test;
- bug fix;
- terminology;
- research/comparison;
- governance/safety.

## Related Issues

Link any related issues.

## Notes

Mention anything uncertain, incomplete, or requiring review.

---

# Issue Guidelines

When opening an issue, please try to label or describe it as one of:

- `documentation`
- `spec`
- `ttml`
- `engine`
- `example`
- `question`
- `good first issue`
- `architecture`
- `semantic contracts`
- `execution trace`
- `governance`
- `comparison`
- `bug`
- `proposal`

Useful issue formats include:

## Documentation Issue

```markdown
## Problem

What is unclear, missing, or inconsistent?

## Location

Which file or section?

## Suggested Improvement

How could it be improved?
```

## Specification Issue

```markdown
## Problem

What part of the model or TTML spec is ambiguous?

## Example

Provide an example if possible.

## Proposed Resolution

Suggest a rule, clarification, or alternative.
```

## Engine Issue

```markdown
## Expected Behaviour

What should the Cognitive Engine do?

## Actual or Current Behaviour

What does it currently do, or what is missing?

## Suggested Implementation

Any ideas for implementation?
```

## Proposal

```markdown
## Proposal

Describe the new idea.

## Why It Matters

Explain the benefit.

## Risks or Trade-offs

Mention complexity, ambiguity, or compatibility concerns.

## Example

Show how it might look in TTML, code, or execution semantics.
```

---

# Coding Guidelines

There is not yet a single official implementation language.

If contributing code:

- keep it simple;
- document assumptions;
- avoid unnecessary dependencies;
- write clear tests where possible;
- separate parsing from execution;
- separate source format from internal program model;
- separate LLM providers from engine semantics;
- separate deterministic functions from LLM operations;
- record trace information wherever practical.

Recommended architectural separation:

```text
Source Parser
↓
Program Model
↓
Graph Compiler
↓
Execution Planner
↓
Runtime
↓
Workspace
↓
Trace
```

Avoid building an engine that treats TTML as merely a list of prompts.

---

# Documentation Style

When writing documentation:

- prefer clear language over hype;
- define terms before using them heavily;
- use examples;
- show small workflows before large ones;
- distinguish current behaviour from future ideas;
- label speculative ideas clearly;
- avoid claiming the framework is complete;
- avoid claiming LLM outputs are automatically correct.

Preferred phrasing:

```text
Thought Tree aims to...
The framework proposes...
A Cognitive Engine should...
A future version may...
```

Avoid overclaiming:

```text
Thought Tree guarantees correctness.
Thought Tree solves hallucination.
Thought Tree replaces human judgement.
```

The framework is about structure, traceability, validation, and improvement — not magic correctness.

---

# TTML Contribution Guidelines

If contributing to TTML:

- preserve human readability;
- preserve LLM authorability;
- keep the core syntax simple;
- document all new elements and attributes;
- include examples;
- consider execution semantics;
- consider validation;
- consider backwards compatibility;
- avoid adding features that belong in the engine rather than the source format.

Important TTML design questions:

- Is this source-level intent, or engine-level implementation detail?
- Does this improve portability?
- Can a human understand it?
- Can an LLM author it reliably?
- Can a Cognitive Engine validate it?
- Can it be traced during execution?

---

# Example Contribution Guidelines

Good examples should include:

- a short README;
- input description;
- final output description;
- TTML file or program-model sketch;
- explanation of operations;
- expected intermediate artefacts;
- notes on validation;
- notes on what the example demonstrates.

Example folder structure:

```text
examples/article-summary-review/
  README.md
  article_summary_review.ttml
  inputs/
    source_article.txt
  expected/
    expected_flow.md
```

Examples do not need to be perfect, but they should be understandable.

---

# Semantic Contract Contribution Guidelines

Semantic contracts are still a developing part of the framework.

If contributing contracts, try to specify:

- semantic type;
- required structure;
- required sections;
- quality criteria;
- validation method;
- failure behaviour;
- whether human review is required.

Example:

```yaml
contract: SummaryContract
semantic_type: Summary
required_sections:
  - main_subject
  - key_points
  - caveats
quality_criteria:
  - must not introduce unsupported claims
  - must be shorter than the source
  - must preserve important qualifications
validation:
  deterministic:
    - ValidateRequiredSections
  llm_review:
    - ReviewAgainstSource
human_review:
  required: false
```

---

# Governance and Maintainer Status

This project is currently in handoff mode.

The original author may not be able to:

- review every issue;
- review every pull request;
- answer all questions;
- maintain the engine;
- manage releases;
- make final architectural decisions.

Contributors are welcome to:

- fork the project;
- build independent implementations;
- propose stewardship structures;
- create community-maintained branches;
- use the ideas in other projects;
- adapt the specification;
- create alternative source formats;
- create compatible or incompatible engines.

If a community of contributors forms, future governance can be defined more formally.

---

# Forking and Independent Implementations

Forks and independent implementations are welcome.

The framework is intended to be open and adaptable.

Possible independent projects include:

- a Python Cognitive Engine;
- a TypeScript Cognitive Engine;
- a .NET Cognitive Engine;
- a CLI runner;
- a visual editor;
- a LangGraph integration;
- a Semantic Kernel integration;
- a module registry;
- a contract validator;
- an enterprise governance layer;
- a creative production platform.

If your implementation diverges from the draft spec, please document the differences clearly.

---

# Communication

Depending on repository setup, discussion may happen through:

- GitHub Issues;
- GitHub Discussions;
- pull request comments;
- external community spaces;
- forks and independent repositories.

Please keep communication constructive.

---

# Code of Conduct

This project should be a respectful and accessible space.

Please:

- be kind;
- assume good faith;
- explain disagreements clearly;
- avoid personal attacks;
- be patient with incomplete work;
- remember that this is a handoff project;
- respect neurodivergent contributors and different communication styles;
- avoid pressuring the original author for ongoing labour.

Unacceptable behaviour includes:

- harassment;
- abuse;
- personal insults;
- discriminatory language;
- repeated bad-faith argument;
- demands that unpaid contributors provide work on command.

---

# Licensing

Please check the repository license before contributing.

By contributing, you agree that your contribution may be distributed under the repository’s license.

If the repository uses separate licenses for code and documentation, please ensure your contribution is compatible with the relevant license.

Suggested licensing model:

- code: Apache-2.0 or MIT;
- documentation/specification: Creative Commons Attribution 4.0.

This may change depending on final repository decisions.

---

# Recognition

Contributors may be recognised through:

- GitHub contribution history;
- acknowledgements file;
- release notes;
- documentation credits.

If you want attribution in a particular form, please mention it in your pull request.

---

# Final Note

Thought Tree is an unfinished but potentially valuable framework.

Its central idea is simple:

```text
LLM-assisted work should be structured, inspectable, reusable, validatable, traceable, and improvable.
```

If you can help make that idea clearer, simpler, more executable, better documented, better tested, or more useful, your contribution is welcome.