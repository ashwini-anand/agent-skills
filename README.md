# Agent Skills

A collection of autonomous AI agent skills designed for verification, reasoning, and multi-agent workflows.

## Installation & Usage in Google Antigravity

You can use these skills in Google Antigravity using either of the following methods:

1. **Global Skill Directory**:
   Copy the `SKILL.md` file into your Antigravity config directory at `~/.gemini/config/skills/<skill-name>/SKILL.md`.  
   *Example*: `~/.gemini/config/skills/llm-debate/SKILL.md`

2. **Direct Chat Prompt**:
   Copy and paste the `SKILL.md` contents directly into an Antigravity chat session and ask Antigravity to use it whenever you type `/<skill-name>` (e.g., `/llm-debate`).

Once added, trigger the skill by typing `/<skill-name>` (e.g., `/llm-debate`).

## Available Skills

- **[`llm-debate`](llm-debate/SKILL.md)**: Executes a multi-agent consensus workflow (Blind Evaluation -> 5-turn debate -> Summary) to eliminate hallucinations and cross-verify facts, logic, and code.  
  *Example*: `/llm-debate Should we use gRPC or REST over HTTP/2 for low-latency internal communication between 20+ microservices?`