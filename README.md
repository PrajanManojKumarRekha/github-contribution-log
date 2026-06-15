# Contribution 1: Adapter: Mistral AI Model Provider

**Contribution Number:** 1
**Student:** Prajan Manoj Kumar Rekha
**Issue:** https://github.com/orthogonalhq/nous-core/issues/308
**Status:** Phase II In Progress

---

## Why I Chose This Issue

I chose this issue because it aligns with LLM provider work I have
already done. In my GSSoC '26 contribution to InterXAI, I built a
fallback LLM provider system using LiteLLM, with Groq as primary and
Gemini as fallback. That required understanding provider interfaces,
streaming responses, and error handling, which is exactly what this
issue involves.

The issue is well scoped with clear acceptance criteria, named
reference files to follow, and an explicit contract to implement. The
blocking PR #324 has already merged so there are no dependencies
blocking the work.

---

## Understanding the Issue

### Problem Description
nous-core has an IModelProvider interface that model backends must
implement to integrate with the system. There is no Mistral AI
provider yet, so users cannot use Mistral models through nous-core.

### Expected Behavior
A mistral provider should exist under
self/subcortex/providers/src/providers/mistral/ following the same
certified provider leaf structure as the Anthropic provider, with
adapter.ts, definition.ts, implementation.ts, index.ts, and
provider.ts files, implementing IModelProvider with invoke, stream,
and getConfig, and exported from the providers package index.

### Current Behavior
The mistral directory does not exist under
self/subcortex/providers/src/providers/. Mistral is not a usable
provider in the system.

### Affected Components
- self/subcortex/providers/src/providers/mistral/ (to be created)
- self/subcortex/providers/src/providers/mistral/adapter.ts
- self/subcortex/providers/src/providers/mistral/definition.ts
- self/subcortex/providers/src/providers/mistral/implementation.ts
- self/subcortex/providers/src/providers/mistral/index.ts
- self/subcortex/providers/src/providers/mistral/provider.ts
- self/subcortex/providers/src/__tests__/ (tests to be added)
- self/shared/src/interfaces/subcortex.ts (contract to implement)

---

## Reproduction Process

### Environment Setup
Cloned the fork and added the upstream remote to access the
feat/contributor-friendly-inference-provider-surface branch which is
not included by default in a fork. Fetched upstream branches and
pushed the feature branch to the fork manually before creating the
working branch from it.

The repo was initially cloned inside an OneDrive synced directory
which can interfere with Node.js tooling on Windows. Moved awareness
of this as a risk for future steps.

pnpm was already available at version 10.6.2. Running pnpm install,
pnpm build, and pnpm test all completed successfully on the feature
branch with no errors.

OS: Windows 11
Node.js: 22+
pnpm: 10.6.2
Base branch: feat/contributor-friendly-inference-provider-surface
Working branch: feature-mistral_adapter

### Steps to Reproduce
1. Clone nous-core and check out
   feat/contributor-friendly-inference-provider-surface
2. Navigate to self/subcortex/providers/src/providers/
3. Only anthropic, ollama, and openai directories exist
4. No mistral directory or implementation exists

### Reproduction Evidence
- **Commit showing reproduction:** To be added in Phase III
- **Screenshots/logs:** Confirmed via directory listing showing only
  anthropic, ollama, and openai under providers/
- **My findings:** The certified provider leaf structure requires five
  files per provider: adapter.ts, definition.ts, implementation.ts,
  index.ts, and provider.ts. This structure is confirmed by inspecting
  the Anthropic reference implementation.

---

## Solution Approach

### Analysis
Mistral uses an OpenAI-compatible /v1/chat/completions API with Bearer
token auth. The maintainer confirmed in issue comments that this
provider should ship as a config entry against the shared
ChatCompletionsProvider primitive introduced in the refactored adapter
surface. The implementation follows the certified provider leaf
structure with five files mirroring the Anthropic reference.

