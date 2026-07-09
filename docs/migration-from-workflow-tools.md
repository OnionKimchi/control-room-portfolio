# Migration From n8n and Activepieces

The project started with workflow automation tools because they were the fastest way to make a working prototype visible.

n8n and Activepieces were useful for:

- visualizing execution flow
- quickly wiring Discord, HTTP calls, model calls, and conditional branches
- inspecting step-level inputs and outputs during early experiments
- validating the basic product shape before committing to a custom runtime

## Why the Runtime Moved Into the Backend

As the project became more stateful, the workflow-tool layer became less comfortable as the source of truth.

The custom backend direction became more attractive because the system needed:

- explicit channel cycle state
- active-cycle interruption handling
- stale callback protection
- testable prompt compilation
- code-reviewed branch logic
- secret references instead of tool-owned credentials
- durable trace records shaped for the frontend
- provider/tool adapters with stable contracts

The goal was not to reject workflow tools entirely. The goal was to keep the workflow visibility they provided while moving execution authority into code.

## What Stayed From the Workflow Phase

The workflow-tool phase clarified the product model:

- Discord is the main user-facing surface
- rooms bind participants, prompts, model assignments, and tool assignments
- a conductor-style decision step is useful for routing turns
- execution needs traceability at the node level
- operators need visibility into compiled prompts and runtime state

## What Changed

| Before | After |
| --- | --- |
| Visual workflow runtime owns execution | Backend graph runner owns execution |
| Credentials stored in workflow-tool connections | Backend-owned secret references and masking |
| Implicit step payloads | Explicit run envelopes and node trace contracts |
| Debugging through workflow run UI | Debugging through backend traces and frontend visualization |
| Harder-to-test branch behavior | Code-owned branch logic and stale-cycle checks |

## Portfolio Note

This section should eventually include a personal account of the migration decision:

- what the first prototype proved
- where the workflow tool started slowing progress
- which bug or limitation forced the rethink
- what became easier after the backend rebuild
- what remained difficult even after the migration
