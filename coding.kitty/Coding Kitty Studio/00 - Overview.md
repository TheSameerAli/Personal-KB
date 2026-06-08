---
title: "Vibe Coding Teaching Platform"
tags: [coding-kitty, product-idea, education, ai-coding, vibe-coding, platform]
type: product-idea
status: shaping
date: 2026-06-08
---

# Vibe Coding Teaching Platform

## Project Space

Main hub:

- [[Coding Kitty Studio]]

Planning notes:

- [[Product/Product Vision|Product Vision]]
- [[Product/MVP Scope|MVP Scope]]
- [[Curriculum/Learning Path|Learning Path]]
- [[Research/Market Research|Market Research]]
- [[Strategy/Business Plan|Business Plan]]
- [[Strategy/Product-Market Fit|Product-Market Fit]]
- [[Strategy/Marketing Strategy|Marketing Strategy]]
- [[Operations/Roadmap|Roadmap]]
- [[Operations/Open Questions|Open Questions]]
- [[Operations/Decision Log|Decision Log]]

## Original Seed

So currently I have a page called Coding Kitty where we explain tech concepts, software concepts, coding concepts in a very bite size, sort of consumable way, and I want to explore sort of expanding this as an educational platform. So, we also have a website called the thecodingkitty.com over there, we have sort of it's just a landing page for The Coding Kitty, and sort of it has all the videos, the voting, and all of that. So, I want to implement a platform for education and right now Vibe coding is something that people are getting into but a lot of non-technical people do not know how to get into vibe coding. So it will be sort of an interactive platform where which people can use to learn how to start with vibe coding, how to use AI agent decoding tools they can use a step-by-step guide. So, obviously, every vibe coder also needs programming basics, so it's going to go through those programming basics and sort of have a structure to go from zero to hero which means it's gonna go from vibe coding to you know From nothing knowing nothing about a technical concepts to learning the technical concepts first is starting with a language and all of that and then going on to building an application at the end of the day so that is the idea, and that is what we need to implement.

## Refined Thesis

Coding Kitty should teach non-technical people how to become AI-assisted software builders: not by becoming traditional programmers first, but by learning enough software thinking to guide agentic coding tools, understand what they produce, fix issues, and ship real apps.

Vibe coding is the cultural hook, but the durable product is software creation literacy for the AI age.

## Core Promise

Go from idea to deployed app using AI coding agents, while learning the software basics needed to stay in control.

This avoids two common problems:

- pretending people do not need fundamentals
- forcing people through a traditional programming course before they build anything meaningful

## Product Positioning

The platform should feel less like "another coding course" and more like a guided workspace for turning ideas into shipped software.

Possible positioning:

- **Coding Kitty Studio**: interactive, project-based, build-focused
- **Coding Kitty Academy**: broader education brand, more course-like
- **Vibe Coding by Coding Kitty**: direct and trend-aligned, useful as a course/path name

Recommended direction:

> `coding.kitty` remains the media and education brand. `Coding Kitty Studio` becomes the interactive learning platform underneath it.

This gives the platform separation while keeping it clearly tied to the Coding Kitty brand.

## Primary Learners

The platform is for people who want agency with software but do not currently identify as traditional programmers.

Primary audience:

- non-technical people
- creators
- students
- professionals trying to automate their work
- founders/operators who want to prototype ideas
- curious beginners who feel coding is intimidating

They are not necessarily trying to become software engineers on day one. They want to understand coding, guide AI tools, build useful things, and deploy real apps.

## Learning Philosophy

The platform should be project-first and fundamentals-as-needed.

Instead of:

> Learn variables, functions, HTML, CSS, JavaScript, then eventually build.

The platform should work more like:

> Build a small personal site, encounter HTML and CSS, learn just enough, use an AI coding agent, inspect the output, improve it, then deploy.

This better matches the learner's motivation and keeps the experience practical.

## Learning Outcomes

By the end of the core path, a learner should be able to:

- understand the basic parts of a web app
- explain an app idea clearly enough for an AI agent to build from
- use agentic coding tools to generate, modify, and debug code
- read enough code to understand what changed
- recognize common frontend, backend, database, deployment, and environment issues
- use version control to save work, understand changes, and recover from mistakes
- understand the basic infrastructure behind a deployed app
- deploy a simple app
- keep improving the app after the first version ships

## Curriculum Structure

### Phase 1: Builder Mindset

- What software is
- What AI coding agents can and cannot do
- How apps, websites, APIs, databases, and hosting fit together
- How to describe an app idea clearly
- How to break an idea into features
- How to think like a product builder

### Phase 2: First App

Recommended free-path project:

- personal website

Reasons:

- emotionally rewarding
- low technical risk
- visually clear
- easy to deploy
- useful for students, creators, and professionals

Possible follow-up paid/deeper project:

- simple productivity app

Reasons:

- introduces state, data, workflows, and persistence
- naturally teaches more software fundamentals
- relevant to professionals trying to automate work

### Phase 3: Prompting Agents

- Writing a clear build prompt
- Asking for a plan before implementation
- Asking the agent to explain code
- Asking the agent to debug
- Asking for small, scoped changes
- Reviewing AI output
- Avoiding vague prompts like "make it better"

