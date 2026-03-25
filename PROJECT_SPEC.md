# PROJECT_SPEC.md

## Friend Screen Share
**Version:** 0.1 MVP  
**Status:** Codex build brief  
**Product type:** Lightweight one-to-one screen sharing app  
**Primary inspiration:** AltSendme’s simplicity and no-account pairing UX, adapted for live screen sharing rather than file transfer

---

## 1. Executive Summary

Friend Screen Share is a dead-simple, no-account screen sharing product designed for fast one-to-one sessions between friends, family, or colleagues.

The core use case is:
- “Can you look at my screen for a minute?”
- “Can you show me how to do this?”
- “Can you quickly check whether this looks right?”
- “Can you help me fix something?”

The product must feel dramatically lighter than Zoom, Meet, Teams, or Discord for quick support and casual collaboration.

### Core promise
**Share your screen with a friend in seconds. No account, no meeting link, no faff.**

### Product philosophy
This app is not a meetings platform.  
It is not a webinar platform.  
It is not remote desktop software.  
It is a **frictionless live screen sharing utility**.

---

## 2. Product Vision

Build the fastest, simplest way for one person to share their screen with one other person.

A host opens a desktop app, clicks **Share my screen**, gets a short code, sends that code to a friend, and the friend joins from a web page. The viewer sees the screen almost immediately, without signup, without account creation, and without installing a heavy communication tool.

### Desired emotional feel
- Fast
- Calm
- Clear
- Trustworthy
- Friendly
- Lightweight
- Zero ceremony

### UX inspiration
Take inspiration from AltSendme’s:
- simplicity
- short-lived pairing model
- no-account feel
- direct-connection mindset

Do **not** copy AltSendme’s transport model for live media.  
This is a real-time streaming product and must be built using **WebRTC** with a lightweight signalling layer.

---

## 3. Goals

### Primary goals
1. Let a host start screen sharing in under 15 seconds
2. Let a viewer join using only a short code
3. Avoid account creation entirely for MVP
4. Keep the architecture simple and honest
5. Prefer direct peer media connection where possible
6. Make error states obvious and understandable
7. Deliver a working end-to-end MVP that a new developer can run locally

### Success criteria
The MVP is successful if:
- a host can launch the desktop app
- the host can start a session
- the host sees a 6-character code
- a viewer can open the web app and enter the code
- the viewer can receive and view the host’s screen
- both users get sensible feedback during connection and disconnection
- local development is documented and repeatable

---

## 4. Non-Goals

The following are **explicitly out of scope** for MVP:

- user accounts
- login
- team workspaces
- chat
- multi-user sessions
- recording
- cloud playback
- file transfer
- remote control
- clipboard sync
- annotation
- voice/video calls as a core feature
- enterprise admin tooling
- calendar/scheduling
- permanent meeting rooms

These can be considered later, but must not distort the MVP architecture.

---

## 5. Core Use Cases

### Use case 1: friend helping friend
A user cannot work out how to do something on their computer. They open Friend Screen Share, start sharing, send the code, and their friend watches in browser.

### Use case 2: quick collaboration
A user wants to show a design, a web page, a bug, or a workflow without setting up a meeting.

### Use case 3: family tech support
A parent, sibling, or non-technical friend needs someone to see what is happening on their screen.

### Use case 4: visual confirmation
Someone wants another person to quickly check a layout, setting, page, or output.

---

## 6. Target Users

### Primary audience
- everyday users
- freelancers
- friends and family
- non-technical users needing help
- technical users who want something lighter than meeting software

### User characteristics
- impatient with setup
- may not want to create accounts
- may not trust heavy software install flows
- often want a 2–10 minute session
- value speed and simplicity over features

---

## 7. Platform Strategy

### MVP platforms

#### Host
- **Desktop app**
- Windows-first
- Built with **Tauri + React + TypeScript**

#### Viewer
- **Web app**
- Chromium-based browsers first
- Built with **React + TypeScript**

### Why this split
This gives the strongest MVP shape:
- host experience feels app-like and intentional
- viewer join flow stays ultra-light
- viewer does not need to install anything
- architecture remains cross-platform-friendly later

