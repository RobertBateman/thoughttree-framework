# GOVERNANCE.md

# Thought Tree Framework Governance

## 1. Project Status: Handoff Mode

The Thought Tree Framework is currently in **handoff mode**.

The original author has developed the core concept, framework documentation, TTML draft direction, prototype proof-of-concept work, and partial Cognitive Engine prototype. However, the original author is stepping back from active development due to family commitments, caring responsibilities, work commitments, burnout recovery, and the fact that the project now requires skills, time, community and technical stewardship beyond what the original author can provide.

This repository is therefore released openly so that others may:

- study the framework;
- fork it;
- implement it;
- improve it;
- critique it;
- adapt it;
- build compatible engines;
- create alternative source formats;
- develop module libraries;
- or continue the work independently.

There is no expectation that the original author will act as an active maintainer, product owner, support provider or final decision-maker.

---

## 2. Project Purpose

The purpose of the Thought Tree Framework is to provide an open model for **cognitive programming**: defining, compiling, executing, validating and improving structured LLM-assisted workflows.

The framework is based on the idea that complex cognitive work should not be treated as a single prompt, an opaque agent loop or an informal chain of completions. Instead, it should be represented as an inspectable process made from:

- Data Units;
- Operations;
- Modules;
- Collections;
- Iterators;
- Semantic Types;
- Contracts;
- Execution Traces;
- and Cognitive Engines.

At the centre of the framework is the pattern:

```text
Data Units → Operations → Data Units
```
A Cognitive Engine compiles a Thought Tree source definition, such as TTML, into an executable cognitive transformation graph and executes it using LLMs, deterministic functions, tools, submodules and human review where appropriate.

## 3. What This Project Is
The Thought Tree Framework is intended to be:

- an open conceptual framework;
- a draft cognitive programming model;
- a proposed source/interchange format through TTML;
- a foundation for Cognitive Engine implementations;
- a basis for reusable cognitive workflow modules;
- a model for inspectable, traceable and improvable LLM-assisted work.

## 4. What This Project Is Not
This project is not currently:

- a finished production system;
- a stable standard;
- a complete Cognitive Engine implementation;
- a replacement for human judgement;
- a guarantee of correct LLM outputs;
- a claim that TTML is the only possible representation;
- a claim that Unity/C# is the best long-term implementation path;
- a centrally controlled commercial product.

The existing prototypes are proof-of-concept and exploratory work. They should be treated as historical and experimental unless future maintainers decide otherwise.

## 5. Core Principles
Future work on the Thought Tree Framework should, where possible, preserve the following principles.

### 5.1 Process Over Prompt
A Thought Tree should describe a process, not merely a prompt.

The framework exists to make cognitive work decomposed, inspectable, reusable and improvable.

### 5.2 Separation of Program and Execution
A Thought Tree Module should define what cognitive process should happen.

A Cognitive Engine should determine how that process is compiled and executed.

LLMs, tools, deterministic functions and humans are execution resources, not the whole system.

### 5.3 Model Independence
Thought Tree programs should avoid unnecessary dependence on a specific LLM provider or model.

The same cognitive program should, where practical, be executable using different models, local or remote providers, different function registries and different runtime policies.

### 5.4 Explicit Artefacts
Important intermediate artefacts should be preserved.

A final output should be traceable through the Data Units, Operations, reviews, revisions and validations that produced it.

### 5.5 Hybrid Execution
LLMs should be used for ambiguous semantic transformations.

Deterministic functions should be used for mechanical, repeatable or formally checkable work.

Human review should be used where judgement, approval, governance or accountability is required.

### 5.6 Validation and Semantic Contracts
Producing a file is not enough.

Where possible, Data Units should be validated against semantic expectations, contracts, schemas, review rubrics or human approval gates.

The framework should distinguish between:

- schema correctness;
- execution correctness;
- semantic correctness.

### 5.7 Traceability
Executions should produce traces.

A trace should record what happened, what inputs were used, what models or functions were called, what outputs were produced, what validations passed or failed, and what human decisions occurred.

### 5.8 Safe Dynamic Behaviour
DynamicCompletion, ProjectCompletion and generated submodules are powerful but should be controlled.

Generated plans or modules should be inspectable, validated and, where appropriate, approved before execution.

### 5.9 Human-Readable and LLM-Authorable
Thought Tree source formats should be understandable by humans and authorable/reviewable by LLMs.

TTML is currently the main draft serialisation format, but future formats such as YAML, JSON or visual graph representations may also be valid if they compile to the same program model.

## 6. Stewardship

### 6.1 Original Author
The original author is the creator of the initial framework concept, documentation and prototypes.

In handoff mode, the original author:
- may clarify historical intent when available;
- may review major proposals if capacity allows;
- may not respond quickly or at all;
- is not responsible for maintaining forks, implementations or downstream projects;
- is not responsible for providing support, guarantees, bug fixes or roadmap delivery.

### 6.2 Current Maintainers
At the time of this handoff release, there may be no active maintainers.

If no maintainers are active, contributors are encouraged to fork the project and continue independently.

### 6.3 Future Maintainers
Future maintainers may emerge from the community.

A future maintainer may be someone who:

- contributes sustained improvements;
- helps clarify the specification;
- builds or maintains a Cognitive Engine;
- maintains documentation;
- curates examples;
- manages issues and pull requests;
- supports module library development;
- or helps coordinate community direction.

