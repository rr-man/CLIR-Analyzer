# CLIR Analyzer — Changelog

All notable changes to the CLIR Analyzer tool are documented here.
This project follows a simplified [Semantic Versioning](https://semver.org/) scheme (MAJOR.MINOR.PATCH).

---

## v1.6.0 — Current

### Added
- Version number now displayed in the header (next to the title) and in the footer.
- Left-aligned badge row: all status badges (direction, answered, recording, domestic/international, queue, agent, device, etc.) are now grouped in a neat row on their own line below the caller name.
- A-leg / B-leg explanation box in the Call legs section that adapts to call direction:
  - Inbound: A-leg = outside caller, B-leg = internal extension that answered.
  - Outbound: A-leg = internal extension that placed the call, B-leg = external number dialled.
  - Internal: A-leg = extension that placed the call, B-leg = extension that was called.
- Clearer Outbound / Domestic call indicators (🏠 Domestic / 🌐 International badge with country).

### Changed
- Corporate footer simplified: "Coeo Solutions · CLIR Analyzer v1.6.0 · parses PBXware AGI call traces · Internal use only · Confidential · © [year]".

---

## v1.5.0

### Added
- Always-visible help captions under the Talk time and Wait time values in the Full Details tab (replaced hover tooltips that were not rendering reliably).
  - Talk time: "Live conversation with the client, after the call was answered. Silence / dead air within it is not measured."
  - Wait time: "Time the caller waited before answer — ringing or holding in the queue. Not conversation time."
- Compact colour legend key at the top of the Full Details tab (before the Call legs section), explaining green / orange / red / grey.

### Changed
- Call Time Breakdown colours finalised: **green = talk time**, **orange = ring / wait time**.
- Relabelled donut metrics from "Routing/Handling" to "Wait" and "Talk Time" for clarity.
- Added a note that dead air / silence within a call is not measured from the AGI trace.

---

## v1.4.0

### Added
- Full Details tab reorganised into numbered sections with headers:
  1. Call legs — who called whom (A-leg / B-leg)
  2. Destination & digits
  3. Features & recording
  4. Routing & channel
  5. Device / UAD
  6. Queue / call center & inbound routing (queue/AI calls only)
  7. Operation times & capacity
- Inline legend pills next to coloured values (e.g. "connected", "recording on", "feature on") to explain what each colour means in context.

---

## v1.3.0

### Added
- Telnyx Voice AI path detection: when the AI forwarding number (3126515380) appears in a trace, the call is flagged as having gone through the Telnyx voice AI and transferred back to the queue (InitSupport / 501).
  - "🤖 Voice AI (Telnyx)" badge, timeline note, and a Voice AI path row in the Inbound routing card.
  - AI number is a single editable constant in the code (AI_NUMBERS).

### Fixed
- Removed an earlier incorrect assumption that extension 251 was a voice-AI seat; 251 is a normal local extension.

---

## v1.2.0

### Added
- Queue / call-center detection for inbound calls:
  - Queue name (e.g. InitSupport), answering agent (e.g. Agent/2740), hold/wait time, talk time.
  - "Disconnected by" now uses the real HANGUP_REASON when present (e.g. Caller), instead of an inferred guess.
  - Inbound routing card: original DID, DID trunk, RDNIS.
- Voicemail flow handling: correct timeline (did-not-answer → routed to voicemail), mailbox, greeting type, and ring-before-voicemail time. No false "answered" event.

### Fixed
- Duration for queue calls now uses ATALKTIME (talk) and HOLDTIME (wait) since queue calls do not populate ANSWEREDTIME.
- Dial parser handles bare-extension dials (e.g. PJSIP/254) in addition to trunk-routed dials.

---

## v1.1.0

### Added
- Device / UAD detection — identifies what device an internal user is on:
  - Desk phone vs softphone/gloCOM vs mobile vs WebRTC.
  - Yealink model extraction (T46S, T54W, W73P, etc.), plus Polycom/Poly, Grandstream, Cisco, Zoiper, Bria and others.
  - Honest fallback when the SIP User-Agent isn't in the trace, with guidance on how to obtain it.

### Fixed
- Space-separated trace format now fully supported (in addition to tab-separated): quote-stripping on values (trunk, source IP, SIP call ID, recording format, etc.).
- Internal extension-to-extension calls correctly detected and labelled "Internal" (previously mislabelled outbound).
- Recording detection: distinguishes active MixMonitor recording from on-demand-armed-but-not-started.

---

## v1.0.0 — Initial release

### Added
- Standalone single-file HTML tool; all parsing runs client-side in the browser (no backend, works offline, no call data leaves the browser).
- Inbound / outbound / internal direction detection.
- A-leg / B-leg two-leg breakdown.
- Destination detail: NPA region (full + abbreviation), E.164 formatting, international country detection (incl. +63 Philippines).
- Timeline view (Call Events) and Full Details view.
- Call Time Breakdown donut (ring vs talk).
- Feature detection: recording, DND, Follow Me, call forwarding, transfers.
- Operation times (business hours) detection.
- Coeo Solutions branding, light/dark theme toggle, "Created by Ron Mangune" credit.
- GitHub Pages ready (index.html + README).