### Future platform expansion
Later, if the MVP works well:
- macOS host
- Linux host
- desktop viewer app
- browser-based host flow where supported

---

## 8. Product Principles

### 8.1 Simplicity first
Every extra step hurts the product. Minimise inputs, toggles, and decisions.

### 8.2 One-to-one first
The product should feel purpose-built for a fast connection between two people.

### 8.3 Privacy by default
Do not store media. Keep session data minimal and short-lived.

### 8.4 Honest architecture
Do not claim impossible privacy or networking guarantees. Explain what the signalling layer can and cannot see.

### 8.5 Direct connection where possible
Use WebRTC peer-to-peer media transport where available, with STUN/TURN support for hard NAT cases.

### 8.6 Clear failure states
Users should know whether the issue is:
- invalid code
- expired code
- permissions denied
- unsupported browser
- network failure
- host disconnected

### 8.7 Build for real-world networks
Assume NAT, firewalls, unstable Wi-Fi, and occasional browser weirdness.

---

## 9. High-Level User Experience

### Host journey
1. Open desktop app
2. Click **Share my screen**
3. Approve screen capture
4. App creates session
5. App shows a 6-character code
6. Host sends code to viewer
7. Viewer joins
8. Host sees status change to live
9. Host ends session when finished

### Viewer journey
1. Open web app
2. Enter 6-character code
3. Click **Join**
4. Viewer connects
5. Viewer sees live screen stream
6. Viewer can fullscreen if needed
7. Session ends when host ends or disconnects

---

## 10. MVP Functional Requirements

### 10.1 Host requirements
The desktop host app must allow the user to:
- launch the app
- click a primary CTA to start sharing
- grant screen capture permission
- create a new session
- display a short session code
- display session status
- end the session
- copy the code
- optionally copy a join link if configured

### 10.2 Viewer requirements
The web viewer must allow the user to:
- load a join screen
- enter a 6-character code
- validate code format client-side
- join a session
- receive a live remote stream
- view the stream in a video element
- fullscreen the stream
- see clear status and error messaging

### 10.3 Session requirements
The system must support:
- creation of a short-lived session
- one host
- one viewer
- session code lookup
- connection state tracking
- signalling message relay
- session expiry after inactivity
- clean end-session flow
- cleanup of abandoned sessions

### 10.4 Media requirements
The MVP must support:
- one video track from host screen capture
- live streaming to one viewer
- optional audio only if it is clean and stable to implement

The MVP does **not** need:
- webcam
- multi-track complexity
- multiple viewers
- recording
- bitrate controls in UI

---

## 11. UX States

### 11.1 Host states
The host app must visibly support these states:

- `idle`
- `creating_session`
- `permission_prompt`
- `permission_denied`
- `waiting_for_viewer`
- `connecting`
- `live`
- `viewer_left`
- `ending`
- `session_ended`
- `network_error`
- `unsupported`

### 11.2 Viewer states
The viewer app must visibly support these states:

- `idle`
- `joining`
- `looking_for_session`
- `connecting`
- `live`
- `invalid_code`
- `expired_code`
- `host_disconnected`
- `session_ended`
- `network_error`
- `unsupported`

### 11.3 UX messaging style
Messages must be:
- plain English
- short
- reassuring
- specific

Examples:
- “Creating your share session…”
- “Waiting for your viewer to join.”
- “Screen access was denied.”
- “That code is invalid or has expired.”
- “The host ended the session.”
- “Connection failed. Please try again.”

---

## 12. Core Technical Architecture

### 12.1 Overview
The product consists of three main runtime pieces:

1. **Desktop host app**
2. **Web viewer app**
3. **Signalling server**

Media must flow over **WebRTC**.  
Signalling must flow over **WebSocket**.

The signalling service must **not** proxy media in MVP.  
It should only help peers discover each other and exchange negotiation data.

### 12.2 Why WebRTC
WebRTC is the correct fit because it is built for:
- real-time media
- low-latency peer communication
- ICE/STUN/TURN NAT traversal
- browser-compatible streaming

Do **not** attempt to adapt a file transfer transport model into a live media transport model.

