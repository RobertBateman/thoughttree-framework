# **Examples**

This section illustrates how Thought Tree cognitive programs can be used to define, execute and 
improve LLM-assisted work. The examples are intentionally described at the program-model level 
rather than only as TTML, because the central idea of the framework is independent of any single 
serialisation format. 
Each example shows how a task can be represented as: 
```text 
Concepts / Data Units 
↓ 
Transformations / Operations 
↓ 
New Concepts / Data Units 
↓ 
Validation / Review / Trace 
``` 
The examples also demonstrate the hybrid nature of the framework. Some transformations are 
performed by LLMs, some by deterministic functions, some by reusable Modules, and some may 
involve human review. 
--- 
# 1. Minimal Example: Article Summary and Review 

## Purpose 
This example demonstrates the smallest useful Thought Tree pattern: take an input document, 
transform it into an output, review the output, and produce a final version. 
The task is: 
> Given an article, produce a concise summary, review it for accuracy and completeness, then 
produce a final revised summary. 
## Concepts 
| Concept | Description | 
|---|---| 
| Article | The source document to summarise. | 
| Draft Summary | The first generated summary. | 
| Summary Review | A review of the draft summary against the source article. | 
| Final Summary | The revised summary produced after review. | 
## Data Units 
| Data Unit | Semantic Type | Physical Format | 
|---|---|---| 
| `source_article` | Article | txt, md, html or pdf | 
| `draft_summary` | Summary | txt or md | 
| `summary_review` | ReviewReport | txt or md | 
| `final_summary` | Summary | txt or md | 
 
## Transformation Flow 
 
```text 
source_article 
    ↓ 
SummariseArticle 
    ↓ 
draft_summary 
    ↓ 
ReviewSummaryAgainstArticle 
    ↓ 
summary_review 
    ↓ 
ReviseSummary 
    ↓ 
final_summary 
``` 
 
## Operations 
 
### 1. Summarise Article 
 
Type: 
 
```text 
TextCompletion 
``` 
 
Inputs: 
 
```text 
source_article 
``` 
 
Output: 
 
```text 
draft_summary 
``` 
 
Description: 
 
> Produce a concise summary of the article. Preserve the main claims, supporting points and 
important qualifications. Do not introduce unsupported information. 
 --- 
 
### 2. Review Summary Against Article 
 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
source_article 
draft_summary 
``` 
Output: 
```text 
summary_review 
``` 
Description: 
> Review the draft summary against the source article. Identify omissions, inaccuracies, unsupported 
claims, unclear wording and areas where the summary could be more faithful to the source. 
--- 
### 3. Revise Summary 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
source_article 
draft_summary 
summary_review 
``` 
Output: 
```text 
final_summary 
``` 
Description: 
> Revise the draft summary using the review. Produce a final summary that is concise, accurate, 
complete and faithful to the source article. 

## Contracts 
The `final_summary` Data Unit may have a semantic contract such as: 
```text 
SummaryContract: - must identify the article's main subject; - must include the primary claims or findings; - must not introduce unsupported information; - must be shorter than the source; - must be understandable without reading the original article; - must preserve important caveats or qualifications. 
``` 
## Why This Is Useful 
This example shows the basic Thought Tree advantage over a single prompt. Instead of asking the 
LLM to “summarise the article” once, the process preserves: - the draft summary; - the review; - the final summary; - the relationship between source, draft, review and revision; - the execution trace showing how the final output was produced. 
This makes even a simple task inspectable and improvable. 
--- 

# 2. Legacy Project Recovery Example 

## Purpose 
This example demonstrates how Thought Tree can turn scattered or incomplete project material into 
structured documentation. 
The task is: 
> Given old notes, meeting transcripts, incomplete specifications and miscellaneous files, recover the 
state of a legacy project and produce a clean project recovery pack. 

## Concepts 
| Concept | Description | 
|---|---| 
| Source Notes | Unstructured legacy material. | 
| Source Digest | Structured summary of all available source material. | 
| Project Timeline | Reconstructed history of the project. | 
| Requirements Register | Extracted functional and non-functional requirements. | 
| Decision Log | Important decisions and their evidence. | 
| Open Issues List | Unresolved questions, risks and gaps. | 
| Recovery Report | Final coherent project recovery document. | 
 
