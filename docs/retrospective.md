# Retrospective

This page is intentionally separate from the README. The README should stay short and recruiter-readable. This file can be more personal and technical.

## Initial Motivation

TBD with author input.

Prompts to answer:

- What were you trying to automate or coordinate?
- Why did a Discord-first interface make sense?
- What was painful about existing tools or manual operation?
- What did you want the system to feel like when it worked?

## Technical Decisions

Early decision points:

- use Discord as the interaction surface
- prototype workflow orchestration with n8n and Activepieces
- move orchestration authority into a TypeScript backend
- keep a frontend operator console for visibility and editing
- keep a local runner boundary for actions that should not run in the browser

## Hard Parts

Likely topics to refine:

- channel cycle state
- user interruption while a run is active
- stale output prevention
- prompt compilation structure
- secret boundaries
- balancing visual workflow UX with code-owned execution

## What I Would Improve

TBD after the main public case study is stable.

Possible angles:

- narrower first milestone
- earlier trace schema design
- stronger test coverage around cycle semantics
- clearer separation between prototype workflow and production runtime
