# Contribution 1: Adapter: Mistral AI Model Provider

**Contribution Number:** 1
**Student:** Prajan
**Issue:** https://github.com/orthogonalhq/nous-core/issues/308
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it directly maps to work I've already done. 
In my GSSoC '26 contribution to InterXAI, I built a fallback LLM 
provider system using LiteLLM, with Groq as primary and Gemini as 
fallback. That work required understanding LLM provider interfaces, 
streaming responses, and error handling patterns — exactly what this 
issue asks for.

The nous-core adapter pattern is well-scoped: implement one interface 
(`IModelProvider`), follow existing reference implementations, write 
tests. The project has clear setup docs, explicit acceptance criteria, 
and reference files named directly in the issue. The blocking PR #324 
(OpenAiCompatibleProvider refactor) has already merged, so the 
implementation path is clear.

---

## Understanding the Issue

### Problem Description
The nous-core project has a `IModelProvider` interface that model 
backends must implement to integrate with the system. A Mistral AI 
provider does not yet exist, meaning users cannot use Mistral models 
through nous-core.

### Expected Behavior
A `mistral-provider.ts` file exists in 
`self/subcortex/providers/src/` that implements `IModelProvider` 
(invoke, stream, getConfig), validates input against 
`TextModelInputSchema`, handles streaming correctly, and is exported 
from the providers package index.

### Current Behavior
No Mistral provider exists. The file is missing entirely.

### Affected Components
- `self/subcortex/providers/src/mistral-provider.ts` (to be created)
- `self/subcortex/providers/src/index.ts` (export to be added)
- `self/subcortex/providers/src/__tests__/` (tests to be added)
- `self/shared/src/interfaces/subcortex.ts` (contract to implement)

---

## Reproduction Process

### Environment Setup
[To be filled during Phase II]

### Steps to Reproduce
1. Clone nous-core
2. Attempt to configure Mistral as model provider
3. No provider implementation exists

### Reproduction Evidence
- **Commit showing reproduction:** [To be added]

---

## Solution Approach

### Analysis
Mistral's API is OpenAI-compatible (`/v1/chat/completions`). The 
implementation follows the same pattern as `openai-provider.ts` but 
with Mistral-specific base URL and auth headers.

### Proposed Solution
Implement `MistralProvider` class following `ollama-provider.ts` and 
`openai-provider.ts` as references, using Mistral's Bearer token auth 
and OpenAI-compatible endpoint.

### Implementation Plan

**Understand:** Create a missing Mistral AI model provider that 
implements the IModelProvider contract.

**Match:** `openai-provider.ts` is the closest reference since Mistral 
uses OpenAI-compatible API shape. `ollama-provider.ts` shows streaming 
implementation patterns.

**Plan:**
1. Read `openai-provider.ts`, `ollama-provider.ts`, 
   `anthropic-provider.ts` fully
2. Read `IModelProvider` interface in 
   `self/shared/src/interfaces/subcortex.ts`
3. Create `mistral-provider.ts` implementing invoke()
4. Implement stream() with correct async iterable pattern
5. Implement getConfig() returning provider metadata
6. Add input validation against TextModelInputSchema
7. Export from `index.ts`
8. Write tests in `__tests__/`
9. Run pnpm typecheck, pnpm lint, pnpm test

**Implement:** [Branch link to be added]

**Review:** pnpm typecheck + pnpm lint + pnpm test all passing before PR

**Evaluate:** Tests cover invoke, stream, getConfig, and input 
validation error cases

---

## Testing Strategy

### Unit Tests
- [ ] invoke() returns correct ModelResponse for valid input
- [ ] stream() yields correct ModelStreamChunk sequence
- [ ] getConfig() returns correct provider metadata
- [ ] Input validation rejects invalid TextModelInput
- [ ] Auth error handled gracefully

### Integration Tests
- [ ] End-to-end invoke against Mistral API (with test key)

---

## Pull Request
**PR Link:** [To be added]
**Status:** Not yet submitted

---

## Resources Used
- https://docs.mistral.ai/
- https://github.com/orthogonalhq/nous-core/blob/main/CONTRIBUTING.md
- Reference: openai-provider.ts, ollama-provider.ts