## Data Units 
 
| Data Unit | Semantic Type | 
|---|---| 
| `legacy_project_material` | SourceCollection | 
| `normalized_project_material` | NormalizedTextCollection | 
| `source_digest` | SourceDigest | 
| `project_timeline` | Timeline | 
| `requirements_register` | RequirementsRegister | 
| `decision_log` | DecisionLog | 
| `open_issues_list` | IssueRegister | 
| `project_recovery_report` | RecoveryReport | 
 
## Transformation Flow 
 
```text 
legacy_project_material 
    ↓ 
NormalizeSourceMaterial 
    ↓ 
normalized_project_material 
    ↓ 
CreateSourceDigest 
    ↓ 
source_digest 
    ├── ExtractTimeline ───────────────→ project_timeline 
    ├── ExtractRequirements ───────────→ requirements_register 
    ├── ExtractDecisions ──────────────→ decision_log 
    └── ExtractOpenIssuesAndRisks ─────→ open_issues_list 
                    ↓ 
            CompileRecoveryReport 
                    ↓ 
        draft_project_recovery_report 
                    ↓ 
            ReviewRecoveryReport 
                    ↓ 
        project_recovery_report 
``` 
 
## Operations 
 
### 1. Normalize Source Material 
 
Type: 
 
```text 
ExecuteFunction 
``` 
 
Function: 
```text 
ConvertDocumentsToText 
``` 
Inputs: 
```text 
legacy_project_material 
``` 
Output: 
```text 
normalized_project_material 
``` 
Purpose: 
Convert PDFs, documents, markdown files, meeting notes and other supported source files into 
readable text. 
--- 
### 2. Create Source Digest 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
normalized_project_material 
``` 
Output: 
```text 
source_digest 
``` 
Purpose: 
Create a structured digest of the source material, preserving facts, dates, names, decisions, 
contradictions and unresolved questions. 
--- 
### 3. Extract Requirements 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
source_digest 
``` 
Output: 
```text 
requirements_register 
``` 
Purpose: 
Extract explicit, inferred and unresolved requirements. 
--- 
### 4. Extract Project Timeline 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
source_digest 
``` 
Output: 
```text 
project_timeline 
``` 
Purpose: 
Reconstruct a chronological timeline of the project. 
--- 
### 5. Extract Decisions 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
source_digest 
``` 
Output: 
```text 
decision_log 
``` 
Purpose: 
Identify important decisions, rationale, dates and supporting evidence. 
--- 
### 6. Extract Open Issues and Risks 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
source_digest 
``` 
Output: 
```text 
open_issues_list 
``` 
Purpose: 
Identify unresolved issues, contradictions, missing information and project risks. 
--- 
### 7. Compile Recovery Report 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
source_digest 
project_timeline 
requirements_register 
decision_log 
open_issues_list 
``` 
Output: 
```text 
draft_project_recovery_report 
``` 
Purpose: 
Produce a coherent project recovery report. 
--- 
### 8. Review Recovery Report 
Type: 
```text 
TextCompletion 
``` 
Inputs: 
```text 
draft_project_recovery_report 
source_digest 
requirements_register 
decision_log 
open_issues_list 
``` 
Output: 
```text 
project_recovery_report 
``` 
Purpose: 
Review and revise the draft report for completeness, source fidelity, clarity and usefulness. 
## Contracts 
The final report may require: 
```text 
RecoveryReportContract: - must include project overview; - must identify known goals and requirements; - must distinguish confirmed facts from inferred assumptions; - must list unresolved questions; - must include risk register; - must include recommended next actions; - must avoid unsupported claims; - must cite or reference source-derived evidence where possible. 
``` 
## Why This Is Useful 
This example shows the value of preserving intermediate artefacts. A human reviewer can inspect the 
requirements register, timeline, decision log or open issues independently, instead of only seeing a 
final generated document. 
--- 
# 3. Creative Production Example: Novel Development 
## Purpose 
This example demonstrates how Thought Tree can support a large creative production process. 
The task is: 
> Given a high-level creative brief, develop a novel by generating concept documents, character 
profiles, setting material, a plot outline, chapter drafts, simulated reader feedback, revised chapters 
and a final manuscript. 
## Concepts 
| Concept | Description | 
|---|---| 
| Story Requirements | Initial creative direction. | 
| Character Profile | Description of a major character. | 
| Setting Overview | Description of the world, locations and atmosphere. | 
| Plot Outline | Structural outline of the story. | 
| Chapter Draft | Draft prose for one chapter. | 
| Beta Reader Persona | Simulated reader perspective. | 
| Feedback Report | Simulated beta reader feedback on a chapter. | 
| Revised Chapter | Chapter revised using feedback. | 
| Manuscript | Complete assembled novel. | 
| Final Edited Novel | Polished final output. | 
 
