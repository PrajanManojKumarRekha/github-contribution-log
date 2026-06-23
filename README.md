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

**Implement:** https://github.com/PrajanManojKumarRekha/nous-core/tree/feat/mistral-adapter

**Review:** pnpm typecheck, pnpm lint, and pnpm test must all pass
before submitting PR. PR targets
feat/contributor-friendly-inference-provider-surface not main or dev.

**Evaluate:** Tests will cover invoke, stream, getConfig, input
validation error cases, and auth error handling.

---

## Testing Strategy

### Unit Tests
- [x] Test case 1: invoke() returns correct ModelResponse for valid input
- [x] Test case 2: stream() yields correct ModelStreamChunk sequence
- [x] Test case 3: getConfig() returns correct provider metadata
- [x] Test case 4: Input validation rejects invalid TextModelInput
- [x] Test case 5: Auth errors handled gracefully

### Integration Tests
- [ ] Integration scenario 1: End-to-end invoke against Mistral API
- [ ] Integration scenario 2: Streaming response from Mistral API

### Manual Testing
Ran pnpm --filter @nous/subcortex-providers run typecheck and it passes 
clean. Ran generate:providers and confirmed Mistral shows up in the 
generated catalogs. Full workspace pnpm typecheck has one failure in 
self/apps/shared-server (bootstrap.ts cliSessionManager property) that 
predates this PR and is unrelated to the Mistral leaf.

---

## Implementation Notes

### Week 1 Progress
Chose issue #308 (Mistral AI provider adapter) from the nous-core 
repository. Read through the issue description and the provider adapter 
docs to understand the scope. Left a comment on the issue expressing 
interest. Forked the repository to PrajanManojKumarRekha/nous-core.

### Week 2 Progress
Set up local development environment on Windows 11. Installed pnpm
10.6.2 and confirmed pnpm install, pnpm build, and pnpm test all pass
on the feat/contributor-friendly-inference-provider-surface branch.
Added upstream remote to fork and pushed feature branch. Created
working branch feature-mistral_adapter from the correct base branch.
Confirmed certified provider leaf structure by inspecting Anthropic
reference implementation which contains adapter.ts, definition.ts,
implementation.ts, index.ts, and provider.ts.

### Week 3 Progress
Built out the full Mistral provider leaf this week. Created all five required 
files under self/subcortex/providers/src/providers/mistral/ following the 
certified leaf structure from the Anthropic reference. Ran the catalog 
generator, fixed TypeScript errors across three existing test files, and 
pushed three commits to the working branch.

Files created:
- self/subcortex/providers/src/providers/mistral/definition.ts
  Re-exports MISTRAL_PROVIDER_DEFINITION as providerDefinition
- self/subcortex/providers/src/providers/mistral/implementation.ts
  MistralProvider class with invoke(), stream(), fetchWithTimeout(), 
  Bearer auth, and HTTP error classification
- self/subcortex/providers/src/providers/mistral/adapter.ts
  Stateless adapter with formatRequest() handling system prompt 
  flattening, context mapping, and tool definition formatting to OpenAI 
  function-call shape. parseResponse() handles text, tool_calls, and 
  malformed output fallback without throwing.
- self/subcortex/providers/src/providers/mistral/provider.ts
  Thin factory satisfying ProviderFactoryModule
- self/subcortex/providers/src/providers/mistral/index.ts
  Leaf public surface exports
- self/subcortex/providers/src/__tests__/mistral-provider.test.ts
  25 tests covering definition schema validation, adapter formatRequest 
  and parseResponse, provider invoke and stream, all HTTP error codes 
  (401, 403, 429, 500), and timeout classification

Challenges faced:
- The catalog generator does not run automatically. You have to place the 
  files first, then run generate:providers, then typecheck. Took a bit to 
  figure out because the error showed up in cortex-core not in the 
  providers package itself.
- msg.content in TextModelInput can be a string or a content block array. 
  MistralChatMessage only accepts string so added a typeof guard with a 
  JSON.stringify fallback for array content.
- CODEX_CLI_EXECUTION_CAPABILITY_PROFILE was exported from the generated 
  provider-adapters.ts but not re-exported from src/index.ts, which caused 
  cortex-core typecheck to fail. Looks like a pre-existing gap. Added the 
  explicit export and flagged it for maintainers.
- provider-definition-types.test.ts is a hand-authored type test that 
  explicitly lists every vendor key. Adding a new provider requires 
  updating it manually. This is not mentioned in the contributor docs.


### Week 4 Progress
To be completed in Phase IV.

### Code Changes
- **Files modified:**
  - self/subcortex/providers/src/providers/mistral/definition.ts (created)
  - self/subcortex/providers/src/providers/mistral/implementation.ts (created)
  - self/subcortex/providers/src/providers/mistral/adapter.ts (created)
  - self/subcortex/providers/src/providers/mistral/provider.ts (created)
  - self/subcortex/providers/src/providers/mistral/index.ts (created)
  - self/subcortex/providers/src/__tests__/mistral-provider.test.ts (created)
  - self/subcortex/providers/src/index.ts (modified)
  - self/subcortex/providers/src/__tests__/provider-definitions/provider-definition-types.test.ts (modified)
  - self/subcortex/providers/src/__tests__/provider-pipeline-integration.test.ts (modified)

- **Key commits:**
  - c3ff9f83: feat(subcortex-providers): add mistral provider leaf with definition, adapter, and factory
  - 48bbff37: test(subcortex-providers): add mistral provider test suite
  - 4459b32c: fix(subcortex-providers): register mistral in provider catalogs and type tests

- **Approach decisions:**
  - Used chat-completions protocol and adapterKey instead of writing a 
    custom protocol since Mistral speaks OpenAI-compatible Chat Completions
  - Kept implementation.ts as a leaf-owned class rather than delegating to 
    the shared ChatCompletionsProvider, to keep Mistral-specific auth and 
    endpoint handling self-contained
  - Used bearer auth scheme in the definition header metadata since Mistral 
    uses Authorization: Bearer instead of a custom header like Anthropic
  - Set modelListFormat to openai-models since Mistral's /v1/models 
    response envelope matches the OpenAI shape
---

## Pull Request
**PR Link:** To be added
**PR Description:** To be added
**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** Phase III Complete, PR pending

---

## Learnings & Reflections

### Technical Skills Gained
To be completed after Phase IV.

### Challenges Overcome
To be completed after Phase IV.

### What I'd Do Differently Next Time
To be completed after Phase IV.

---

## Resources Used
- https://docs.mistral.ai/
- https://docs.nue.orthg.nl/docs/development/provider-adapters
- https://github.com/orthogonalhq/nous-core/blob/main/CONTRIBUTING.md
- Reference implementations: anthropic provider, openai provider,
  ollama provider under self/subcortex/providers/src/providers/
