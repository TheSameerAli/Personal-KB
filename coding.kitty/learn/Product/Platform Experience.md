---
title: "coding.kitty/learn — Platform Experience"
tags: [coding-kitty-learn, platform, ux, product]
type: product-design
status: draft
date: 2026-06-08
---

# Platform Experience

## Experience Goal

The platform should feel like an interactive build companion: learners always know what they are building, what to ask the AI agent, what changed, and what to do next.

The first major interaction should be idea intake, not course browsing.

The platform should ask what the learner wants to build, diagnose the project shape, recommend a build route, then generate a personalized course from reusable learning blocks.

## Core Components

- app idea intake
- project diagnosis
- personalized course map
- route selector: web, mobile, automation, or not sure
- lesson sidebar
- project checklist
- guided task flow
- prompt builder
- code or artifact workspace
- progress tracking
- checkpoints
- version control steps
- deployment steps
- final project showcase

## Idea Intake Flow

The intake should collect only enough information to shape the course:

- app idea
- audience or use case
- web, mobile, or undecided
- whether accounts/login are needed
- whether data needs to be saved
- whether the app needs AI features
- learner skill level
- preferred AI coding tool, if any

The output should be:

- recommended project archetype
- recommended first build route
- concepts the learner will need
- concepts they can skip for v1
- estimated course length
- first build sprint

## Personalized Course Map

Each generated course should show:

- the final product goal
- build sprints
- lessons attached to each sprint
- prompts to run in the learner's AI coding tool
- review checkpoints
- version control checkpoints
- deployment milestone
- final launch checklist

## Guided Agent Task Pattern

Example task:

> Add a contact form to your website.

Steps:

1. Describe what the form should collect.
2. Generate a prompt.
3. Run it in the chosen tool.
4. Review what changed.
5. Save a version control checkpoint.
6. Debug if it fails.
7. Mark the task complete.