## Data Units 
 
| Data Unit | Semantic Type | 
|---|---| 
| `story_requirements` | CreativeBrief | 
| `character_{{CharacterIterator}}_profile` | CharacterProfile | 
| `setting_overview` | SettingOverview | 
| `plot_outline` | PlotOutline | 
| `draft_chapter_{{ChapterIterator}}` | ChapterDraft | 
| `beta_reader_persona_{{PersonaIterator}}` | ReaderPersona | 
| `feedback_chapter_{{ChapterIterator}}_persona_{{PersonaIterator}}` | FeedbackReport | 
| `revised_chapter_{{ChapterIterator}}` | RevisedChapter | 
| `compiled_novel_manuscript` | Manuscript | 
| `final_edited_novel` | NovelManuscript | 
 
## Iterators 
 
```text 
CharacterIterator = 1..CharacterCount 
ChapterIterator = 1..ChapterCount 
PersonaIterator = 1..PersonaCount 
``` 
 
## Collections 
 
```text 
all_character_profiles 
    = character_1_profile 
      character_2_profile 
      ... 
 
feedback_for_chapter_{{ChapterIterator}} 
    = feedback_chapter_{{ChapterIterator}}_persona_1 
      feedback_chapter_{{ChapterIterator}}_persona_2 
      ... 
 
all_revised_chapters 
    = revised_chapter_1 
      revised_chapter_2 
      ... 
``` 
 
## Transformation Flow 
 
```text 
story_requirements 
    ↓ 
ConceptDevelopment 
    ├── character profiles 
    ├── setting_overview 
    └── plot_outline 
 
character profiles + setting_overview + plot_outline 
    ↓ 
DraftChapter[for each chapter] 
    ↓ 
draft_chapter_n 
 
setting_overview + plot_outline 
    ↓ 
CreateBetaReaders[for each persona] 
    ↓ 
beta_reader_persona_n 
 
draft_chapter_n + beta_reader_persona_n 
    ↓ 
SimulateFeedback[for each chapter/persona pair] 
    ↓ 
feedback_chapter_n_persona_m 
 
draft_chapter_n + feedback_for_chapter_n 
    ↓ 
ReviseChapter[for each chapter] 
    ↓ 
revised_chapter_n 
 
all_revised_chapters 
    ↓ 
CompileNovel 
    ↓ 
compiled_novel_manuscript 
 
compiled_novel_manuscript + plot_outline + setting_overview 
    ↓ 
FinalEditorialPass 
    ↓ 
final_edited_novel 
``` 
 
## Notable Features 
 
This example demonstrates several core framework capabilities: 
 
### Recursive modularity 
 
The `ConceptDevelopment` operation can be implemented as a pre-existing Module or generated 
submodule. 
 
### Iteration 
 