If an active maintainer group forms, it should document its membership and decision process in this file or a successor governance document.

## 7. Contribution Model
Contributions are welcome in the form of:

- documentation improvements;
- terminology clarification;
- diagrams;
- TTML examples;
- schema improvements;
- Cognitive Engine prototypes;
- alternative source formats;
- execution trace schemas;
- semantic contract schemas;
- tests and conformance examples;
- comparisons with related frameworks;
- module examples;
- validation tools;
- issue reports;
- implementation notes;
- forks and independent continuations.

Because the project is in handoff mode, pull requests and issues may not receive prompt review unless an active maintainer group forms.

## 8. Decision Making

### 8.1 During Handoff Mode
During handoff mode:

- no central maintainer is guaranteed;
- no single implementation is authoritative;
- forks are welcome;
- independent implementations are welcome;
- discussion is encouraged but may not result in formal decisions;
- contributors may use issues, discussions, forks or external spaces to continue the work.

If there is no active maintainer, the practical decision-making mechanism is forking and implementation.

### 8.2 If Active Maintainers Form
If an active maintainer group forms, decisions should preferably be made by rough consensus.

Major changes should be documented, especially changes affecting:

- the Thought Tree Program Model;
- TTML syntax;
- execution semantics;
- semantic contracts;
- conformance expectations;
- licensing;
- governance;
- security model;
- module registry design.

For significant proposals, maintainers are encouraged to use an RFC-style process.

Suggested path:

rfcs/
  0001-example-proposal.md
  
Each RFC should explain:

- the problem;
- proposed change;
- alternatives considered;
- compatibility impact;
- effect on existing modules;
- effect on Cognitive Engine implementations;
- migration guidance if needed.

## 9. Versioning
The framework should distinguish between:

- the abstract Thought Tree Program Model;
- TTML versions;
- Cognitive Engine implementations;
- module versions;
- contract versions;
- execution trace schema versions.

Breaking changes to TTML or execution semantics should be clearly versioned and documented.

A stable 1.0 label should not be used until there is sufficient agreement on:

- core program model;
- TTML structure;
- execution semantics;
- conformance expectations;
- minimal examples;
- and at least one working reference implementation.

## 10. Conformance
A Cognitive Engine should not be considered conformant merely because it can send prompts to an LLM.

A basic conformant engine should, at minimum, aim to support:

- loading a Thought Tree source definition;
- validating its structure;
- resolving inputs and references;
- executing declared operations;
- preserving intermediate artefacts;
- resolving final outputs;
- detecting unresolved dependencies;
- detecting output collisions;
- recording an execution trace.

More advanced engines may support:

- iterator expansion;
- Collections;
- semantic types;
- semantic contracts;
- validation gates;
- human review;
- DynamicCompletion;
- ProjectCompletion;
- generated submodules;
- module registries;
- function registries;
- tool registries;
- testing and improvement workflows.

Future maintainers may define formal conformance levels.

## 11. Safety and Governance Expectations
Implementations should take care when supporting:

- file system access;
- external tool calls;
- code execution;
- network access;
- generated modules;
- dynamic execution plans;
- human approval gates;
- model provider credentials;
- sensitive data;
- automated publishing;
- compliance, legal, medical, financial or safety-sensitive workflows.

Dynamic or generated modules should be validated before execution.

Silent overwriting, silent failure and untraceable execution should be avoided.

## 12. Licensing
This project uses a permissive licensing model.

Unless otherwise stated:

- software source code is licensed under the MIT License;
- documentation, specifications, examples, diagrams, conceptual material and TTML examples are released under CC0 1.0 Universal.

See LICENSE.md for details.

Contributors agree that their contributions are provided under the applicable license for the part of the project they contribute to.

## 13. Forks and Independent Implementations
Forks are explicitly welcome.

Independent implementations are explicitly welcome.

No permission is required to:

- implement a Cognitive Engine;
- create an alternative TTML parser;
- create another source format;
- create module libraries;
- build commercial or non-commercial tooling;
- adapt the framework for private use;
- publish derivative documentation;
- experiment with incompatible variants.

Attribution is appreciated but not required for CC0-covered material.

For MIT-covered code, the MIT license notice must be preserved as required by the license.

## 14. Project Name
The name “Thought Tree Framework” may be used to refer to the original framework concept and compatible implementations.

If future maintainers, forks or companies create substantially incompatible versions, they are encouraged to make that clear to avoid confusion.

This governance file does not establish a trademark policy. If a formal trademark policy is ever required, it should be added separately.

## 15. No Warranty or Obligation
The Thought Tree Framework is provided as-is.

There is no warranty that:

- the framework is correct;
- the documentation is complete;
- the prototypes work;
- the TTML draft is stable;
- any Cognitive Engine implementation is safe;
- generated outputs are accurate;
- workflows are suitable for production use.

Users and implementers are responsible for their own validation, safety, compliance and review processes.

## 16. Summary
The Thought Tree Framework is being released openly so that the idea can survive beyond the original author’s capacity to continue it.

The preferred future of the project is one in which others can freely study, fork, implement, refine, criticise, extend and improve the framework.

The central aim remains:

To move LLM-assisted work beyond isolated prompting and opaque automation toward structured, inspectable, reusable and improvable cognitive programs.