### 12.3 Why a separate signalling service
WebRTC peers still need a method to exchange:
- offers
- answers
- ICE candidates
- session state

A small signalling server keeps the design clean and minimal.

### 12.4 Why Tauri for desktop host
Tauri provides:
- lightweight desktop packaging
- web tech frontend
- native shell benefits
- good fit for a Windows-first utility

### 12.5 Why a browser viewer
A browser viewer reduces friction dramatically:
- no install required
- easy invite flow
- excellent for casual joining
- suits the product promise

---

## 13. Monorepo Structure

```text
friend-screen-share/
  apps/
    desktop/
    web/
    signal/
  packages/
    protocol/
    rtc/
    ui/
    config/
  docs/
    product/
    adr/
    security/
    testing/
    deployment/
```

### Proposed package purposes

#### `apps/desktop`
Tauri desktop host app

#### `apps/web`
Browser join/viewer app

#### `apps/signal`
WebSocket signalling service

#### `packages/protocol`
Shared message types, schemas, session models

#### `packages/rtc`
Shared WebRTC helper utilities

#### `packages/ui`
Optional shared UI primitives

#### `packages/config`
Shared TypeScript, lint, format config

---

## 14. Recommended Stack

### Workspace and tooling
- pnpm
- Turborepo
- TypeScript
- ESLint
- Prettier

### Desktop app
- Tauri
- React
- Vite
- TypeScript

### Web app
- React
- Vite
- TypeScript

### Signalling server
- Node.js
- TypeScript
- WebSocket server

### Validation
- Zod for runtime schema validation

### Testing
- Vitest for unit tests where useful

### Styling
Prefer one of:
- Tailwind
- CSS Modules

Keep styling calm and minimal.

---

## 15. Shared Protocol Design

### 15.1 Protocol goals
The signalling protocol must be:
- typed
- validated at runtime
- small
- explicit
- easy to debug

### 15.2 Required messages
At minimum support these messages:

- `create_session_request`
- `create_session_response`
- `join_session_request`
- `join_session_response`
- `host_ready`
- `viewer_ready`
- `offer`
- `answer`
- `ice_candidate`
- `session_state`
- `error`
- `end_session`
- `ping`
- `pong`

### 15.3 Session model
Each session should track at minimum:

- `sessionId`
- `sessionCode`
- `createdAt`
- `expiresAt`
- `state`
- `hostConnected`
- `viewerConnected`

Optional internal fields:
- socket references
- cleanup timers
- last activity timestamp

### 15.4 Runtime validation
All inbound signalling messages must be validated.  
Malformed or unexpected messages must be safely rejected.

---

## 16. Session Code Design

### 16.1 Code format
The session code must be:
- 6 characters
- human-friendly
- case-insensitive
- easy to read aloud

Avoid ambiguous characters where possible:
- `O` vs `0`
- `I` vs `1`
- `S` vs `5`

### Example character set
Use something like:
`ABCDEFGHJKLMNPQRSTUVWXYZ23456789`

### 16.2 Code lifecycle
Codes must be:
- short-lived
- invalid after session expiry
- invalid after host ends session
- invalid after cleanup

### 16.3 Code expiry
Initial MVP defaults:
- expire session if no viewer joins after configurable timeout
- expire stale abandoned sessions
- clean up ended sessions promptly

---

## 17. Session Lifecycle

### 17.1 Normal happy path
1. Host opens app
2. Host requests session creation
3. Signalling server creates session
4. Host receives session code
5. Host starts or prepares screen capture
6. Viewer enters code
7. Viewer joins session
8. Offer/answer flow completes
9. ICE candidates exchanged
10. Stream becomes live
11. Host or viewer ends session
12. Session closes and cleans up

### 17.2 Host disconnect
If host disconnects:
- viewer must be informed
- session should end
- resources cleaned up

### 17.3 Viewer disconnect
If viewer disconnects:
- host should be informed
- host may remain active briefly if reconnect support is low effort
- otherwise session may remain waiting or end, depending on chosen policy

### 17.4 Expiry flow
If no viewer joins in time:
- session expires
- host UI reflects expiry or timeout
- code becomes invalid
- cleanup runs

