# CODEX_KICKOFF_PROMPT.md

Use the attached `PROJECT_SPEC.md` as the source of truth.

Your job is to scaffold and implement the MVP for **Friend Screen Share**.

## Core instruction
This product is:

**AltSendme’s low-friction product spirit, rebuilt as a proper WebRTC screen sharing app.**

Do **not** attempt to replicate AltSendme’s transport layer for streaming.
Use AltSendme only as **UX inspiration**.
This product must use **WebRTC** for live screen sharing and a lightweight **signalling service** for negotiation.

## Primary goal
Build a dead-simple no-account screen sharing app for quick one-to-one sessions.

### Required user flow
Host:
1. Open desktop app
2. Click **Share my screen**
3. Grant screen capture permission
4. App creates a session
5. App shows a 6-character code
6. Host sends code to friend
7. Viewer joins
8. Host sees connected/live state

Viewer:
1. Open web app
2. Enter 6-character code
3. Join session
4. Receive live host screen stream

## Product constraints
Do not add:
- accounts
- chat
- recording
- file transfer
- remote control
- multi-user rooms
- enterprise features

Keep the MVP tight.

## Technical requirements
- Monorepo
- `apps/desktop` = Tauri + React + TypeScript
- `apps/web` = React + TypeScript
- `apps/signal` = Node.js + TypeScript signalling server
- `packages/protocol` = shared message types and schemas
- `packages/rtc` = shared WebRTC helpers
- `packages/ui` = shared UI primitives if useful
- `packages/config` = shared lint/tsconfig/prettier config

## Required architecture
- WebRTC for live media
- WebSocket signalling
- short-lived 6-character session codes
- signalling server must not store or proxy media
- in-memory session storage for MVP
- strict TypeScript
- runtime validation for signalling messages
- clear session lifecycle and cleanup
- prepare STUN/TURN config through environment variables

## Required docs
Create and keep aligned with implementation:
- `docs/product/mvp.md`
- `docs/adr/0001-architecture.md`
- `docs/adr/0002-signalling-and-session-codes.md`
- `docs/security/threat-model.md`
- `docs/testing/manual-test-checklist.md`
- `docs/deployment/signal-service.md`
- root `README.md`

## Execution order
Follow this exact order:

### Phase 1 — scaffold
- create the monorepo
- configure pnpm workspaces
- configure Turborepo
- configure strict TypeScript
- configure lint/format
- create placeholder app entrypoints
- make sure the repo builds and typechecks cleanly

### Phase 2 — shared protocol
- define signalling message types
- define runtime schemas
- define session model
- export a clean package API

### Phase 3 — signalling server
- create session
- join session
- WebSocket relay
- offer/answer/ICE relay
- session state transitions
- inactivity expiry
- cleanup
- structured logs

### Phase 4 — desktop host app
- host home screen
- create session flow
- request screen capture
- display join code
- status updates
- end session flow

### Phase 5 — WebRTC host implementation
- create peer connection
- add screen video track
- create and relay offer
- receive answer
- relay ICE candidates
- cleanup on exit/end

### Phase 6 — web viewer app
- join screen
- code validation
- join flow
- receive remote stream
- render remote video
- status/error states
- fullscreen support

### Phase 7 — hardening
- timeout handling
- expired/invalid code UX
- disconnect cleanup
- STUN/TURN env support
- manual QA checklist
- deployment notes

### Phase 8 — polish
- refine UI
- refine copy
- ensure docs match implementation
- do not add non-MVP features

## UX requirements
The UI should feel:
- calm
- modern
- simple
- consumer-friendly
- not enterprise
- not cluttered

Use plain-English statuses like:
- Creating your session…
- Waiting for your viewer…
- Connecting…
- Live
- That code is invalid or expired.
- Screen access was denied.
- The host ended the session.

## Definition of done
The task is complete when:
1. the monorepo builds cleanly
2. local dev is documented
3. host can create a session
4. a 6-character code is shown
5. viewer can join using the code
6. WebRTC connects locally
7. viewer sees the host screen
8. disconnect and expiry states are handled sensibly
9. docs are present and useful

## Output expectations
Do not hand-wave.
Do not leave major TODO placeholders in core flows.
Prefer a working happy path over speculative abstraction.
Keep the codebase clean, modular, and explainable.

Start now by scaffolding the repository according to `PROJECT_SPEC.md`.