Chapter drafting, persona creation and feedback simulation are expanded over iterators. 
### Collection aggregation 
Feedback from multiple personas is grouped into a chapter-specific Collection before revision. 
### Hybrid execution - LLMs generate concepts, drafts, feedback and revisions. - A deterministic function concatenates revised chapters. - Optional validation checks continuity, chapter completeness and formatting. 
### Traceability 
The final edited novel can be traced back through: 
```text 
final_edited_novel 
← compiled_novel_manuscript 
← revised chapters 
← feedback reports 
← draft chapters 
← plot outline / setting / character profiles 
← story requirements 
``` 
## Contracts 
Example contracts include: 
```text 
CharacterProfileContract: - must include role, motivation, conflict, relationships and arc; - must be distinct from other major characters; - must support the plot outline. 
ChapterDraftContract: - must align with the plot outline; - must use relevant character profiles; - must preserve continuity; - must contain a complete chapter-level scene or sequence. 
NovelManuscriptContract: - must contain all revised chapters in order; - must be internally consistent; - must preserve major plot and character arcs; - must not include editorial notes unless requested. 
``` 
## Why This Is Useful 
This example shows Thought Tree as a production pipeline rather than a single generation prompt. It 
allows a complex creative output to be decomposed, reviewed, revised and assembled through an 
inspectable process. 
--- 
# 4. Technical Documentation Example: Video Game Technical Design Document 
## Purpose 
This example demonstrates use of Thought Tree for structured technical synthesis. 
The task is: 
> Given design notes for a video game, produce a complete Technical Design Document suitable for 
engineers, designers, technical artists, producers and QA. 
## Concepts 
| Concept | Description | 
|---|---| 
| Design Notes | Source material describing the game. | 
| Source Digest | Structured summary of the source notes. | 
| Requirements Register | Extracted technical and product requirements. | 
| Game Intent and Scope | Product intent and scope constraints. | 
| System Decomposition | Proposed technical system breakdown. | 
| TDD Section | One section of the final document. | 
| Draft TDD | Assembled technical design document. | 
| Review Plan | Review and correction plan. | 
| Final TDD | Polished Technical Design Document. | 
## Data Units 
| Data Unit | Semantic Type | 
|---|---| 
| `video_game_design_notes` | SourceNotes | 
| `source_digest` | SourceDigest | 
| `technical_requirements_register` | RequirementsRegister | 
| `game_intent_and_scope` | ProductScope | 
| `system_decomposition` | SystemArchitecturePlan | 
| `tdd_section_01_overview` | DocumentSection | 
| `tdd_section_02_requirements_constraints` | DocumentSection | 
| `tdd_section_03_architecture` | DocumentSection | 
| `tdd_section_04_gameplay_systems` | DocumentSection | 
| `draft_video_game_tdd` | DraftTechnicalDesignDocument | 
| `tdd_review_and_correction_plan` | ReviewPlan | 
| `video_game_technical_design_document` | TechnicalDesignDocument | 
## Transformation Flow 
```text 
video_game_design_notes 
    ↓ 
NormalizeDesignNotes 
    ↓ 
normalized_video_game_design_notes 
    ↓ 
CreateSourceDigest 
    ↓ 
source_digest 
    ├── ExtractTechnicalRequirements ─────→ technical_requirements_register 
    ├── ExtractGameIntentAndScope ─────────→ game_intent_and_scope 
    └── DesignSystemDecomposition ─────────→ system_decomposition 
 
source_digest + requirements + scope + decomposition 
    ↓ 
Draft TDD Sections 
    ↓ 
tdd_section_01 
tdd_section_02 
tdd_section_03 
... 
 
all TDD sections 
    ↓ 
AssembleDraftTDD 
    ↓ 
draft_video_game_tdd 
    ↓ 
ReviewDraftTDD 
    ↓ 
tdd_review_and_correction_plan 
    ↓ 
FinalizeVideoGameTDD 
    ↓ 
video_game_technical_design_document 
``` 
 
## Hybrid Execution 
 
This example uses: 
 
| Operation | Execution Type | 
|---|---| 
| Normalize design notes | Deterministic function | 
| Create source digest | LLM transformation | 
| Extract requirements | LLM transformation | 
| Draft sections | LLM transformation | 
| Assemble draft TDD | Deterministic function | 
| Review draft | LLM transformation | 
| Finalise TDD | LLM transformation | 
 
## Contracts 
 
