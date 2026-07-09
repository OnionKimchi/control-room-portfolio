# Component Responsibilities

## Discord Ingress

Discord ingress is the user-facing entry point. It listens to Discord gateway events, ignores bot messages, applies configured guild/channel filters, normalizes message payloads, and sends them to the backend API.

It does not own workflow execution. Its job is to record facts and let the backend decide whether a new graph run should start.

## Backend API

The backend API is the control plane. It owns durable state, room configuration, channel message history, channel cycle state, prompt compilation, model profile resolution, secret reference handling, run creation, and trace storage.

The backend replaces the workflow runtime role previously held by n8n or Activepieces. Instead of passing fragile data between visual nodes, it uses explicit contracts for graph nodes, run envelopes, and trace snapshots.

## Frontend Console

The frontend console is an operator tool. It should make the system inspectable without becoming the execution authority.

Expected responsibilities:

- edit rooms, participants, prompts, model assignments, and tool assignments
- show secret configured/missing status without exposing secret values
- preview compiled prompts
- show the backend-owned workflow graph
- inspect runs, node statuses, sanitized inputs/outputs, and errors

## Runner API

The runner API is a local boundary for execution that should not happen directly inside the browser. It exposes readiness and execution endpoints and gives the backend a controlled way to call local tools.

For the public portfolio, the runner is important because it shows the project was not only a web UI. It had a concrete local runtime boundary.

## Runner Desktop App

The runner desktop app is a small Windows utility for local operation. It shows Docker engine status, backend stack status, frontend console availability, and runner API readiness. It also provides quick actions such as opening the console and viewing Docker logs.

Screenshot needed:

- `assets/screenshots/runner-app.png`

## Backend Without a Screenshot

The backend does not need a screenshot. It should be represented with architecture diagrams, responsibility tables, API flow descriptions, and migration notes.
