# Boundary Assumption Audit

[Русская версия](README.ru.md)

A skill for static, read-only audits of limits and scaling assumptions in small and medium software projects. Finds risks such as truncated results, conflicting size limits, resource exhaustion, and fragile environment assumptions. Reports evidence from source code and recommends focused remedies without running project code or tests, or modifying the audited project. Runs only when explicitly invoked.

- **Repository name:** `boundary-assumption-audit`
- **Skill name:** `boundary-assumption-audit`
- **Skill instructions:** [SKILL.md](SKILL.md)
- **License:** [MIT](LICENSE)

## What it examines

- Result clipping and premature termination: pagination, query limits, slices, traversal and iteration caps.
- Resource handling: allocations, queues, concurrency, retries, timeouts, and cancellation.
- Conflicting contracts: accepted versus stored sizes, producer versus consumer limits, defaults, and units.
- Boundary and environment assumptions: overflow, sentinel values, empty input, and single-worker or host assumptions.

The audit follows specific suspected failures. It preserves justified product, protocol, resource, and security limits and does not attempt to catalog every numeric literal.

## Design patterns and principles

These describe the skill's audit workflow and authority boundaries.

| Pattern / principle | Application |
|---|---|
| Explicit Activation | Requires an explicit invocation; generic review or debugging requests do not activate the audit. |
| Least Privilege | Permits passive inspection and report delivery outside the audited target. |
| Progressive Refinement | Screens relevant paths first, then traces specific suspected failures. |
| Contract Checking | Compares limits and assumptions across producers, consumers, storage, and configuration. |
| Evidence Traceability | Connects each finding to source locations, triggering conditions, consequences, and proposed verification. |
| Separation of Concerns | Keeps static findings, runtime validation, and implementation changes separate. |

## Installation in BB

Keep the skill files in a folder named `boundary-assumption-audit`, matching the `name` in the YAML frontmatter. Publish that folder through BB's Agent Tools plugin:

```text
bb tools skills-publish <path-to-boundary-assumption-audit> --dry-run
bb tools skills-publish <path-to-boundary-assumption-audit>
```

If BB cannot determine the source machine, add `--host <machine-id>` to both commands. Publishing installs the skill into BB's skills hub. Start a new thread to discover the installed revision; an existing thread may retain its earlier skill list.

The commands above require BB with the Agent Tools plugin. They are installation instructions, not actions performed during an audit. Other skill hosts require their own installation mechanism; compatibility has not been verified here.

## Usage

Explicitly invoke the skill by name and specify a repository or subsystem, optionally with a focus:

```text
Use boundary-assumption-audit for /path/to/project.
Focus on pagination and producer/consumer size limits.
```

```text
Используй boundary-assumption-audit для /path/to/project.
Особое внимание: повторные попытки, тайм-ауты и отмена операций.
```

Without a supplied path, the skill uses the active repository. Reading or discussing the skill does not start an audit.

## Output

Each completed audit produces a concise findings table in chat and a downloadable Markdown report with evidence and recommendations. Both use the conversation language. Reports default to UTF-8 with BOM and are saved outside the audited repository.

Findings include source references, severity, confidence, triggering conditions, consequences, and proposed verification scenarios. Primary recommendations use one of these action labels: `ELIMINATE`, `REDESIGN`, `DERIVE`, `CONFIGURE`, `ALIGN`, `KEEP`, `VERIFY`.

Unsupported concerns remain unresolved questions. If no actionable findings are identified, the report states that conclusion only for the inspected scope.

## Execution boundaries

The audit reads source, configuration, tests as text, and existing evidence. It does not run target code, tests, builds, linters, migrations, or dependency installation; it does not connect to target databases or operate live services, devices, or UI. Proposed checks are reported as not executed.

These are instructions to the agent, not an operating-system sandbox. Static evidence does not establish runtime acceptance or prove the absence of defects. Report delivery depends on the host's attachment or download capabilities; delivery failures must be disclosed.

## Repository contents

| File | Purpose |
|---|---|
| [SKILL.md](SKILL.md) | Authoritative agent instructions and report requirements. |
| [README.md](README.md) | English overview, installation, and usage. |
| [README.ru.md](README.ru.md) | Russian overview, installation, and usage. |
| [LICENSE](LICENSE) | MIT license. |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution and review guidance. |
| [docs/PUBLISHING.md](docs/PUBLISHING.md) | Publication metadata and pre-publication review notes. |

## License

Distributed under the [MIT License](LICENSE).
