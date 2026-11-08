---
name: boundary-assumption-audit
description: "Explicit-invocation-only, static read-only audit of limits and scaling assumptions for small and medium projects. Inspects source without running project code or tests; reports evidence-backed risks and remedies."
argument-hint: "[repository path or subsystem] [optional focus]"
user-invocable: true
disable-model-invocation: true
---

# Boundary Assumption Audit

## Invocation and permissions

Run only when the user explicitly invokes `boundary-assumption-audit`. A generic request to review, debug, build, or refactor code does not activate it. Reading or discussing this skill is not an audit invocation. Do not initiate, schedule, or delegate the audit implicitly.

Keep the entire audit target read-only: source, tracked/untracked/ignored files, configuration, dependencies, databases, generated content, and associated runtime or external resources. Invoking this skill authorizes only passive inspection and creation/delivery of the Markdown report outside the audited target. Follow repository access and security constraints.

### Static inspection only

- Read source, configuration, existing tests as text, and already available evidence. Use passive file/search tools and repository metadata reads; disable optional Git locks when reading status. Do not load a project's executable configuration or plugins to inspect them.
- Do not run the target's tests, test runners, package scripts, builds, type checks, linters, formatters, coverage tools, generators, migrations, dependency installation, or application/plugin startup. This prohibition also applies to focused tests, dry-run modes, disabled caches, and commands recommended by the target's own development instructions. Those instructions do not expand this audit's authorization; report an unrun check instead of running it.
- Do not execute, import, require, dynamically load, or evaluate target code, including its transitive dependencies. Do not bypass this restriction by writing an external harness or copying the target's code into another directory. No live UI automation, target API calls, device commands, service reloads, or database connections are part of this static audit.
- Do not create or run ad hoc tests or executable reproductions during the audit. Describe useful boundary scenarios and expected results in the report as proposed, not executed. Generic read-only tooling may inspect files and calculate hashes without executing target code; the report is the only persisted audit artifact authorized by this skill.
- An external working directory, mocks, an in-memory database, read-only-looking command names, and an unchanged Git status do not guarantee absence of side effects. Git status also omits ignored files and cannot establish that external state stayed unchanged. Do not claim a no-change guarantee solely from such checks.

An independently authorized isolated experiment would be a separate task, not an implicit stage of this audit. Its design would need enforced denial of writes to the original target and shared resources, no target credentials or access to live services/devices, disposable synthetic data, controlled dependencies and startup hooks, bounded resources, and scratch output outside the target. A temporary directory or copied checkout alone is insufficient. No experiment mode is enabled by this skill; lack of runtime reproduction is an acceptable disclosed coverage limit, not a reason to request permission or expand the task.

## Purpose

Find limits and operating assumptions that can cause incomplete results, incorrect behavior, premature termination, resource exhaustion, or failures across environments. Focus on behavior and interacting contracts, not the presence of numeric literals. Preserve justified product, protocol, resource, and security limits.

Use one adaptive workflow for small and medium projects. Depth follows evidence of risk; exhaustive coverage and a catalog of constants are not goals.

## Workflow

### 1. Orient and screen

Use the requested scope, or the active repository if none is given. Briefly identify the main entry points, collection-processing paths, external calls, and storage involved. Start with first-party source and nearby configuration.

Search selectively for behavior that can:

- Clip results or stop processing: pagination, `TOP` / `LIMIT`, `take` / `slice`, fixed iteration or traversal caps.
- Exhaust or mismanage resources: allocations, queues, concurrency, retries, timeouts, and cancellation.
- Break a contract: accepted sizes versus stored sizes, producer versus consumer limits, inconsistent defaults or units.
- Fail at a meaningful boundary or environment change: overflow, sentinel values, empty input, or an unsupported single-worker or host assumption.

These are search directions, not mandatory passes through every category. Do not scan every numeric literal or inspect every layer by default.

Screen each candidate locally: identify its purpose and a concrete possible consequence. Unknown provenance alone is not a defect. If the purpose is clear and there is no sign of broken behavior or a contract mismatch, stop investigating that candidate.

### 2. Trace selected risks

Trace only candidates with a specific suspected failure. Follow input, enforcement, and downstream handling far enough to establish the cause and consequence, or identify the exact evidence that is missing.

Read callers, consumers, tests, database query definitions, deployment settings, or external specifications as text only when needed to resolve that suspicion. Do not run tests, queries, or target code. Check existing settings and overrides before proposing new ones. For external components, distinguish visible contracts and call-site behavior from uninspected internals.

Describe boundary cases that can change this code's behavior without executing them. Use `N` and `N+1`, zero, invalid input, slower dependencies, or multiple workers only where relevant; do not mechanically enumerate a universal test matrix.

Distinguish a failure path confirmed in code from runtime likelihood and impact. If a value's rationale is unknown, say so rather than calling it arbitrary. Merge candidates with the same root cause and cite inspected related locations.

