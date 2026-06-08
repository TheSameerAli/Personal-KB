---
title: "coding.kitty/learn — Idea-to-Course Engine"
tags: [coding-kitty-learn, product, ai-first-learning, personalization, usp]
type: product-strategy
status: draft
date: 2026-06-08
---

# Idea-to-Course Engine

## Core Idea

coding.kitty/learn should not start with a fixed course catalog.

It should start with the learner's app idea.

The platform asks:

> What do you want to build?

Then it turns that answer into a bespoke AI-builder course that teaches the exact concepts, prompts, tools, checkpoints, and deployment steps needed to build that product.

This is the clearest USP:

> A learning platform that turns your app idea into a personalized build course, then teaches you to ship it with AI agents while understanding what is happening.

## Why This Is Stronger Than A Normal Course

Most beginner coding courses start from a curriculum:

- learn HTML
- learn CSS
- learn JavaScript
- learn Git
- build a sample app

Most vibe-coding courses start from a tool:

- learn Cursor
- learn Lovable
- learn Replit Agent
- learn prompting
- follow the instructor's project

coding.kitty/learn can start from motivation:

- I want to build a tiny-task to-do app
- I want to build a habit tracker
- I want to build a portfolio site
- I want to build an invoice generator
- I want to build an internal tool for my job

The learner is more likely to finish because the course is attached to something they already care about.

## Anti-Vibe Coding Position

The platform can use "vibe coding" as the cultural hook, but the point is not to make people blindly vibe their way into fragile software.

The better positioning is:

> Vibe coding gets you moving. coding.kitty/learn helps you understand, verify, version, deploy, and improve what AI builds.

This makes the brand feel practical and trustworthy. It still welcomes beginners, but it does not sell the fantasy that software creation has no consequences.

## User Journey

### 1. Idea Intake

The learner describes what they want to build.

Example:

> I want to build a to-do list app that breaks big tasks into tiny actionable steps.

The platform asks a few clarifying questions:

- Who is this for?
- Should it be web, mobile, or not sure yet?
- Does it need login?
- Does it need to save data?
- Does it need notifications, payments, AI features, or collaboration?
- How technical are you today?
- Which tools do you want to use, if any?

### 2. Project Diagnosis

The platform maps the idea into a software shape.

For the tiny-task to-do app, the diagnosis might be:

- product type: productivity app
- likely route: web app first, mobile later
- frontend: task list, task detail, breakdown UI, completion states
- backend: API or server actions if using a full-stack framework
- data: tasks, subtasks, users, preferences
- optional AI: task breakdown assistant
- infrastructure: database, hosting, environment variables, domain
- risk areas: authentication, data persistence, AI cost, overcomplicated first version

### 3. Learning Path Generation

The platform creates a course from reusable content blocks.

The course is not generated from scratch every time. It should be assembled from a trusted knowledge library:

- core explainers
- project tasks
- prompt templates
- tool recipes
- debugging drills
- Git checkpoints
- deployment labs
- security notes
- quizzes and reflection checks

For the tiny-task to-do app, the course might include:

- product scoping for AI builders
- frontend basics for interactive lists
- data modeling basics
- authentication basics, if needed
- database basics
- API basics
- prompting an AI agent to build in small slices
- reviewing diffs after each feature
- deploying a full-stack app
- buying and connecting a domain
- monitoring logs after launch

### 4. Tool Route Selection

The platform recommends one or more build routes:

| Route | Best for | Example tools |
| --- | --- | --- |
| Browser app-builder route | fastest beginner prototype | Lovable, Bolt, Replit |
| Web-code route | transferable web app skills | Replit, Cursor, GitHub, Vercel/Netlify |
| Mobile route | mobile-first ideas | Expo, React Native, FlutterFlow, mobile AI builders |
| Automation/internal-tool route | work productivity apps | Retool-style tools, Google Sheets, APIs, scripts |

For MVP, the platform should probably recommend one default route and show alternatives without overwhelming the learner.

### 5. Guided Build Sprints

The course is broken into build sprints.

Each sprint teaches one product outcome and the concepts needed for it.

Example:

- Sprint 1: define the app and create the first screen
- Sprint 2: add task creation
- Sprint 3: save tasks
- Sprint 4: break tasks into subtasks
- Sprint 5: add user accounts, if needed
- Sprint 6: deploy
- Sprint 7: connect domain and test production
- Sprint 8: improve, share, and plan v2

Each sprint includes:

- what the learner is building
- why it matters
- the concept being learned
- a prompt builder
- expected AI-agent behavior
- review checklist
- Git checkpoint
- debugging branch
- completion criteria

## Example Personalized Course

Idea:

> A to-do app that breaks each task into tiny steps.

Generated course:

1. Shape the product
   - define user, use case, and first version
   - learn what "MVP" means
   - write the first product prompt

2. Choose the build route
   - web app vs mobile app
   - beginner-friendly stack recommendation
   - learn what frontend, backend, and database mean

3. Build the first UI
   - create task list screen
   - learn components, state, and styling at a high level
   - prompt the agent to build only the first screen

4. Add task creation
   - create form and task card
   - learn forms and validation
   - review the AI-generated code change

5. Add tiny-step breakdown
   - manual breakdown first
   - optional AI breakdown later
   - learn why feature complexity should be staged

6. Save the data
   - choose local storage or database
   - learn persistence
   - add a Git checkpoint before touching storage

7. Add accounts, if needed
   - decide whether login is required for v1
   - learn authentication basics
   - avoid overbuilding early

8. Deploy the app
   - connect repo
   - configure environment variables
   - learn build logs and deployment errors

9. Buy and connect a domain
   - learn DNS basics
   - connect domain to hosting provider
   - verify HTTPS

10. Improve safely
    - collect feedback
    - plan v2
    - use small prompts and checkpoints

## Product Architecture Implication

The platform needs a content library, not just AI text generation.

Useful internal model:

- `Project Archetypes`: portfolio site, CRUD app, marketplace, content app, dashboard, automation, AI wrapper, mobile app
- `Capability Blocks`: frontend, backend, database, auth, payments, AI API, files, email, notifications, deployment
- `Learning Blocks`: concept explainers, exercises, quizzes, reflection prompts
- `Agent Task Blocks`: prompt templates, review checklists, debugging prompts
- `Tool Recipes`: Lovable, Bolt, Replit, Cursor, GitHub, Vercel, Netlify, Supabase, Firebase
- `Risk Rules`: when to warn about security, auth, payments, private data, or scope creep

The AI layer should act as a course planner and tutor on top of curated blocks.

## MVP Version

The MVP should not try to generate every possible app course.

Start with a constrained idea intake that maps most beginner ideas into 3-5 supported project archetypes:

- personal website
- simple productivity app
- content/blog app
- simple dashboard
- lightweight AI wrapper

For each archetype, build a high-quality canonical path, then let AI personalize:

- app name
- feature examples
- prompts
- chosen tool route
- concepts included or skipped
- final deployment checklist

This gives the experience of personalization without relying on fully unbounded AI curriculum generation.

## Strategic Bet

The platform wins if learners say:

> I did not just watch a course. I built my idea, and now I understand what the AI helped me do.

