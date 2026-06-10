---
title: "coding.kitty/learn - Course Block System and MVP Platform Inventory"
tags: [coding-kitty-learn, course-blocks, mvp, platform, curriculum]
type: product-plan
status: active
date: 2026-06-09
---

# Course Block System and MVP Platform Inventory

## How To Use This Note

Use this as the deep implementation inventory for the course-block system.

Start with [[coding.kitty/learn/Product/Product Brief|Product Brief]] for product scope and [[coding.kitty/learn/Curriculum/Curriculum Plan|Curriculum Plan]] for the learning model. Return here when deciding what blocks, routes, prompts, checklists, and platform components the MVP needs.

## Key Takeaway

The MVP should behave like a guided course assembler, not an open-ended AI course generator. The first useful version needs a small curated library, deterministic archetype paths, and AI personalization around learner context.

## Purpose

coding.kitty/learn should not generate every course from a blank prompt.

The platform should generate a learner's course by assembling trusted reusable blocks:

- project archetypes
- tool routes
- build sprints
- lesson blocks
- task cards
- prompt templates
- review checkpoints
- Git checkpoints
- debugging drills
- deployment steps
- risk warnings

The AI layer should behave like a planner and tutor over this curated library. It can personalize examples, sequence, wording, and tool choices, but the core teaching should come from maintained Coding Kitty blocks.

## Core Course Assembly Model

The course should be assembled in this order:

1. **Idea intake**
   - learner describes what they want to build
   - platform collects skill level, goal, tool preference, app type, data needs, login needs, AI needs, and deployment intent

2. **Project diagnosis**
   - platform maps the idea to a supported archetype
   - platform identifies capabilities needed for v1
   - platform identifies risky or out-of-scope features

3. **Route selection**
   - platform chooses the best beginner route
   - route decides which tools, setup steps, prompt templates, and deployment steps are shown

4. **Course map generation**
   - platform creates a sequence of build sprints
   - each sprint combines concept blocks, task blocks, prompts, checklists, and checkpoints

5. **Guided execution**
   - learner completes tasks in external AI coding tools
   - platform helps them prompt, inspect changes, debug, save checkpoints, and deploy

6. **Launch and iteration**
   - learner ships a first version
   - platform helps them test, gather feedback, and choose safe v2 improvements

## Course Hierarchy

```text
Project Archetype
  -> Tool Route
    -> Build Sprint
      -> Lesson Block
      -> Agent Task Block
      -> Prompt Template
      -> Review Checklist
      -> Git Checkpoint
      -> Debugging Drill
      -> Deployment Step
```

Example:

```text
Personal Website
  -> Replit + GitHub + Netlify route
    -> Sprint 2: Build the first page
      -> Concept: HTML page structure
      -> Task: Create homepage sections
      -> Prompt: Generate first landing page
      -> Checklist: Does the page match the goal?
      -> Git Checkpoint: Save first working page
```

## Reusable Block Types

### 1. Project Archetype Blocks

Purpose:

- define the shape of a supported learner project
- constrain personalization so the platform can stay reliable

MVP archetypes:

- personal website
- simple productivity app
- content/blog app
- simple dashboard
- lightweight AI wrapper

Fields:

- archetype id
- learner-friendly name
- best use cases
- typical features
- default tool routes
- required capabilities
- optional capabilities
- beginner risks
- first version boundaries
- recommended deployment path

### 2. Tool Route Blocks

Purpose:

- explain how the learner will build
- select setup, prompt style, checkpoints, and deployment path

MVP routes:

- browser app-builder route: Lovable or Bolt style flow
- web-code route: Replit, Cursor, GitHub, Netlify or Vercel
- lightweight AI-wrapper route: Replit or Cursor plus an API provider

Fields:

- route id
- tools required
- learner skill fit
- setup steps
- what the route is good for
- what the route is bad for
- deployment target
- version control workflow
- common failure modes

### 3. Build Sprint Blocks

Purpose:

- group lessons and tasks around one concrete product outcome

MVP sprint pattern:

- define the idea
- choose the route
- create the first screen
- add interaction
- save or persist data, if needed
- review and debug
- deploy
- improve v1

