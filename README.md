# Contribution 1: Adapter: Mistral AI Model Provider

**Contribution Number:** 1
**Student:** Prajan Manoj Kumar Rekha
**Issue:** https://github.com/orthogonalhq/nous-core/issues/308
**Status:** Phase I Complete

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
A mistral-provider.ts file should exist in
self/subcortex/providers/src/ that implements IModelProvider with
invoke, stream, and getConfig, validates input against
TextModelInputSchema, handles streaming correctly, and is exported
from the providers package index.

### Current Behavior
The file does not exist. Mistral is not a usable provider in the
system.

### Affected Components
- self/subcortex/providers/src/mistral-provider.ts (to be created)
- self/subcortex/providers/src/index.ts (export to be added)
- self/subcortex/providers/src/__tests__/ (tests to be added)
- self/shared/src/interfaces/subcortex.ts (contract to implement)

---

## Reproduction Process

### Environment Setup
To be completed in Phase II.

### Steps to Reproduce
1. Clone nous-core
2. Attempt to configure Mistral as a model provider
3. No provider implementation exists

### Reproduction Evidence
- **Commit showing reproduction:** To be added in Phase II
- **Screenshots/logs:** N/A, this is a missing feature not a bug
- **My findings:** To be completed in Phase II

---

## Solution Approach

### Analysis
Mistral uses an OpenAI-compatible /v1/chat/completions API with Bearer
token auth. The implementation should closely follow openai-provider.ts
with Mistral-specific base URL and auth configuration.

### Proposed Solution
Create MistralProvider implementing IModelProvider, using
openai-provider.ts as the primary reference for API shape and
ollama-provider.ts for streaming patterns.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** A Mistral AI provider is missing from nous-core. I need
to create mistral-provider.ts that implements the IModelProvider
contract.

**Match:** openai-provider.ts is the closest reference since Mistral
uses an OpenAI-compatible API. ollama-provider.ts demonstrates the
streaming implementation pattern.

**Plan:**
1. Read openai-provider.ts, ollama-provider.ts, and
   anthropic-provider.ts fully
2. Read IModelProvider in self/shared/src/interfaces/subcortex.ts
3. Create mistral-provider.ts implementing invoke()
4. Implement stream() with correct async iterable pattern
5. Implement getConfig() returning provider metadata
6. Add input validation against TextModelInputSchema
7. Export from self/subcortex/providers/src/index.ts
8. Write tests in self/subcortex/providers/src/__tests__/
9. Run pnpm typecheck, pnpm lint, pnpm test

**Implement:** To be added in Phase II

**Review:** pnpm typecheck, pnpm lint, and pnpm test must all pass
before submitting PR

**Evaluate:** Tests will cover invoke, stream, getConfig, and input
validation error cases

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
To be completed in Phase II.

---

## Implementation Notes

### Week 1 Progress
To be completed in Phase II.

### Week 2 Progress
To be completed in Phase II.

### Week 3 Progress
To be completed in Phase II.

### Week 4 Progress
To be completed in Phase II.

### Code Changes
- **Files modified:** To be added in Phase II
- **Key commits:** To be added in Phase II
- **Approach decisions:** To be added in Phase II

---

## Pull Request
**PR Link:** To be added
**PR Description:** To be added
**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** Not