### Proposed Solution
Create self/subcortex/providers/src/providers/mistral/ with the same
five-file structure as the Anthropic provider, implementing
IModelProvider using Mistral's OpenAI-compatible endpoint and Bearer
token authentication.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** A Mistral AI provider is missing from nous-core. The
certified provider leaf structure requires five files per provider
directory. I need to replicate this structure for Mistral using its
OpenAI-compatible API.

**Match:** The Anthropic provider under
self/subcortex/providers/src/providers/anthropic/ is the primary
reference with five files: adapter.ts, definition.ts,
implementation.ts, index.ts, and provider.ts. Mistral's
OpenAI-compatible API shape means openai/adapter.ts is also relevant
for protocol-level implementation details.

**Plan:**
1. Read all five Anthropic provider files fully to understand the
   certified leaf structure
2. Read openai provider files for OpenAI-compatible protocol patterns
3. Read IModelProvider in self/shared/src/interfaces/subcortex.ts
4. Create self/subcortex/providers/src/providers/mistral/
5. Create definition.ts with Mistral provider metadata and config
6. Create implementation.ts with invoke() and stream() against
   Mistral's /v1/chat/completions endpoint
7. Create adapter.ts wiring implementation to IModelProvider contract
8. Create provider.ts as the provider entry point
9. Create index.ts exporting the provider
10. Write tests in self/subcortex/providers/src/__tests__/
11. Run pnpm typecheck, pnpm lint, pnpm test

**Implement:** https://github.com/PrajanManojKumarRekha/nous-core/tree/feature-mistral_adapter

**Review:** pnpm typecheck, pnpm lint, and pnpm test must all pass
before submitting PR. PR targets
feat/contributor-friendly-inference-provider-surface not main or dev.

**Evaluate:** Tests will cover invoke, stream, getConfig, input
validation error cases, and auth error handling.

---

## Testing Strategy

### Unit Tests
- [ ] Test case 1: invoke() returns correct ModelResponse for valid input
- [ ] Test case 2: stream() yields correct ModelStreamChunk sequence
- [ ] Test case 3: getConfig() returns correct provider metadata
- [ ] Test case 4: Input validation rejects invalid TextModelInput
- [ ] Test case 5: Auth errors handled gracefully

### Integration Tests
- [ ] Integration scenario 1: End-to-end invoke against Mistral API
- [ ] Integration scenario 2: Streaming response from Mistral API

### Manual Testing
To be completed in Phase III.

---

## Implementation Notes

### Week 1 Progress
Set up local development environment on Windows 11. Installed pnpm
10.6.2 and confirmed pnpm install, pnpm build, and pnpm test all pass
on the feat/contributor-friendly-inference-provider-surface branch.
Added upstream remote to fork and pushed feature branch. Created
working branch feature-mistral_adapter from the correct base branch.
Confirmed certified provider leaf structure by inspecting Anthropic
reference implementation which contains adapter.ts, definition.ts,
implementation.ts, index.ts, and provider.ts.

### Week 2 Progress
To be completed in Phase III.

### Week 3 Progress
To be completed in Phase III.

### Week 4 Progress
To be completed in Phase III.

### Code Changes
- **Files modified:** To be added in Phase III
- **Key commits:** To be added in Phase III
- **Approach decisions:** To be added in Phase III

---

## Pull Request
**PR Link:** To be added
**PR Description:** To be added
**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** Not yet submitted

---

## Learnings & Reflections

### Technical Skills Gained
To be completed after Phase III.

### Challenges Overcome
To be completed after Phase III.

### What I'd Do Differently Next Time
To be completed after Phase III.

---

## Resources Used
- https://docs.mistral.ai/
- https://docs.nue.orthg.nl/docs/development/provider-adapters
- https://github.com/orthogonalhq/nous-core/blob/main/CONTRIBUTING.md
- Reference implementations: anthropic provider, openai provider,
  ollama provider under self/subcortex/providers/src/providers/