The final TDD may have a contract such as: 
```text 
TechnicalDesignDocumentContract: - must include overview, requirements, architecture and implementation sections; - must distinguish explicit source facts from inferred assumptions; - must identify unresolved questions; - must avoid unsupported engine-specific claims; - must include risks and dependencies; - must be useful to engineering, design, production and QA audiences; - must preserve traceability to the source digest and requirements register. 
``` 
## Why This Is Useful 
This example shows that Thought Tree is not only for creative work. It can also perform structured 
organisational synthesis, where auditability, source fidelity and intermediate review are important. 
--- 
# 5. Compliance and Policy Review Example 
## Purpose 
This example demonstrates how Thought Tree can support audit, compliance and governance 
workflows. 
The task is: 
> Given policy documents, procedure notes and regulatory obligations, produce a compliance gap 
analysis and remediation plan. 
## Concepts 
| Concept | Description | 
|---|---| 
| Source Policy Material | Internal policy documents and notes. | 
| Regulatory Obligations | Requirements imposed by external rules. | 
| Obligation Register | Structured list of obligations. | 
| Evidence Map | Mapping between obligations and internal evidence. | 
| Gap Analysis | Identification of missing or weak compliance areas. | 
| Risk Register | Compliance risks and severity. | 
| Remediation Plan | Proposed actions to close gaps. | 
## Transformation Flow 
```text 
policy_material + regulatory_material 
↓ 
NormalizeSources 
↓ 
normalized_policy_material + normalized_regulatory_material 
    ↓ 
ExtractObligations 
    ↓ 
obligation_register 
    ↓ 
MapEvidenceToObligations 
    ↓ 
evidence_map 
    ↓ 
IdentifyComplianceGaps 
    ↓ 
gap_analysis 
    ↓ 
AssessComplianceRisks 
    ↓ 
risk_register 
    ↓ 
GenerateRemediationPlan 
    ↓ 
remediation_plan 
    ↓ 
ReviewForAuditReadiness 
    ↓ 
final_compliance_report 
``` 
 
## Important Contracts 
 
```text 
ObligationRegisterContract: - each obligation must be clearly stated; - each obligation must identify source regulation or policy source; - each item must be classified by type, priority and applicability. 
 
GapAnalysisContract: - must distinguish confirmed gaps from possible gaps; - must identify missing evidence; - must avoid legal conclusions unless explicitly authorised; - must include uncertainty and recommended human review points. 
 
RemediationPlanContract: - must include actions, owners, priorities and dependencies; - must connect each remediation action to one or more identified gaps; - must flag high-risk items for human review. 
``` 
 
## Human Review 
 
This example is a good candidate for required human review, because compliance outputs may affect 
legal, regulatory or operational decisions. 
 
A Thought Tree Module may therefore include a review gate: 
```text 
HumanReview: - approve obligation register; - approve gap analysis; - approve remediation plan; - record reviewer identity and decision. 
``` 
## Why This Is Useful 
The framework makes it possible to separate: - extraction of obligations; - evidence mapping; - gap identification; - risk assessment; - remediation planning; - human approval. 
This is more auditable than a single prompt asking for a compliance report. 
--- 
# 6. Cyclic Research and Monitoring Example 
## Purpose 
This example demonstrates how a Thought Tree workflow can be executed repeatedly over time. 
The task is: 
> Monitor a topic, collect new information, summarise changes, assess significance and produce a 
periodic report. 
## Concepts 
| Concept | Description | 
|---|---| 
| Monitoring Topic | The subject being tracked. | 
| Source Results | New information gathered during a cycle. | 
| Change Summary | What has changed since the last report. | 
| Significance Assessment | Evaluation of why the changes matter. | 
| Periodic Report | Final report for this cycle. | 
| Historical State | Previous reports and tracked context. | 
## Transformation Flow 
```text 
monitoring_topic + historical_state 
↓ 
RetrieveNewSources 
    ↓ 
new_source_results 
    ↓ 
SummariseNewInformation 
    ↓ 
new_information_summary 
    ↓ 
CompareWithPreviousState 
    ↓ 
change_summary 
    ↓ 
AssessSignificance 
    ↓ 
significance_assessment 
    ↓ 
GeneratePeriodicReport 
    ↓ 
periodic_report 
    ↓ 
UpdateHistoricalState 
    ↓ 
updated_historical_state 
``` 
 
## Execution Pattern 
 
This example may use cyclic execution: 
 
```text 
Run workflow 
Wait for scheduled interval 
Run workflow again 
Update state 
Repeat 
``` 
 
## Safeguards 
 
Cyclic workflows should declare: 
 