---

## 18. Desktop Host App Specification

### 18.1 Main host screen
The host home screen should have:
- clear title
- one primary CTA: **Share my screen**
- lightweight explanation text

### Example copy
**Share your screen with a friend**  
Start a session, send them the code, and they can watch instantly.

### 18.2 Share setup flow
On clicking the primary CTA:
- request screen capture
- create session
- show session code
- show current state

The host screen should prominently show:
- the join code
- copy button
- session status
- end session button

### 18.3 Host UI structure
Suggested layout:
- header/title
- session card
- large code display
- status line
- actions row
- error message area

### 18.4 Host behaviors
The host must:
- dispose media tracks on end
- close RTCPeerConnection on end
- disconnect signalling socket cleanly
- handle denied permissions gracefully
- not leave stale sessions when app closes

---

## 19. Web Viewer Specification

### 19.1 Join screen
The viewer landing screen should include:
- app title
- short explanation
- code input
- join button

### Example copy
**Join a screen share**  
Enter the 6-character code your friend sent you.

### 19.2 Viewer screen
Once connecting/live, show:
- status
- video view
- fullscreen option
- retry or back action on failure

### 19.3 Viewer video handling
The viewer must:
- bind remote `MediaStream` to a video element
- handle autoplay restrictions gracefully
- provide a clear play interaction if browser policy requires it

### 19.4 Viewer failure cases
The UI must clearly distinguish:
- invalid code
- expired code
- host offline
- unsupported browser
- network failure

---

## 20. WebRTC Implementation Requirements

### 20.1 Peer connection setup
Shared RTC utilities should help with:
- creating peer connection
- registering event listeners
- adding local tracks
- receiving remote tracks
- ICE handling
- cleanup

### 20.2 Host responsibilities
The host must:
- create a peer connection
- add the screen video track
- create/send offer when appropriate
- relay ICE candidates
- handle answer
- manage disconnect cleanup

### 20.3 Viewer responsibilities
The viewer must:
- create a peer connection
- receive offer or participate in negotiation flow
- produce/send answer
- relay ICE candidates
- assemble remote stream
- display stream

### 20.4 ICE configuration
ICE server config should be supplied via environment variables.

Must support:
- STUN URLs
- TURN URLs
- TURN username
- TURN credential

Do not hardcode secrets.

---

## 21. Signalling Server Specification

### 21.1 Role
The signalling server exists to:
- create sessions
- map session codes to active sessions
- relay negotiation messages
- track session state
- expire inactive sessions
- clean up memory

It does **not**:
- store media
- record media
- proxy media in MVP
- provide identity/accounts

### 21.2 Transport
Use WebSockets for active session messaging.

Optional simple HTTP endpoints may exist for:
- health checks
- debugging in dev
- create/join bootstrap if useful

### 21.3 Session storage
Use in-memory storage for MVP.

Document clearly:
- this is acceptable for development and early MVP
- this is not horizontally scalable
- a Redis-backed session store is the likely next step

### 21.4 Logging
Use structured logs for:
- session created
- session joined
- offer relayed
- answer relayed
- ICE relayed
- session ended
- session expired
- malformed message
- socket disconnected

---

## 22. State Management Approach

### 22.1 UI state
Prefer light local React state unless complexity requires more.

Only use Zustand if state becomes meaningfully shared and cumbersome.

### 22.2 Connection state
Represent connection/session states explicitly, not with loose booleans.

Example:
- `idle`
- `creating`
- `waiting`
- `connecting`
- `live`
- `ended`
- `error`

This will keep UX and debugging much cleaner.

---

## 23. Error Handling Philosophy

Errors must:
- be visible
- be specific
- not rely only on console logs
- not trap users in confusing states

### 23.1 Required error cases
The system must handle:
- permission denied
- unsupported browser
- signalling unavailable
- invalid code
- expired code
- duplicate viewer in one-viewer session
- host disconnected
- WebRTC negotiation timeout
- ICE failure
- unexpected session termination

### 23.2 User-facing style
Avoid overly technical wording unless it is useful.

Good:
- “The code is invalid or expired.”
- “Screen access was denied.”
- “We couldn’t connect the session.”

