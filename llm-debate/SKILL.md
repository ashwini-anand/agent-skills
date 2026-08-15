---
name: llm-debate
description: Executes a multi-agent consensus workflow (Blind Evaluation -> 5-turn maximum debate -> Consensus Summary) to eliminate hallucinations and cross-verify facts, logic, and code.
---

# LLM Debate

Use this skill whenever the user triggers /llm-debate or requests multi-agent verification, consensus checking, or cross-critique on a query, code snippet, design decision, or factual question.

## Protocol Steps

### Step 1: Blind Evaluation

1. Launch two separate subagents (LLM-A and LLM-B) simultaneously using `invoke_subagent`.
2. Give both subagents the exact same prompt independently without mentioning the other. Instruct subagents to actively use available workspace tools (such as running terminal commands, executing unit tests, reading codebase files, or searching the web) to verify claims before forming or defending their stance.
3. Collect both outputs.

### Step 2: Agreement Check

1. Compare the outputs from LLM-A and LLM-B.
2. If both agents fully agree in facts and logic:
   - Present the final consensus answer to the user using the Step 4 template.
   - Stop here.

### Step 3: Anti-Sycophancy Debate Loop (Max 5 Turns)

If the initial outputs differ, run an iterative turn-taking debate loop for up to 5 iterations:

1. **Turn N**:
   - Send LLM-A's latest response to LLM-B with the following anti-sycophancy instruction:
     > "LLM-A gave this response to the query: <LLM-A response>. Review their points carefully. Actively use available workspace tools (such as running terminal commands, executing unit tests, reading codebase files, or searching the web) to verify claims before forming or defending your stance. Change your stance ONLY if their logic or evidence proves you wrong. If their critique contains flaws or misunderstandings, defend your original position with clear reasoning."
   - Receive LLM-B's response/critique.
   - Send LLM-B's response to LLM-A with the same anti-sycophancy instruction.
   - Receive LLM-A's updated response/defense.

2. **Check for Consensus**:
   - If at any turn LLM-A and LLM-B reach mutual agreement on facts and logic, break early.

### Step 4: Final Summary Report

Generate a clear markdown response for the user formatted as follows:

# LLM Debate Results

### Status: [FULL CONSENSUS | RESOLVED VIA DEBATE | UNRESOLVED DEADLOCK]
- **Total Debate Turns**: [0 to 5]
- **Tools / Evidence Used**: [e.g., ran pytest, searched web, inspected file.py, or None]

## Executive Summary
[Concise 2-sentence summary of the final answer and debate outcome]

## Points of Agreement
- **[Topic 1]**: [Detail ...]
- **[Topic 2]**: [Detail ...]

## Points of Discrepancy (If Any)
- **Model A Position**: [Summary of LLM-A's stance]
- **Model B Position**: [Summary of LLM-B's stance]
- **Root Cause / Discrepancy**: [e.g., different initial assumptions, missing context, edge case interpretation]

## Verified Final Consensus / Recommendation
[The complete, verified solution for the user]

## Actionable Next Steps for User
- [ ] [Specific verification, test to run, or source for human inspection]