```text - schedule or trigger; - maximum runtime; - maximum cost per cycle; - source retrieval limits; - stopping conditions; - human review requirements; - state update rules. 
``` 
 
## Why This Is Useful 
 
This example shows Thought Tree as more than a one-shot production system. It can support 
ongoing cognitive processes such as: - market monitoring; - competitor analysis; - regulatory tracking; - customer feedback reporting; - project health reporting; - research briefings. 
--- 
# 7. Module Authoring Example 
## Purpose 
This example demonstrates the self-referential capability of the framework: using a Thought Tree 
program to generate another Thought Tree program. 
The task is: 
> Given a high-level operation description, the TTML schema and available function definitions, 
generate a valid executable TTML Module that can perform the operation. 
## Concepts 
| Concept | Description | 
|---|---| 
| Original Operation | The task to convert into a Module. | 
| Operation Requirements | Extracted requirements for the task. | 
| Function Usage Plan | Deterministic functions that should be used. | 
| Module Design | Planned structure of the generated Module. | 
| Draft TTML | First generated module definition. | 
| Validation Report | Schema and execution validation results. | 
| Final TTML | Corrected executable module. | 
## Transformation Flow 
```text 
original_operation + TTML_schema + function_definitions 
↓ 
ExtractOperationRequirements 
↓ 
operation_requirements 
↓ 
SelectFunctionUsage 
↓ 
function_usage_plan 
↓ 
DesignTTMLStructure 
↓ 
ttml_structure_design 
    ↓ 
DraftTTMLDocument 
    ↓ 
draft_generated_ttml 
    ↓ 
ValidateDraftTTML 
    ↓ 
validation_reports 
    ↓ 
AnalyzeValidation 
    ↓ 
draft_correction_plan 
    ↓ 
ReviseFinalTTML 
    ↓ 
NewTTML 
``` 
 
## Hybrid Execution 
 
| Step | Type | 
|---|---| 
| Extract requirements | TextCompletion | 
| Select function usage | TextCompletion | 
| Design TTML structure | TextCompletion | 
| Draft TTML document | TextCompletion | 
| Validate against schema | ExecuteFunction | 
| Validate with executor | ExecuteFunction | 
| Analyse validation | TextCompletion | 
| Revise final TTML | TextCompletion | 
 
## Why This Is Important 
 
This example shows that the framework can be used to author its own programs. 
 
This does not mean the system is automatically correct. The generated TTML still requires validation. 
But it does mean that Thought Tree can support a powerful workflow: 
 
```text 
ambiguous task 
    ↓ 
generated cognitive program 
    ↓ 
validation 
    ↓ 
execution 
``` 
 
This is one of the clearest ways in which Thought Tree differs from ordinary prompt chaining. 
--- 
 
# 8. Module Improvement Example 
## Purpose 
This example demonstrates how Thought Tree workflows can improve existing Thought Tree 
Modules. 
The task is: 
> Given an existing TTML Module, the operation it was intended to satisfy, the schema, function 
definitions and evaluation criteria, produce an improved TTML Module and an explanation of the 
changes. 
## Concepts 
| Concept | Description | 
|---|---| 
| Original TTML | Existing module to improve. | 
| Operation Requirements | What the module is intended to accomplish. | 
| Schema Validation Report | Whether the XML is structurally valid. | 
| Execution Validation Report | Whether the module can run correctly. | 
| Design Review | Assessment of module structure and quality. | 
| Test Case Specifications | Representative scenarios for evaluation. | 
| Improvement Findings | Combined list of issues and opportunities. | 
| Improved TTML Design | Planned revised module structure. | 
| Draft Improved TTML | Candidate improved module. | 
| Comparison Report | Comparison between original and improved versions. | 
| Final Improved TTML | Final revised module. | 
| Improvement Summary | Human-readable explanation of changes. | 
## Transformation Flow 
```text 
original_ttml + project_completion_operation + evaluation_criteria 
↓ 
ExtractOperationRequirements 
↓ 
operation_requirements 
original_ttml + schema 
↓ 
ValidateOriginalTTMLXSD 
↓ 
original_validation_report_xsd 
original_ttml + executor 
↓ 
ValidateOriginalTTMLExecutor 
↓ 
original_validation_report_executor 
operation_requirements + original_ttml + validation reports 
    ↓ 
AnalyzeOriginalTTMLDesign 
    ↓ 
original_ttml_design_review 
 
operation_requirements + evaluation criteria 
    ↓ 
GenerateTestCaseSpecifications 
    ↓ 
test_case_specifications 
 
original_ttml + test_case_specifications 
    ↓ 
ReviewOriginalAgainstTestCases 
    ↓ 
test_case_reviews 
 
validation reports + design review + test case reviews 
    ↓ 
CompileImprovementFindings 
    ↓ 
improvement_findings 
 
improvement_findings + schema + function definitions 
    ↓ 
DesignImprovedTTML 
    ↓ 
improved_ttml_design 
 
improved_ttml_design 
    ↓ 
DraftImprovedTTML 
    ↓ 
draft_improved_ttml 
 
draft_improved_ttml 
    ↓ 
Validate + Compare + Revise 
    ↓ 
improved_ttml 
    ↓ 
improvement_summary 
``` 
 