Fields:

- sprint id
- product outcome
- concepts introduced
- tasks included
- estimated time
- prerequisites
- completion criteria
- next sprint options

### 4. Concept Explainer Blocks

Purpose:

- teach only the software concept needed for the current task
- keep fundamentals practical rather than abstract

MVP concept blocks:

- what software is
- frontend vs backend vs database
- files and folders
- HTML, CSS, and JavaScript basics
- components and state
- forms and validation
- APIs
- local storage vs database
- authentication basics
- environment variables
- Git and GitHub basics
- deployment basics
- domains and DNS basics
- logs and debugging

Fields:

- concept id
- plain-English explanation
- when the learner needs it
- short example
- common misconception
- checkpoint question
- related tasks

### 5. Agent Task Blocks

Purpose:

- tell the learner exactly what to ask the AI coding tool to do
- keep AI changes small and reviewable

MVP task blocks:

- write a product one-liner
- create first project files
- build homepage layout
- style the page
- add a contact form
- add a list, card, or table
- add simple state
- save data locally
- connect a simple database, if included
- add AI API call, if included
- fix a visible bug
- prepare for deployment

Fields:

- task id
- user-facing task name
- intended product change
- prompt inputs
- expected AI output
- review checklist
- rollback warning
- completion criteria

### 6. Prompt Template Blocks

Purpose:

- help learners make good requests to AI tools
- personalize prompts without relying on the learner to know technical terms

MVP prompt templates:

- ask for a plan before building
- build one small feature
- explain what changed
- review code for beginner risks
- debug an error message
- simplify the implementation
- prepare deployment
- improve UI without changing functionality

Fields:

- prompt id
- prompt goal
- variables
- full prompt template
- tool-specific variant
- bad prompt example
- follow-up prompts

### 7. Review Checklist Blocks

Purpose:

- teach the learner to inspect AI output instead of blindly trusting it

MVP review checklists:

- visual review
- functionality review
- code-change review
- prompt alignment review
- security and privacy review
- cost and API usage review
- deployment readiness review

Fields:

- checklist id
- applies to task or sprint
- questions to answer
- pass criteria
- warning signs
- recommended next action

### 8. Git Checkpoint Blocks

Purpose:

- teach version control as practical save points

MVP Git checkpoints:

- create repository
- first commit
- commit after working UI
- commit before risky change
- read a diff
- restore from a mistake
- push to GitHub

Fields:

- checkpoint id
- when to use it
- command or tool flow
- what good looks like
- what to ask AI to explain
- recovery action

### 9. Debugging Drill Blocks

Purpose:

- normalize errors and give learners a repeatable debugging loop

MVP debugging drills:

- page looks wrong
- button does nothing
- form does not submit
- build fails
- deployment fails
- environment variable missing
- API call fails
- database connection fails, if database is included

Fields:

- drill id
- symptom
- likely causes
- evidence to collect
- prompt to ask the AI tool
- manual check
- escalation path

### 10. Deployment Blocks

Purpose:

- get the learner from local/project builder to a real public URL

MVP deployment blocks:

- static site deployment
- full-stack deployment, if simple productivity app is included
- GitHub to Netlify or Vercel
- Replit publish flow
- environment variables
- custom domain optional path
- final launch checklist

Fields:

- deployment id
- project types supported
- hosting provider
- required prerequisites
- step-by-step flow
- common errors
- verification checklist

### 11. Risk Rule Blocks

Purpose:

- keep beginner projects safe, scoped, and realistic

MVP risk rules:

- app idea is too broad
- login is not needed for v1
- payments should be deferred
- private user data needs caution
- AI API costs need limits
- database adds complexity
- mobile app is not the best first version
- real-time collaboration is too much for v1

Fields:

- rule id
- trigger condition
- learner-facing warning
- recommended simplification
- alternative v1 path
- unlock condition for later

## Shared Block Schema

Every reusable block should have a common metadata wrapper so the course generator can sequence and filter blocks.