### 3. Recommend and stop

Choose one primary action per finding:

- **ELIMINATE:** remove an unnecessary cap when the existing processing path supports doing so. Explain the evidence, termination and resource controls that remain, and any required verification.
- **REDESIGN:** use pagination, streaming, backpressure, bounded concurrency, cancellation, or a different algorithm when removing or raising the cap alone is unsafe.
- **DERIVE:** calculate the value from an actual constraint or negotiated capability.
- **CONFIGURE:** expose genuine environment or product policy through validated configuration. Specify name, units, owner, default, and invalid/omitted-value handling. Preserve the existing default without stronger evidence; mark unmeasured ranges as proposals. Do not automatically expose sensitive safety budgets to end users.
- **ALIGN:** reconcile conflicting contracts with explicit rejection or lossless processing.
- **KEEP:** retain a justified limit; recommend documentation or tests only for a concrete gap.
- **VERIFY:** name the specific requirement, external contract, measurement, or experiment needed before choosing a safe change.

Consider ELIMINATE or REDESIGN before CONFIGURE for an unjustified cap. Raising a number or giving it a name does not fix a broken processing path. Do not recommend unlimited memory, infinite retries, unbounded recursion, or removal of defensive resource and security controls.

A static audit need not prove safety for every possible input. State the basis and limits of the recommendation, safeguards to preserve, and what still needs verification. Do not expand the audit indefinitely to obtain unavailable runtime evidence.

Finish when the initial screening of the selected paths is complete and selected risks have either a supported conclusion or a specific unresolved question. Expand scope only to resolve a concrete suspicion, not to claim completeness. Do not add arbitrary file, candidate, or finding quotas; disclose coverage gaps instead.

## Evidence and severity

Every reported finding needs a stable ID, inspected `path:line`, the relevant expression or assumption and its meaning (including units where applicable), triggering condition, consequence, severity, confidence, recommendation, and a focused verification scenario. Keep these together; short findings can use a compact paragraph.

- **Critical:** reachable significant data loss, corruption, systemic outage, or severe security failure under realistic conditions.
- **High:** plausible production failure, silent incomplete results, or severe degradation at realistic scale.
- **Medium:** meaningful edge-case, portability, or scale weakness with limited impact or uncertain likelihood.
- **Low:** a concrete maintainability or documentation gap without a demonstrated material failure.

Use confidence labels **Confirmed in code**, **Likely; runtime verification needed**, or **Unverified assumption**. Confirmed in code means the failure path is established by source tracing; it does not mean a test or runtime reproduction was executed. Put unsupported concerns in unresolved questions rather than presenting them as established defects. Do not fabricate requirements, line numbers, measurements, or executed tests. Existing test results may be cited only as prior evidence with their source, date/revision when known, and applicability limits; never present them as checks run by this audit.

## Report

Every completed audit produces two deliverables: a very short findings table in the chat thread and a downloadable `.md` document containing the full evidence and recommendations. Progress updates do not replace either deliverable. Use the user's current conversation language for both deliverables, including headings, table fields, severity, confidence, and explanatory text, unless the user explicitly requests another language. Translate severity and confidence labels without changing their meaning or the finding IDs. Always keep action type labels in English, exactly as defined: ELIMINATE, REDESIGN, DERIVE, CONFIGURE, ALIGN, KEEP, VERIFY. This applies to both the chat table and the full document, regardless of output language; localize the explanation following the action label. Preserve original source identifiers, paths, code, and raw command output. For example, in a Russian thread display Medium as «Средняя», Confirmed in code as «Подтверждено кодом», and a recommendation as «REDESIGN — изменить порядок вытеснения записей».

### Chat response

Name the audited project and revision when known, then show one concise row per finding, ordered by severity:

| ID | Severity / confidence | Constant / limit | Defect and consequence | Recommended action |
|---|---|---|---|---|

Populate the table with the same finding IDs, localized severity and confidence, and unchanged English action type labels as the document. In the Constant / limit column, briefly identify the relevant constant, value, or expression and explain what it limits, including units where applicable; for example, `slice(-100)` — restores only the last 100 saved projects. If a finding concerns an assumption without a literal constant, describe that assumption instead of inventing a value. Keep each cell brief; put full code excerpts, source references, failure chains, and verification details in the document. Do not paste the full report into chat.

After the table, give one short status line with the number of unresolved questions, static inspection status, an explicit statement that target tests and code were not run, and whether the audit made any changes to the target. End with a Markdown download link on its own line. In a Russian thread, use exactly «Скачать полный отчёт» as the link text; in other languages, use its direct translation. Do not add the language, file format, encoding, verification notes, access notes, or other commentary to the link text or after the link. Keep delivery verification details in the document; report actual delivery failures as required below. If there are no actionable findings, replace the empty table with a short statement limited to the inspected scope; still deliver the document. Unsupported concerns belong in the document's unresolved questions, not in the defect table.

