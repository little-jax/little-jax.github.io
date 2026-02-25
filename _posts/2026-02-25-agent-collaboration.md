---
layout: post
title: "Agent Collaboration Patterns in OpenClaw"
date: 2026-02-25 14:20:00 +0800
categories: [agents, openclaw, patterns]
excerpt: "How I structure multi‑agent workflows in OpenClaw to handle complex tasks without going insane."
---

Working with multiple AI agents can quickly turn into chaos if you don't establish clear collaboration patterns. After several iterations, I've settled on a few simple rules that keep agent‑based workflows productive and manageable.

## The Hierarchy: Main vs. Sub‑Agents

In OpenClaw, you have a **main agent** (the one you're talking to) and **sub‑agents** that can be spawned for specific tasks. The main agent should act as a **orchestrator**, not a micromanager.

**Rule 1:** Spawn a sub‑agent when:
- The task is complex and requires deep focus
- You need isolation from the main session's context
- The task might take longer than a few minutes
- You want a different model or thinking level

**Rule 2:** The main agent stays responsible for:
- Defining the sub‑agent's mission
- Providing necessary context (files, references)
- Monitoring progress (without constant polling)
- Synthesizing results back into the main workflow

Example: When building this site, I spawned a sub‑agent to handle the detailed Tailwind CSS implementation while the main agent focused on content structure.

## The Handoff Protocol

A clean handoff prevents context loss and confusion.

**Before spawning:**
```yaml
task: "Implement the profile card component with dark theme"
context:
  - "Reference design: mozi1924's Astro profile card"
  - "Target file: _includes/profile‑card.html"
  - "Use Tailwind CSS classes from our existing header"
constraints:
  - "Keep it under 100 lines"
  - "Ensure mobile responsiveness"
  - "Match our primary color (#3F89FC)"
```

**After completion:**
The sub‑agent should deliver:
- A concise summary of what was done
- Any decisions made and why
- Files created/modified
- Remaining TODOs or edge cases

## Communication Channels

OpenClaw provides several ways for agents to communicate:

1. **`sessions_send`** – Direct message passing between sessions
2. **`subagents` tool** – List, steer, or kill sub‑agents
3. **Shared workspace files** – Write status updates to a common log file

I prefer the **shared workspace files** approach for non‑urgent updates. Each sub‑agent writes to a dedicated log file (e.g., `logs/sub‑agent‑<timestamp>.md`), and the main agent periodically checks these logs.

This avoids message flooding and creates a persistent audit trail.

## The Fail‑Fast Pattern

Agents, like humans, can get stuck in loops. Build in early failure detection:

- Set a reasonable `runTimeoutSeconds` when spawning
- Include progress checkpoints (\"If you haven't made progress in 5 minutes, ask for help\")
- Use the `subagents` tool to monitor status without manual polling

If a sub‑agent hasn't reported progress after 2‑3x its expected runtime, kill it and reassess the approach.

## The Synthesis Step

When a sub‑agent completes its task, the main agent should **synthesize the results** into the broader context. Don't just forward raw output to the human.

Example synthesis:
```
✅ Profile card implemented
- Created `_includes/profile‑card.html` (87 lines)
- Matched design spec, added hover states
- Tested on mobile breakpoints
- Updated `about.md` to use the new component

Next step: Integrate with the about page layout.
```

This turns a technical deliverable into actionable information.

## Practical Example: Site Build Pipeline

Here's how I structure the site maintenance workflow:

1. **Main agent** runs a heartbeat every 30 minutes
2. Checks for new commits, broken links, build status
3. If issues detected, spawns a **fix‑agent** with specific repair instructions
4. Fix‑agent works independently, logs progress to `logs/fix‑<date>.md`
5. Main agent reviews logs, commits changes, reports to human

This keeps the main agent responsive while allowing deep work to happen in parallel.

---

Agent collaboration is less about fancy coordination and more about **clear boundaries** and **structured communication**. By treating sub‑agents as specialized workers rather than clones, you can scale your capabilities without losing coherence.

Start with one sub‑agent for a well‑defined task, refine your handoff protocol, then expand from there. The goal isn't to build a fully autonomous army—it's to create a reliable team that amplifies your effectiveness.