Bad:
- “SDP negotiation failed.”
- “ICE gathering timeout exceeded.”

Technical detail can remain in logs.

---

## 24. Security and Privacy Requirements

### 24.1 Security goals
- no accounts
- minimal stored metadata
- no media retention
- short session windows
- clear lifecycle cleanup
- no false privacy promises

### 24.2 What the signalling server can see
The signalling layer may see:
- session creation metadata
- session code
- timestamps
- connection state
- negotiation messages such as SDP and ICE payloads
- IP-related networking details depending on implementation context

This must be documented honestly.

### 24.3 What the signalling server should not do
It must not:
- store media
- record streams
- build user profiles
- keep session history longer than required for runtime operation

### 24.4 Threats to document
Threat model must discuss:
- session code guessing
- hijack attempts
- malformed messages
- denial-of-service concerns
- browser permission risks
- accidental metadata retention
- limitations of the MVP architecture

### 24.5 Mitigations
MVP mitigations should include:
- short code expiry
- one-viewer-only session locking
- runtime validation
- fast cleanup
- minimal logging retention
- configurable timeouts

---

## 25. Accessibility and Usability Considerations

Even as a lightweight MVP, the product should aim for:
- clear contrast
- obvious status text
- keyboard-usable code input
- sensible button labels
- large readable join code
- no unnecessary motion or clutter

Do not over-design.  
The product should feel obvious in under five seconds.

---

## 26. Performance Expectations

### 26.1 MVP expectations
The product should aim for:
- fast app startup
- fast session creation
- reasonable connection speed on home internet
- minimal UI blocking
- efficient cleanup after session end

### 26.2 Non-goals for performance
Do not prematurely overengineer:
- custom media pipelines
- adaptive streaming dashboards
- advanced telemetry
- multi-quality control UIs

Keep the implementation simple and stable.

---

## 27. Configuration

### 27.1 Environment variables
Support the following kinds of variables where relevant:

#### Signalling server
- `PORT`
- `HOST`
- `SESSION_WAITING_TIMEOUT_MS`
- `SESSION_IDLE_TIMEOUT_MS`
- `SESSION_LIVE_GRACE_TIMEOUT_MS`

#### WebRTC
- `RTC_STUN_URLS`
- `RTC_TURN_URLS`
- `RTC_TURN_USERNAME`
- `RTC_TURN_CREDENTIAL`

#### App URLs
- `WEB_VIEWER_BASE_URL`
- `SIGNAL_SERVER_URL`

### 27.2 Config principles
- environment-driven
- no secrets hardcoded
- documented in `.env.example` files
- sensible local defaults where safe

---

## 28. Documentation Deliverables

The repo must include the following docs:

### Product
- `docs/product/mvp.md`

### ADRs
- `docs/adr/0001-architecture.md`
- `docs/adr/0002-signalling-and-session-codes.md`

### Security
- `docs/security/threat-model.md`

### Testing
- `docs/testing/manual-test-checklist.md`

### Deployment
- `docs/deployment/signal-service.md`

### Root docs
- `README.md`

---

## 29. Manual Test Requirements

A manual QA checklist must verify:

- host can create a session
- session code displays correctly
- viewer can join using valid code
- invalid code is rejected
- expired code is rejected
- host can grant screen capture
- denied permissions are handled
- viewer can receive stream
- session ends correctly
- disconnects are handled
- cleanup runs
- unsupported browser states are clear
- local dev instructions work on a fresh machine

---

## 30. Production-Readiness Lite

The MVP should include enough structure for an early real deployment of the signalling server.

Must include:
- environment-based config
- health endpoint
- structured logging
- WebSocket deployment notes
- reverse proxy notes
- explicit warning about in-memory sessions
- note on likely Redis migration later

This is not enterprise hardening.  
It is basic operational sanity.

---

## 31. Recommended Development Order

### Phase 1 — foundation
- scaffold monorepo
- configure tooling
- create README files
- create docs
- create placeholder app entrypoints

### Phase 2 — shared protocol
- define message types
- define schemas
- define session model
- export clean API

