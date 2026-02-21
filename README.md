# interview-me

**A Claude Code skill that turns vague requirements into production-grade specs — by interviewing you like a senior architect would.**

[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://docs.anthropic.com/en/docs/claude-code) &nbsp; [![MIT License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> Claude Code skills are reusable, prompt-based tools that extend [Claude Code's](https://docs.anthropic.com/en/docs/claude-code) CLI capabilities.

<!-- ![demo](assets/demo.gif) -->
<!-- TODO: Replace with a speed-run GIF of a full interview session -->

## Why This Exists

When you're building alone, there's no one to challenge your assumptions. Requirements start as vague ideas, edge cases get discovered in production, and security concerns become afterthoughts.

**interview-me** is the senior architect you wish you had on the team. It reads your requirement (or file), analyzes your codebase for context, then conducts a structured interview — one question at a time — pushing back on contradictions, probing gaps, and refusing to move forward until security concerns are addressed.

The result: a comprehensive, opinionated specification document with a full decisions log and implementation order.

## Key Behaviors

| Behavior | What It Does |
|----------|-------------|
| **Codebase-Aware Pre-Analysis** | Forks an agent to scan your project's architecture, dependencies, and conventions before asking a single question |
| **One Question at a Time** | Deep, focused interview — never batches questions. Each question has 2-4 well-crafted options |
| **Active Pushback** | Challenges contradictions, over-engineering, and missing edge cases directly |
| **Security Hard Blocks** | Refuses to generate a spec until security concerns (PII, auth, injection, secrets) are explicitly addressed |
| **Coverage Tracking** | Visual progress map showing which areas are explored vs. pending |
| **Resume Support** | Saves interview state to JSON — pick up where you left off, with stale-answer detection |
| **Auto-Split Detection** | Proposes breaking large specs (8+ areas) into linked sub-specs with dependency graphs |
| **Task Breakdown** | Generates implementation tasks as GitHub Issues, markdown checklist, or Claude Code tasks |

## Installation

### Quick Install (curl)

**Global** (available in all projects):
```bash
mkdir -p ~/.claude/skills/interview-me
curl -o ~/.claude/skills/interview-me/SKILL.md \
  https://raw.githubusercontent.com/Sorbh/interview-me/main/interview-me/SKILL.md
```

**Per-project**:
```bash
mkdir -p .claude/skills/interview-me
curl -o .claude/skills/interview-me/SKILL.md \
  https://raw.githubusercontent.com/Sorbh/interview-me/main/interview-me/SKILL.md
```

### Git Clone (for customization)

```bash
git clone https://github.com/Sorbh/interview-me.git
cp interview-me/interview-me/SKILL.md ~/.claude/skills/interview-me/SKILL.md
```

> Restart your Claude Code session after installation.

## Usage

```
/interview-me path/to/requirements.md
/interview-me "I want to build a user authentication system with OAuth"
```

Or just tell Claude: *"interview me about this feature"*

## Example Output

Here's an abbreviated look at what interview-me generates:

```
# Spec: User Authentication System

## Overview
OAuth2-based authentication with Google and GitHub providers,
session management via HTTP-only cookies, and role-based access control.

## Goals & Non-Goals
- Goal: Support Google + GitHub OAuth providers
- Goal: Session-based auth with 24h expiry
- Non-Goal: SAML/enterprise SSO (out of scope for v1)

## Security Considerations
- HARD BLOCK RESOLVED: Token storage moved from localStorage to HTTP-only cookies
- CSRF protection via double-submit cookie pattern
- Rate limiting: 5 failed attempts → 15min lockout

## Decisions Log
| # | Topic           | Decision              | Rationale                        |
|---|----------------|-----------------------|----------------------------------|
| 1 | Token storage  | HTTP-only cookies     | Pushback: localStorage is XSS-vulnerable |
| 2 | Session length | 24h with refresh      | User preference over 1h default  |
| 3 | OAuth providers| Google + GitHub only  | Scope constraint for v1          |

## Implementation Order
1. Database schema (users, sessions, oauth_tokens)
2. OAuth provider integration (Google → GitHub)
3. Session middleware
4. Protected route wrapper
5. Login/logout UI components
   ...
```

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI

## License

MIT
