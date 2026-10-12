# How OpenAI Codex Was Used on StudyMaster

This document records the role of OpenAI Codex in the development of StudyMaster. It exists so a
reviewer can distinguish genuine AI-assisted engineering from a generic “built with AI” badge.

## Collaboration model

The product owner supplied the problem, Nigerian education context, product direction, user feedback,
acceptance decisions and hands-on visual testing. The platform was developed across multiple Codex
sessions. Within those sessions, primary Codex agents inspected and modified the repositories,
researched APIs, proposed architecture, implemented features, diagnosed failures and coordinated
multiple specialist subagents for parallel research, implementation, testing and review.

The owner remained the product decision-maker. Codex operated as the engineering collaborator.

## Where Codex contributed

### Product decomposition

Large changes were separated into reviewable phases: question collection and data preparation,
versioned notes, note narration, removal of the old embedded live-teach control, and then the
standalone Live Learning classroom. Each phase had a concrete outcome and verification gate. Primary
agents split independent work across subagents and synthesized the results into one coherent product.

### The 23,355-question corpus

Codex helped build and run the end-to-end question-data workflow across earlier StudyMaster sessions:

1. scrape more than 23,000 examination questions from source material;
2. clean malformed, duplicated and inconsistent records;
3. normalize examination, subject and topic fields;
4. generate curriculum-aware topic labels;
5. embed question text together with its label using 256-dimensional OpenAI embeddings;
6. create the MongoDB Atlas vector index;
7. combine exact matches, vector neighbours and cached keyword expansion;
8. make the search degrade safely when an AI or vector service is unavailable;
9. connect retrieved questions to notes, practice and explanations.

The original scraping and cleaning tools evolved across sessions and are not all retained in the
current repository. The surviving productization scripts are `label_topics.py`,
`embed_questions.py`, `create_vector_index.py` and `normalize_topics.py` in the server repository.

### Notes and narration

Codex helped implement note history as real versions rather than a dropdown that silently replaces
content. Follow-up instructions receive previous-note context, can advance the lesson rather than
repeat it, and preserve links between chat turns and the versions they created. Codex also integrated
OpenAI speech synthesis with the existing word/segment highlighting behavior.

### Live Learning

Codex researched OpenAI's current live-audio and delegation architecture and built the classroom
across both repositories. Work included lesson-notebook preparation, WebRTC negotiation, transcript
handling, typed questions, microphone control, timer/extension behavior, pause/resume, board
operations, LaTeX rendering, quizzes and persisted lesson state.

The board-scribe architecture was designed to prevent “two minds”: the visual board is derived from
what the live teacher actually says. OpenAI Decisions performs a bounded classification, while only
symbolic speech needs an additional notation conversion.

### Product and interaction quality

Codex translated the owner's reference image into the StudyMaster visual language, generated the owl
state artwork, implemented responsive layouts and created distinct listening, thinking, speaking and
writing states. The owner performed the final authenticated visual acceptance testing.

### Debugging and verification

Codex diagnosed Google OAuth origin issues, port conflicts, invalid model-output shapes and live-board
fallback behavior. Verification included Next.js production builds, Python compilation, Ruff checks,
route and persistence tests, real Decisions requests, GPT-Live endpoint access checks and published
repository scans.

## Custom Codex skills

The owner created a reusable instruction system around Codex rather than relying on one-off prompts.
Examples include:

- **phased product build** — turns large ideas into reviewable releases;
- **website design quality** — preserves the intended experience and requires UI verification;
- **production web motion** — guides purposeful animation and interaction work;
- **review and verification practices** — require functional evidence before completion;
- **repository working agreements** — protect existing code, explain architecture and keep the owner
  involved in high-impact decisions.

These skills are reusable development infrastructure. They allowed separate Codex sessions to work
with the same standards while primary agents orchestrated specialist subagents for research, data
work, implementation, testing and review. The primary agent remained responsible for integrating
their output, checking shared state and verifying the complete result with the owner.

This workflow matches OpenAI's documented model: skills provide reusable task instructions, while a
primary agent can delegate independent work to focused subagents and combine their results. See the
[OpenAI Skills documentation](https://developers.openai.com/api/docs/guides/tools-skills) and
[OpenAI Multi-agent documentation](https://developers.openai.com/api/docs/guides/agents-api/multi-agent).

## Human and Codex responsibilities

| Product owner | OpenAI Codex |
|---|---|
| Defined the student problem and product vision | Converted requirements into architecture and coordinated implementation |
| Supplied user feedback and reference experiences | Researched APIs and technical trade-offs |
| Chose feature priorities and accepted behavior | Orchestrated agents and subagents across client, server and data work |
| Performed authenticated visual and voice testing | Ran automated builds, service checks and diagnostics |
| Owns the curriculum mission and final decisions | Documented the system and surfaced limitations honestly |

## Why this matters

Codex did not replace product judgment. It shortened the distance between an ambitious education idea
and a tested implementation while preserving a clear human decision-maker, an inspectable codebase
and explicit verification evidence.
