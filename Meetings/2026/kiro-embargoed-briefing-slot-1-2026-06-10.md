---
title: "FW: Kiro Embargoed Briefing - Slot 1"
date: 2026-06-10
time: "14:30-15:00"
tags: [kiro, aws, product-briefing, briefing, embargoed, meeting-notes]
type: meeting-notes
status: active
confidentiality: embargoed
people: ["[[Helen Hasbun]]", "[[Kyle]]", "[[Sameer Ali]]"]
related: ["[[2026-06-10 Daily Briefing]]", "[[2026-06-10 End-of-Day Journal]]"]
---

# FW: Kiro Embargoed Briefing - Slot 1

> Source: pasted notes from the 2026-06-10 Kiro embargoed briefing.
> 
> Publishing note: treat as embargoed until the launch details are public.

## Summary

Kiro is positioning itself as an IDE, CLI, web, and mobile platform built around the same agent harness. The core bet is less about a single model and more about a multi-model enterprise agent system with controls for context management, memory, repo maps, and safe cloud execution.

The next wave of launches centers on mobile, specs on web, automations, Agent Focus Mode in the IDE, and a CLI migration onto the new agent harness. The group is expected to receive early access to the iOS TestFlight and the updated CLI.

---

## Platform Overview

- Kiro combines IDE, CLI, web, and mobile surfaces that share the same agent harness.
- The core bet is a multi-model harness with enterprise-first controls, context management, memory, and repo maps.
- Every session runs in an isolated cloud sandbox with 128 GB storage.
- The sandbox is treated as an untrusted environment for safe code execution.
- Multi-repo support is available per session.
- GitLab support is rolling out today and can be mixed with GitHub repos.

---

## New Launches

### Mobile App

- iOS-first companion app for monitoring and managing agent sessions on the go.
- Supports autonomous mode, described as "set and step away."
- Supports non-autonomous mode.
- Gated launch planned for the New York event next week.
- TestFlight access was promised to this briefing group.
- Android has no firm timeline.
- Android beta is tentatively targeted for July/August, with Kyle caveating: "don't hold me to that."

### Specs on Web

- Structured planning layer covering requirements, design, and tasks.
- Includes a dependency graph for sub-agent parallelization.
- Uses automated reasoning to check requirement consistency.
- Generates verifiable property-based tests.
- Already live silently on web.
- Formally announced today.
- Coming to CLI soon.
- Roadmap includes collaborator invites so non-technical stakeholders, such as PMs, can annotate and review specs.

### Automations

- Cron-style scheduled agent runs against selected repos using a prompt.
- Main use cases:
  - changelogs
  - security reviews on recent PRs
  - dependency updates
- Launching next week alongside mobile.

### Agent Focus Mode

- One-click overlay inside the IDE.
- Surfaces multi-session management without requiring a full IDE rewrite.
- Beta is tentatively planned for next week or shortly after.
- Public vs. private beta is still TBD.

### CLI Update

- CLI is moving to the new agent harness.
- Gains specs, `/rewind`, effort configs, and `/goal` autonomous loops.
- Mid-turn steering is included.
- Early access for this group is expected this week via a follow-up email from Helen.

---

## Q&A Highlights

### Model Switching

- No user-facing model control today.
- Auto mode internally matches model choice to effort level.

### MCP on Kiro Web

- MCP servers run inside the sandbox.
- OAuth support for remote MCP servers, such as Postman and Figma, is rolling out soon.
- Network controls support open internet or allowlisted access.
- Specific MCP endpoints can be permitted.
- IAM role assumption is supported and is positioned as more secure than key/value pairs.
- IAM policies can be scoped to user ID.

### Config Portability

- Agent configs carry over from web to mobile.
- Portable config includes:
  - repos
  - MCP
  - IAM
  - steering files

### LLM Gateway / Guardrails

- Not supported today.
- Kiro is running on managed Bedrock with no customer data access.
- Iterative guardrails support, such as Bedrock Guardrails, is being discussed internally.

### Longer-Term Direction

- Kyle framed Kiro's focus as the system around the model:
  - context management
  - repo maps
  - memory
- "Always-on persistent agent fleets" are actively being explored, with nothing public yet.
- OpenClaude story: "nothing to share publicly, but we are very active in that."
- More roadmap details are expected in the next few months, including around Kiro's birthday in July.

---

## Action Items

- [ ] **[[Kyle]] / [[Helen Hasbun]]:** Share TestFlight access for the iOS mobile app with this group at launch.
- [ ] **[[Helen Hasbun]]:** Send early access details for the CLI update this week.
- [ ] **[[Kyle]]:** Flag this group for Android beta access when ready, tentatively July/August.
- [ ] **[[Kyle]]:** Follow up on the LLM gateway / guardrails question.
- [ ] **[[Helen Hasbun]] / [[Kyle]]:** Re-engage this group around Kiro's July birthday for roadmap updates.

---

## See Also

- [[2026-06-10 Daily Briefing]] - day plan and scheduled briefing
- [[2026-06-10 End-of-Day Journal]] - daily recap
