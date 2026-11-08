# Contributing

Keep changes focused on the skill's stated purpose: explicit-invocation-only, static, read-only audits of limits and scaling assumptions.

## Proposing changes

Explain the problem, the expected behavior, and the relevant instruction or example. Use synthetic excerpts or material you have permission to share. Remove credentials, private paths, and confidential project details.

Treat [SKILL.md](SKILL.md) as the authoritative instruction set. Keep the English and Russian READMEs consistent with it and preserve English pattern names and recommendation action labels in both languages.

## Review checks

- Preserve YAML metadata, including explicit invocation settings; keep the skill folder name aligned with `name` when installing.
- Check Markdown links and consistency between the skill and its documentation.
- Preserve the distinction between source evidence, proposed checks, prior results, and checks actually executed.
- Review activation examples and near misses: an explicit named invocation should activate the audit; a generic review request or discussion of the skill should not.
- For behavior changes, document the evaluation method and observed results. Use separately authorized, disposable fixtures; do not run an audit against live resources to evaluate the skill.

This repository contains instructions and documentation, with no application build or automated test suite. Do not describe documentation checks as proof of runtime behavior.