### Phase 3 — signalling server
- session creation
- session join
- WebSocket relay
- state transitions
- expiry and cleanup

### Phase 4 — desktop host app
- home screen
- create session flow
- capture permission flow
- show code
- basic status UI

### Phase 5 — WebRTC host side
- peer connection creation
- add screen track
- offer generation
- ICE relay
- cleanup

### Phase 6 — web viewer
- join page
- code validation
- connection flow
- receive answer/offer path
- render video

### Phase 7 — hardening
- timeout handling
- TURN/STUN config
- better messages
- cleanup edge cases
- test checklist

### Phase 8 — polish
- visual refinement
- copy refinement
- deployment notes
- documentation cleanup

---

## 32. Codex Build Instructions

The implementation should be broken into bounded tasks, not one huge vague request.

### Important instruction to Codex
Do not attempt to reproduce AltSendme’s transport model for streaming.  
Use AltSendme only as UX inspiration.  
This product must use **WebRTC** for live screen sharing and a lightweight signalling service for negotiation.

### Engineering expectations
- strict TypeScript
- clean file organisation
- reusable modules
- runtime validation
- no hand-wavy placeholders
- documented local setup
- working happy path first
- minimal but polished UI

---

## 33. Repo Scripts Expectations

At root, provide clear scripts such as:

- `pnpm dev`
- `pnpm build`
- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`

Where useful, also provide:
- `pnpm dev:desktop`
- `pnpm dev:web`
- `pnpm dev:signal`

Commands must be documented.

---

## 34. UI Tone and Brand Direction

The visual style should be:
- modern
- minimal
- calm
- unbloated
- slightly friendly, not playful
- utility-first, not corporate

Avoid:
- clutter
- too many cards
- noisy gradients
- gimmicks
- enterprise dashboard vibes

This is a consumer-friendly utility.

---

## 35. Suggested UI Copy

### Host home
**Share your screen with a friend**  
Start a session, send them the code, and they can watch instantly.

Button:
**Share my screen**

### Waiting state
**Your session is ready**  
Send this code to your viewer.

### Viewer join
**Join a screen share**  
Enter the code your friend sent you.

Button:
**Join**

### Invalid code
**That code is invalid or has expired.**

### Permission denied
**Screen access was denied. Please allow screen sharing and try again.**

### Session ended
**The session has ended.**

---

## 36. Future Extensions

These are possible future features, but must not affect MVP design unnecessarily:

- remote control
- clipboard sync
- file drop
- voice chat
- text chat
- annotations
- multi-viewer sessions
- browser host mode
- persistent identities
- saved recent sessions
- native desktop viewer
- better reconnection flows
- relay fallback media architecture

---

## 37. Risks and Constraints

### Product risks
- users may compare it to full meeting apps
- users may expect remote control
- users may assume any code always works instantly on hostile networks

### Technical risks
- NAT traversal failures without TURN
- screen capture differences across platforms
- browser autoplay restrictions
- WebRTC debugging complexity
- Tauri-specific edge cases on desktop packaging

### Mitigation approach
- keep scope tight
- document limits clearly
- support TURN config early
- make errors understandable
- focus on one clean happy path first

---

## 38. Definition of Done for MVP

The MVP is done when:

1. The repo scaffolds and builds cleanly
2. Docs are present and useful
3. The signalling server runs locally
4. The desktop host app runs locally
5. The web viewer app runs locally
6. Host can create a session
7. Host gets a 6-character code
8. Viewer can join with code
9. WebRTC negotiation succeeds in local testing
10. Viewer sees the host’s screen
11. Session end/disconnect is handled gracefully
12. A new developer can clone the repo and follow the docs successfully

---

## 39. Final Build Note

This project should be treated as:

**AltSendme’s low-friction product spirit, rebuilt as a proper WebRTC screen sharing app.**

That means:
- keep the pairing flow simple
- keep the UX almost frictionless
- do not overbuild
- do not drift into meetings software
- do not drift into remote control software
- deliver the fastest possible one-to-one live screen share experience

---

## 40. One-Line Product Positioning

**Share your screen with a friend in seconds. No account, no meeting link, no faff.**