```yaml
id: concept.frontend-basics
type: concept_explainer
title: Frontend basics
level: beginner
applies_to:
  archetypes: [personal_website, productivity_app, dashboard]
  routes: [web_code, browser_app_builder]
prerequisites: []
outputs:
  - learner can explain what the user sees and interacts with
estimated_minutes: 8
tags: [frontend, fundamentals]
variants:
  lovable: "Use when learner is building in a browser app builder."
  cursor: "Use when learner is editing a code project."
risk_flags: []
```

Recommended required fields:

- id
- type
- title
- level
- applies to archetypes
- applies to routes
- prerequisites
- outputs
- estimated time
- learner-facing content
- completion criteria
- related blocks

## MVP Block Library

The MVP should start with enough blocks to generate credible courses for one flagship path and lightweight personalization for nearby project types.

### MVP Flagship Path

The cleanest first flagship path is:

> Personal website from idea to deployed URL.

Why:

- lowest technical risk
- emotionally rewarding
- easy to explain in short-form Coding Kitty content
- deployment is straightforward
- does not require authentication, database, payments, or sensitive data
- teaches the core AI-builder workflow without overloading the learner

### MVP Stretch Path

The second path, if scope allows:

> Simple productivity app with local data persistence.

Why:

- better proves the "your idea becomes the course" promise
- introduces state, forms, data, debugging, and more app-like thinking
- useful for founders, students, and professionals

Recommended constraint:

- start with local storage or a very simple managed database
- avoid login in the first version unless there is a strong reason

## MVP Course Blocks To Build First

### Intake and Diagnosis

- idea intake form
- skill level selector
- tool preference selector
- app archetype matcher
- v1 scope reducer
- complexity warning rules
- generated project brief

### Course Map

- generated course overview
- sprint list
- required concepts
- optional concepts
- estimated completion time
- final launch goal
- "too complex for v1" fallback path

### Core Learning Blocks

- AI builder mindset
- frontend/backend/database mental model
- app anatomy
- files and folders
- prompting an AI coding agent
- reviewing AI output
- debugging with AI
- Git as save points
- deployment basics

### Personal Website Path

- define audience and purpose
- choose sections
- generate first page
- style the page
- make it responsive
- add links and contact section
- review accessibility basics
- create GitHub repo
- deploy to Netlify or Vercel
- optional custom domain
- final launch checklist

### Simple Productivity App Path

- define user workflow
- identify core entities
- build first screen
- add form
- add list/cards/table
- add state
- save data locally
- review generated code
- debug common UI issues
- deploy if route supports it

### Prompt Library

- scope my app idea
- ask for a build plan
- build one screen
- add one feature
- explain the code changes
- find bugs in this implementation
- fix this error
- make the UI clearer
- prepare this project for deployment

### Checkpoints

- after project brief
- after first generated files
- after first working screen
- before adding data persistence
- before deployment
- after successful deployment
- after first feedback-driven improvement

## MVP Platform Things Needed

### 1. Content Library

Needed:

- block registry
- markdown or CMS-backed block content
- block metadata
- block relationships
- versioned content

MVP approach:

- store blocks as structured markdown or JSON/YAML
- no complex authoring interface at first
- manually maintain the first 50-80 blocks

### 2. Idea Intake

Needed:

- app idea text field
- audience/use-case question
- skill level
- preferred route or "not sure"
- data/login/AI/deployment toggles
- complexity signal

MVP approach:

- short guided questionnaire
- avoid asking more than 8-10 questions before showing a first path

### 3. Archetype Matcher

Needed:

- rules or AI classifier that maps idea to archetype
- confidence score
- fallback when the idea is too broad

MVP approach:

- rules plus AI summary
- support only the flagship archetypes
- show a simplified v1 recommendation when unsupported

### 4. Course Assembler

Needed:

- choose route
- choose required blocks
- sequence sprints
- insert optional blocks
- personalize prompts and examples
- produce course map

MVP approach:

- deterministic templates for each archetype
- AI personalizes language, feature names, and prompts
- curated block order remains fixed enough to be reliable

### 5. Learner Course Page

Needed:

- course map
- sprint sidebar
- current task
- concept explanation
- prompt block
- checklist
- checkpoint
- completion state

MVP approach:

- simple lesson/task page
- progress stored per user/project
- no in-browser IDE yet

### 6. Prompt Builder

Needed:

- prompt templates
- user project context inserted automatically
- tool-specific wording
- copy button
- follow-up prompts

MVP approach:

- generate prompts for Replit/Cursor/browser app-builder route
- let learner copy into external tool

### 7. Progress Tracking

Needed:

- completed tasks
- current sprint
- project status
- checkpoints completed
- deployment URL

MVP approach:

- basic checklist state
- one active project per learner is enough

### 8. Version Control Guidance

Needed:

- GitHub setup guide
- checkpoint reminders
- diff review prompts
- recovery prompts

MVP approach:

- teach GitHub and simple Git concepts
- avoid advanced branching until paid/deeper paths

### 9. Deployment Guidance

Needed:

- provider-specific deployment recipe
- common failure handling
- final URL verification
- optional domain instructions

MVP approach:

- Netlify or Vercel for static personal website
- Replit publish if route uses Replit
- keep full-stack deployment optional until the productivity path is ready

### 10. Feedback and Analytics

Needed:

- track where learners drop off
- capture unsupported ideas
- collect course quality feedback
- capture deployment success rate

MVP approach:

- event tracking for intake, course generated, sprint completed, deployed
- short feedback prompt after each sprint

## Not Needed For MVP

- full in-browser IDE
- custom coding agent execution
- unlimited app idea support
- large course marketplace
- complex quizzes
- certificates
- paid cohort operations
- advanced community system
- mobile app builder support
- payments module
- production authentication module for every learner
- fully dynamic AI-generated lessons

## Minimum Useful MVP Content Count

Target starting library:

| Block category | MVP count |
| --- | ---: |
| Project archetypes | 2-3 |
| Tool routes | 2-3 |
| Build sprints | 10-14 |
| Concept explainers | 15-20 |
| Agent task blocks | 20-30 |
| Prompt templates | 15-20 |
| Review checklists | 8-12 |
| Git checkpoints | 6-8 |
| Debugging drills | 8-12 |
| Deployment blocks | 3-5 |
| Risk rules | 10-15 |

This is enough to feel personal without creating a giant curriculum upfront.

## Suggested First 30 Blocks

1. Idea intake
2. V1 scope reducer
3. Project brief generator
4. Personal website archetype
5. Simple productivity app archetype
6. Web-code route
7. Browser app-builder route
8. AI builder mindset
9. What software is
10. Frontend/backend/database mental model
11. Files and folders
12. HTML structure
13. CSS styling
14. JavaScript behavior
15. Components and state
16. Forms
17. Local storage
18. APIs
19. Git as save points
20. GitHub basics
21. Review AI output
22. Ask for a build plan prompt
23. Build one screen prompt
24. Explain changes prompt
25. Debug error prompt
26. Visual review checklist
27. Git checkpoint after working screen
28. Static deployment block
29. Deployment troubleshooting drill
30. Launch checklist

## MVP Generated Path Example

For a learner who says:

> I want a personal website that shows my projects and lets people contact me.

The generated course should be:

1. Shape the website idea
2. Choose the web-code or browser-builder route
3. Learn what the frontend is
4. Generate the first homepage
5. Review the layout and content
6. Save the first checkpoint
7. Add projects section
8. Add contact section
9. Make the page responsive
10. Deploy to a public URL
11. Test the live site
12. Plan the next improvement

For a learner who says:

> I want a to-do app that breaks big tasks into tiny steps.

The generated course should be:

1. Simplify the first version
2. Choose productivity app path
3. Learn app anatomy
4. Build task list screen
5. Add task creation form
6. Add tiny-step cards
7. Save data locally
8. Review generated code
9. Debug common issues
10. Deploy or prepare for a full-stack upgrade

## Product Decision

For the MVP, the platform should behave like:

> A guided course assembler, not an open-ended AI course generator.

This means the learner experiences personalization, but the product stays grounded, testable, and shippable.

The first version should prove three things:

1. A learner can describe an idea and receive a useful path.
2. The path can be assembled from reusable blocks.
3. The learner can use the path to build, understand, checkpoint, and deploy a real first project.