## Why This Is Important 
 
This example demonstrates a key strategic property of the framework: 
 
> Thought Tree workflows can be reviewed, tested, revised and improved using the same framework 
they define. 
 
This makes possible: 
- module quality improvement; - module library maintenance; - regression testing; - automated refactoring; - organisational workflow evolution; - comparison between versions; - continuous improvement of cognitive processes. --- 
# 9. Pattern Summary 
Across the examples, several reusable patterns appear. 
## 9.1 Draft → Review → Revise 
Used in: - article summary; - novel writing; - technical documentation; - compliance reporting. 
Pattern: 
```text 
source 
↓ 
draft 
↓ 
review 
↓ 
revision 
↓ 
final output 
``` 
This is one of the most important cognitive programming patterns because it makes quality control 
explicit. 
--- 
## 9.2 Extract → Structure → Synthesize 
Used in: - legacy project recovery; - technical design document generation; - compliance analysis. 
Pattern: 
```text 
unstructured source material 
↓ 
extracted facts / requirements / obligations 
↓ 
structured intermediate artefacts 
↓ 
synthesised final document 
``` 
This is useful when the source material is messy, incomplete or too large for direct final-output 
generation. 
--- 
## 9.3 Iterate → Aggregate → Transform 
Used in: - chapter drafting; - feedback simulation; - test case generation; - batch content generation. 
Pattern: 
```text 
item_1 
item_2 
item_3 
... 
↓ 
per-item transformation 
↓ 
collection 
↓ 
aggregate transformation 
``` 
This pattern is supported by Iterators and Collections. 
--- 
## 9.4 Generate → Validate → Repair 
Used in: - TTML authoring; - module improvement; - structured document generation. 
Pattern: 
```text 
generated artefact 
↓ 
validation 
↓ 
correction plan 
↓ 
revised artefact 
``` 
This is essential when LLMs are asked to produce structured outputs such as XML, JSON, code or 
formal documents. 
--- 
## 9.5 Plan → Approve → Execute 
Used in: - dynamic module generation; - high-impact compliance workflows; - large production pipelines. 
Pattern: 
```text 
ambiguous task 
↓ 
execution plan or generated module 
↓ 
validation / human approval 
↓ 
execution 
``` 
This reduces the risk of allowing dynamic LLM planning to directly execute without inspection. 
--- 
# 10. What the Examples Demonstrate 
Together, these examples show that Thought Tree programs can support a wide range of cognitive 
work: 
| Capability | Demonstrated By | 
|---|---| 
| Simple LLM workflow structuring | Article summary | 
| Intermediate artefact preservation | Legacy project recovery | 
| Large-scale creative production | Novel development | 
| Structured technical synthesis | TDD generation | 
| Auditability and human review | Compliance analysis | 
| Scheduled/cyclic execution | Research monitoring | 
| Self-authoring | Module generation | 
| Self-improvement | Module improvement | 
They also show the main distinction between Thought Tree and ordinary LLM prompting: 
> A Thought Tree does not only describe the desired output. It describes the cognitive production 
process that should create, review, validate and preserve that output. 