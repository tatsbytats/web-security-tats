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

You need two things: the skill folder (it must be named web-security-tats) and your agent's skills folder.

Step 1. Get the files. Do one of these:
- Clone the repo:
      git clone https://github.com/tatsbytats/web-security-tats.git
- Or download the ZIP from the GitHub repo and unzip it.

Step 2. Copy the web-security-tats folder into your agent's skills folder. Pick the command for your system.

macOS or Linux (Terminal):

    cp -r web-security-tats ~/.claude/skills/

Windows (Command Prompt):

    xcopy /E /I web-security-tats %USERPROFILE%\.claude\skills\web-security-tats

Windows (PowerShell):

    Copy-Item -Recurse web-security-tats $HOME\.claude\skills\web-security-tats

Git Bash (any OS):

    cp -r web-security-tats ~/.claude/skills/

One line, clone and install, per system:

macOS or Linux / Git Bash:
    git clone https://github.com/tatsbytats/web-security-tats.git && cp -r web-security-tats ~/.claude/skills/

Windows Command Prompt:
    git clone https://github.com/tatsbytats/web-security-tats.git && xcopy /E /I web-security-tats %USERPROFILE%\.claude\skills\web-security-tats

Other agents: replace ~/.claude/skills/ with the right path.
- opencode: ~/.config/opencode/skills/
- Codex or ChatGPT agents: ~/.agents/skills/
- Cursor: copy SKILL.md into .cursor/rules/ as web-security-tats.mdc

Step 3. Restart your agent. The skill triggers automatically on web app security, build, and review tasks.

## Usage

When you ask the agent to build, review, or harden a web app, the skill loads and guides the agent through the security rules and the audit workflow. The agent reports findings as Critical, High, and Recommendations with file and line references.

## Files

- SKILL.md: the skill content loaded by the agent

## License

MIT