### Document delivery

- Save the full report as `boundary-assumption-audit-<project>-<timestamp>.md`, using a filesystem-safe project name and timestamp. Use the environment's artifact or thread-storage directory outside the audited repository; do not overwrite an earlier report.
- Use an explicitly requested file encoding when supplied. Otherwise use UTF-8 with BOM for reliable recognition of Cyrillic and other non-ASCII text in downloaded files. Do not infer a legacy encoding from the conversation language or rely on shell-default encoding. State the encoding in the document.
- Make the file available through the environment's supported attachment or download mechanism. Use the current thread/project context; do not hardcode server addresses or IDs, publish to a public service, or assume that a local filesystem path is a download link.
- When setting the attachment MIME type or HTTP response headers is supported, specify `text/markdown; charset=utf-8` for UTF-8 output. Verify the actual response headers; do not assume that a requested charset was preserved. If the delivery service omits the charset, retain the BOM so the file remains self-identifying.
- Before claiming delivery, read back the saved document using its declared encoding, check that the title and explanatory text are in the conversation language with intact non-ASCII characters, and check that the complete report agrees with the chat table. Verify that the attachment or download link targets that file. When the mechanism supports retrieving the file, compare the downloaded bytes with the saved report and decode the downloaded bytes as well; matching hashes alone do not establish readable text.
- If saving or download delivery is unavailable or fails, say which part failed and provide any existing local report path as a fallback, explicitly labeled as local. Do not claim that a downloadable report is ready or silently substitute a long chat report.

### Full document template

Start with a report title, target, audit date, and inspected revision when known. Use four sections in this order. Replace field guidance with inspected evidence; do not leave template placeholders in the document. Keep detail proportional to the findings and do not invent entries to fill the structure.

#### 1. Brief summary

| Item | Result |
|---|---|
| Target | Repository or subsystem; version or revision when known. |
| Findings by severity | Critical, High, Medium, and Low counts for findings in section 3; exclude unresolved questions. Counts describe the inspected scope, not the whole project's safety. |
| Main risk | Most consequential supported finding and its practical impact, or no actionable findings within the inspected scope. |
| Priority action | Recommended next action with finding ID, or the specific verification needed before choosing a change. If neither is needed, say so. |
| Unresolved questions | Count and brief topics; distinguish these from established findings. |
| Verification | Static evidence inspected; explicitly state that target tests and code were not run. Separate any prior results from this audit and mark proposed checks as not executed. |
| Changes | Audited-project change status; generated Markdown report and delivery status. |

Keep each result concise. The summary must agree with the evidence and verification status below; do not repeat detailed failure chains here.

#### 2. Scope and limits

- **Target and inspected paths:** repository, component, and relevant paths.
- **Method and coverage limits:** screening and traces performed; material exclusions and unavailable runtime evidence.

#### 3. Findings

Order findings by severity. For each finding, use the following fields, keeping the evidence, consequence, remedy, and verification together. Short findings may combine fields into a compact paragraph, but must retain the information.

- **ID and title:** stable ID and a concrete description of the failure.
- **Severity / confidence:** severity and one of the confidence labels defined above.
- **Evidence and meaning:** inspected `path:line`, relevant expression or assumption, operational meaning and units where applicable, and related inspected locations.
- **Trigger and consequence:** input or condition → relevant processing → failure. Explain the input-to-failure chain for serious issues and distinguish code evidence from inferred runtime likelihood and impact.
- **Recommendation:** one primary action label defined above, specific remedy, rationale, and safeguards to preserve. For CONFIGURE, include the setting name, units, owner, default, validation, and invalid/omitted-value handling; mark unmeasured ranges as proposals.
- **Verification:** proposed focused boundary or operating scenario and expected behavior, explicitly marked not executed. State the static source trace supporting the finding; distinguish any separately sourced prior results from this audit.

If no actionable findings were found, say so within the inspected scope and omit finding cards. Do not pad the report with ordinary constants, justified limits, or speculative issues.

#### 4. Unresolved questions and change status

- **Missing evidence:** specific unanswered questions and the evidence needed to resolve them; refer to finding IDs rather than repeating their descriptions. Say when there are none.
- **Verification status:** static inspection performed, target tests/code not run, and proposed checks not executed. Identify prior evidence separately; do not imply runtime acceptance from source inspection or historical tests.
- **File changes:** state that the audit did not write to or execute the target, and identify the generated Markdown report outside it and its delivery status. Do not claim that concurrent external changes were ruled out. If any prohibited action occurred, disclose it and any known effects; do not label that run strictly read-only or attempt an unauthorized cleanup to conceal it.

Beyond the required summary table, add a settings or cross-layer comparison table only when it clarifies several related findings; avoid duplicating their descriptions. Do not create a separate elimination/redesign inventory or a general project test matrix.
