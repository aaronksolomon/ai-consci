# Repository guidance

This is a research scaffold, not a demonstration that AI is or is not conscious. Read [README](README.md), the [charter](docs/charter.md), and the [claims map](docs/claims-and-evidence.md) before substantive changes.

## Epistemic standards

- Label substantive claims as **Empirical**, **Theoretical**, **Interpretive**, or **Analogical**, using the definitions in the claims map. Distinguish an external finding from a result produced here.
- Use **Draft**, **Frozen**, **Completed**, and **Superseded** for protocol lifecycle; use **Template** for an unfilled case study and **Undecided** for unresolved choices. A document may contain a proposed hypothesis without containing a result.
- Never fabricate measurements, runs, quotations, citations, participant statements, or proof steps. Mark missing evidence explicitly. Report failures, null outcomes, exclusions, and protocol deviations.
- Do not infer subjective experience, or its absence, from performance, fluent self-report, architecture labels, or philosophical resemblance alone. Name the extra theoretical assumptions behind any bridge between them.
- Treat “frozen left hemisphere” as an exploratory analogy. Do not turn human hemisphere research into a model identity. Do not equate a shared board with global neuronal workspace theory.
- Attribute Buddhist interpretations to a text, translator, teacher, or this project's own reading. Do not invent Thích Nhất Hạnh quotations or treat a publisher's description as a verified passage from a book.

## Sources and provenance

Prefer primary texts, original papers, author manuscripts, and official teaching archives. Record author, title, date or version, stable URL/DOI, locator when available, and what was actually checked. Distinguish metadata/abstract checks from full-text review. Keep paraphrases separate from quotations; quotations require exact wording and a page, section, or timestamp. Record disagreements and source limitations in [sources](docs/sources.md).

For future runs, preserve protocol commit, task and prompt hashes, model/version, settings, seed where supported, budgets and actual usage, message order, tool outputs, human interventions, verifier version, scores, and exclusions. Do not request or claim access to hidden model reasoning; record exposed messages and checkable work products.

## Privacy and scope

- This repository is public. Never copy private ChatGPT transcripts, credentials, personal data, or private/unpublished Collatz artifacts into it. Public availability of related work does not clear private material.
- Evidence needs explicit selection and permission covering contributors, quoted passages, and artifacts. Redaction alone is not permission. Keep uncleared material outside the checkout; `.gitignore` is only a guardrail.
- The initial authorization covers documentation and scaffolding. Do not install models, launch experiments, spend on API runs, recruit participants, or begin training without a separate explicit instruction establishing scope and budget.
- Keep initial tasks synthetic or appropriately licensed, bounded, independently checkable, and free of personal data. New human-participant or sensitive-data work needs a separate protocol and consent plan.
- Do not change visibility, repository settings, or licensing without explicit authorization. Preserve user files and concurrent changes; do not force-push or rewrite history.

## Editing and validation

Keep the structure lean. Prefer a clear protocol over a generic framework. Check relative Markdown links and anchors, status-label consistency, factual attribution, and staged content for secrets or private information. Run `git diff --check` (and `git diff --cached --check` before a commit). Run implementation checks only when relevant code exists. Before pushing, inspect remote changes and use an ordinary fast-forward push; reconcile concurrent work without overwriting it. Verify the remote commit and report configured checks accurately, including when none exist.
