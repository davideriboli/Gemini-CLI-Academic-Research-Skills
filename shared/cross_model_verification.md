# Cross-Model Verification Protocol (v3.1 Pro)

## Overview

This protocol enables optional cross-model verification for high-stakes AI judgments. When enabled, a second AI model independently reviews outputs from the primary model, reducing shared-bias blind spots.

**This is entirely optional.** All ARS skills work with **Gemini 3.1 Pro** alone. Cross-model verification is an additional layer for users who want higher confidence in integrity checks, devil's advocate challenges, and review judgments.

## Why Cross-Model Verification

A stress test of 68 AI-generated citations found 31% had problems — and all passed three rounds of same-model integrity checks. The root cause: the verifying AI and the generating AI share the same training data distribution, so they share the same blind spots. A different model (trained on overlapping but not identical data, with different RLHF tuning) can catch errors that the primary model systematically misses.

**What it improves:** Error rate reduction (estimated 31% → ~5-10%). Different models catch different types of hallucination patterns.

**What it doesn't solve:** Frame-lock (all LLMs share most training data), sycophancy (all RLHF models have this tendency). These are degree improvements, not kind improvements.

## Supported Models

| Model | API ID | Provider | Role |
|-------|--------|----------|----------|
| Gemini 3.1 Pro | `gemini-3.1-pro` | Google | **Primary model** (default for Gemini CLI) |
| GPT-5.4 Pro | `gpt-5.4-pro` | OpenAI | Cross-verification — strongest reasoning |
| Claude 3.5 Sonnet | `claude-3-5-sonnet` | Anthropic | Cross-verification — nuanced logic |
| GPT-5.4 | `gpt-5.4` | OpenAI | Cross-verification — balanced cost/performance |

**Recommended cross-verification pair:** Gemini 3.1 Pro (primary) + GPT-5.4 Pro or Claude 3.5 Sonnet (verifier).

## Setup Guide

### Prerequisites

You need API keys from at least one additional provider. Since you are using **Gemini CLI**, the Google API is already available.

### Step 1: Get API Keys for Verifiers

**OpenAI (GPT-5):**
1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Create a new API key.
3. Copy the key (starts with `sk-`).

**Anthropic (Claude):**
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Create a new API key.
3. Copy the key (starts with `sk-ant-`).

### Step 2: Set Environment Variables

Add to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
# Optional: Cross-model verification for ARS
export OPENAI_API_KEY="sk-your-key-here"
export ANTHROPIC_API_KEY="sk-ant-your-key-here"

# Choose your preferred cross-verification model
# Options: gpt-5.4-pro, claude-3-5-sonnet
export ARS_CROSS_MODEL="gpt-5.4-pro"
```

Then reload: `source ~/.zshrc`

### Step 3: Verify Setup

In Gemini CLI, you can test by asking:
```
Check if cross-model verification is available for ARS
```

## How It Works in Each Skill

### Integrity Verification (academic-pipeline, Stage 2.5 / 4.5)

**When `ARS_CROSS_MODEL` is set:**
- Primary model (Gemini) runs full Phase A-E verification as normal.
- After Phase A completes, a random 30% sample of references is sent to the cross-model for independent verification.
- Cross-model receives only the reference text and paper context — not Gemini's verification result (to prevent anchoring).
- Disagreements are flagged as `[CROSS-MODEL-DISAGREEMENT]` and prioritized for human review.

### Devil's Advocate (deep-research + academic-paper-reviewer)

**When `ARS_CROSS_MODEL` is set:**
- After the DA completes its standard review/checkpoint, the cross-model receives the same material and generates an independent critique.
- The DA then compares: any CRITICAL or MAJOR issues found by the cross-model but not by the DA are added as `[CROSS-MODEL-FINDING]`.

---

## API Call Patterns via Shell

Agents use `run_shell_command` to call secondary models when cross-verification is enabled.

### Calling OpenAI (GPT-5)
```bash
curl -s https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "'"$ARS_CROSS_MODEL"'",
    "messages": [{"role": "user", "content": "'"$(echo "$PROMPT" | jq -Rs .)"'"}],
    "temperature": 0.1
  }' | jq -r '.choices[0].message.content'
```

### Calling Anthropic (Claude)
```bash
curl -s https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "'"$ARS_CROSS_MODEL"'",
    "max_tokens": 2048,
    "messages": [{"role": "user", "content": "'"$(echo "$PROMPT" | jq -Rs .)"'"}]
  }' | jq -r '.content[0].text'
```

---

## Limitations & Graceful Degradation

If cross-model verification fails (API error, rate limit), the agent will log the error and continue with single-model verification. **Never block the pipeline on cross-model failure.**