### Phase 4: Software Basics for AI Builders

- files and folders
- HTML, CSS, and JavaScript basics
- APIs
- databases
- authentication
- environment variables
- terminal basics
- Git and GitHub basics
- dependencies and package managers
- common error messages

These topics should be taught when the learner needs them during projects, not as abstract prerequisites.

### Phase 5: Version Control

Learners should understand version control as a practical safety net, not as abstract developer ceremony.

- what version control is and why it matters
- Git vs GitHub
- repositories
- commits
- branches
- pull requests at a beginner level
- reading diffs to see what changed
- reverting or recovering from mistakes
- using GitHub as a project home
- asking AI agents to explain code changes before committing

The goal is not to make learners Git experts immediately. The goal is to help them build the habit of saving meaningful checkpoints and understanding what changed when an AI agent edits their project.

### Phase 6: Infrastructure and Deployment

Learners should understand enough infrastructure to know where their app lives, how it runs, and what can break after deployment.

- what deployment means
- static sites vs full-stack apps
- hosting providers
- domains and DNS basics
- environment variables
- databases in production
- APIs and backend services
- authentication services
- logs and error monitoring
- build steps and CI/CD at a beginner level
- common deployment failures
- costs, limits, and security basics

This should be taught through real deployment tasks, not as a separate cloud theory course.

### Phase 7: Ship and Improve

- deploy the app
- test the app
- fix bugs
- add features
- collect feedback
- share the project
- improve the next version

## Platform Experience

The product should include an actual interactive workspace with tasks, code, and progress tracking.

Potential workspace components:

- lesson sidebar
- project checklist
- guided task flow
- code or artifact workspace
- prompt builder
- "ask the agent" moments
- progress tracking
- checkpoints
- deployment steps
- final project showcase

## Guided Agent Tasks

A core mechanic could be guided agent tasks.

Example:

> Task: Add a contact form to your website.

Steps:

1. Describe what the form should collect.
2. Generate a prompt.
3. Run it in the chosen tool.
4. Review what changed.
5. Debug if it fails.
6. Mark the task complete.

This keeps the product tool-agnostic while still giving learners a practical, step-by-step experience.

## Tool-Agnostic Strategy

The platform should teach principles first, then integrate specific tools later.

Suggested progression:

### Beginner-Friendly Platforms

- Lovable
- Bolt
- Replit
- v0
- other easy-to-use app generation tools

### Intermediate Agentic Coding

- Cursor
- Windsurf
- GitHub Copilot
- Codex-style agents
- Claude Code-style agents

### Advanced Developer Workflow

- local IDEs
- terminal
- GitHub
- deployment providers
- databases

The platform should provide tool recipes without making the core learning experience dependent on one vendor.

## Freemium Model

### Free Tier

- intro lessons
- first personal website path
- basic prompt templates
- basic progress tracking
- community access, if desired

### Paid Tier

- full project paths
- productivity app path
- automation workflow modules
- AI agent playbooks
- project reviews
- templates
- certificates or badges
- office hours
- challenges and hackathons

### Sponsor-Supported Opportunities

- tool-specific modules
- sponsored build challenges
- hackathon tie-ins
- "build with X" labs
- partner resource sections

## Brand and Theme

The platform should sit under the Coding Kitty brand with a medium level of cat theming.

Good use of theme:

- mascot guidance
- playful lesson names
- cat-flavored examples
- badges
- soft visual identity
- short videos in the classic Coding Kitty tone

Avoid:

- making every concept a cat joke
- overwhelming the learning interface
- making the product feel unserious for professionals

The product should feel friendly and approachable, but still credible.

## MVP Recommendation

Build the first version around one complete guided path:

> Build and deploy your first personal website with AI.

The MVP should include:

- Coding Kitty Studio branding
- a structured lesson path
- interactive task checklist
- progress tracking
- prompt templates
- tool-agnostic guidance
- instructions for 2-3 beginner-friendly tools
- beginner-friendly version control flow
- deployment walkthrough
- simple infrastructure explainer for where the project lives after deployment
- final project completion page or showcase

The second path should be:

> Build a simple productivity app with an AI coding agent.

This creates a natural free-to-paid journey.

## Long-Term Vision

Over time, this can become:

- a course business
- a community
- a SaaS learning platform
- a sponsor-supported education hub
- a project showcase
- a hackathon and challenge engine

The long-term platform should help people move from "I have an idea" to "I shipped something real" with AI as the accelerator and Coding Kitty as the guide.

## Open Decisions

- What is the exact first MVP scope?
- Should the platform execute code itself, or guide users through external tools first?
- Which 2-3 tools should be supported in the first version?
- Which version control workflow should be taught first: GitHub-only, local Git, or both?
- Which deployment/infrastructure providers should be used in the first path?
- What does the free tier include exactly?
- What does the paid upgrade unlock?
- Should the first course be called "Vibe Coding" or should that only be used in marketing copy?
- How much community support should be included in version one?
- Should project submissions and showcases be part of MVP or a later feature?
