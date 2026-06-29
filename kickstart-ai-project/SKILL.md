---
name: kickstart-ai-project
description: Lightweight requirements kickoff for small/medium software projects (internal tools, utilities, scripted workflows, AI-assisted apps that consume models rather than train them). Use when the user wants to scope a new small project, write a quick spec, or "kickstart" something that does not warrant the full /start-ai-project framework. Triggers include "kickstart", "new tool", "small project", "internal tool spec", "draft requirements for".
---

# Kickstart AI Project

A lean requirements pass for small/medium projects, stripped to what a single-developer or small-team internal tool actually needs. Skip phases that don't apply — note explicitly when you skip one and why.

If the project will train or fine-tune a model, has regulated decision-making, or is safety-critical, **stop and tell the user to use `/start-ai-project` instead**.

## Workflow

Walk the user through each phase in order. Ask questions, capture answers, then write the structured spec at the end. Do not skip silently — say "skipping X because Y".

### Phase 1 — Problem & Scope

1. One- or two-sentence problem statement. What real-world outcome does this tool produce?
2. What's **inside** the system vs. **outside**? List external systems/services it talks to (databases, APIs, file shares, email, an LLM endpoint, etc.).
3. What inputs does it consume? What outputs does it produce? Who/what triggers it?

A Work Context Diagram is optional — only sketch one if the boundary is non-obvious.

### Phase 2 — Users & Data Sources

1. Who are the users? (1–3 short bullets per user type — role, primary goal, technical comfort.)
2. What data sources does it read or write? For each: who owns it, is it sensitive (PII, commercial-confidential, regulated), and is read/write access already in place?
3. If the tool calls an LLM or external AI API: which provider/model, where do prompts/data go, and are there data-residency concerns?

### Phase 3 — Success Measures

Define just two layers:

| Layer | Question |
|-------|----------|
| **User outcome** | How is the user better off? (Time saved, fewer errors, decisions unblocked.) Pick a measure even if rough. |
| **Acceptance criteria** | What concrete, testable conditions mean v1 is "done"? List 3–7. |

Skip organizational objectives and leading indicators unless the user explicitly cares.

### Phase 4 — Functional Requirements

Numbered list of what the system **shall do**. Keep them implementation-free, testable, and unambiguous (no "usually/typically"). Use the form: `FR-1: The system shall <verb> <object> <condition>.`

### Phase 5 — Non-Functional Requirements

Cover only what applies. For each chosen category, write 1–3 concrete requirements:

- **Usability** — who runs it, where, how often. CLI vs. UI vs. scheduled job.
- **Performance** — acceptable latency / throughput / batch size for the realistic case, not the worst case.
- **Security & access** — credentials handling, who can run it, audit logging if needed.
- **Maintainability** — language/runtime, where it lives (repo, server, scheduled task), who maintains it.
- **Reliability** — what happens on partial failure (retry, skip, alert, manual rerun).

Skip fairness, explainability, and model-degradation NFRs unless an AI component makes a non-trivial decision.

### Phase 6 — Constraints

- Tech stack mandates (language, framework, database, hosting).
- Integration constraints (must read from system X, must write to format Y).
- Compliance: only if personal/regulated data is touched — note GDPR/CCPA/internal policy and stop to flag if uncertain.
- Budget/time/compute caps if any.

### Phase 7 — Risks & Mitigations

Replace formal Fault Tree Analysis with a small risk register. For the top 3–7 risks:

| Risk | Likelihood × Impact | Mitigation |
|------|--------------------|-----------|

Include at least one risk for: bad input data, integration failure, and (if AI is in the loop) wrong model output. For each high-likelihood-or-high-impact risk, add a corresponding requirement back into Phase 4 or 5.

## Output

Compose a single Markdown spec note with these sections in order:

1. Problem & Scope
2. Users & Data Sources
3. Success Measures (user outcome + acceptance criteria)
4. Functional Requirements (numbered)
5. Non-Functional Requirements (by category, only those used)
6. Constraints
7. Risks & Mitigations
8. Open Questions (anything the user couldn't answer yet)

Ask the user where to save it. Default suggestion: `Projects/<Project_Name>/Requirements.md` in TitleCase_Underscore per the vault's filename convention. Do not save without confirmation.

## Review checklist before handing off

- [ ] Each requirement is testable and implementation-free.
- [ ] Every external system/API the tool depends on is named.
- [ ] At least one risk + mitigation pair exists.
- [ ] Skipped phases are listed with a one-line reason.
- [ ] Open questions are captured rather than guessed.
