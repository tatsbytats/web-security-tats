# web-security-tats 

I am new to AI assisted development and found it hard to secure a website with AI help. This skill is meant to help other developers secure their web apps. I am tats, happy building.

An agent-agnostic security skill for AI coding assistants. It distills the essential full-stack web application security knowledge into guidance an AI agent can apply when building, reviewing, or hardening web apps, APIs, and deployments.


## What it covers

- The security mindset: assume malicious input and ask how a feature could be abused
- Non-negotiable rules: hash passwords, never encrypt them. No secrets in frontend code. Parameterized SQL only. No innerHTML with user input. Ownership based authorization to stop IDOR. Safe error handling. File upload content checks. Non-root containers.
- OWASP Top 10
- A layered defense model
- A learning progression from fundamentals to advanced topics
- A concrete audit workflow the agent can run

## Supported agents

The skill is plain markdown and works with any agent that loads SKILL.md style skills or rules.

- Claude Code: place in ~/.claude/skills/web-security-tats/
- opencode: place in ~/.config/opencode/skills/web-security-tats/
- Codex or ChatGPT agents: place in ~/.agents/skills/web-security-tats/
- Cursor: convert SKILL.md to a .mdc rule under .cursor/rules/

## Install

Pick your agent and run its one command. It clones the repo and drops the skill into the right folder.

Claude Code:

    git clone https://github.com/tatsbytats/web-security-tats.git && cp -r web-security-tats ~/.claude/skills/

opencode:

    git clone https://github.com/tatsbytats/web-security-tats.git && cp -r web-security-tats ~/.config/opencode/skills/

Codex or ChatGPT agents:

    git clone https://github.com/tatsbytats/web-security-tats.git && cp -r web-security-tats ~/.agents/skills/

Cline, Roo Code, Kilo Code, and Windsurf also read the Claude Code skill format, so the Claude Code command above works for them too (use their configured skills path if different).

Windows note: in Command Prompt replace the `cp -r web-security-tats DEST` part with `xcopy /E /I web-security-tats DEST`.

Restart your agent after installing. The skill triggers automatically on web app security, build, and review tasks.

## Usage

When you ask the agent to build, review, or harden a web app, the skill loads and guides the agent through the security rules and the audit workflow. The agent reports findings as Critical, High, and Recommendations with file and line references.

## Files

- SKILL.md: the skill content loaded by the agent

## License

MIT
