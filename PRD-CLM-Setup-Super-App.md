# PRD — CLM Setup: Super App

---

**Document Owner:** @Nathan Aderogba

**Product Name:** Rapsodo Golf CLM PRO

**Target:** Lightweight post-purchase setup flow within the Rapsodo Golf PC application. Guides a new CLM PRO owner from first app launch through room confirmation, installation verification, device connection, firmware update, and calibration — ending with the device ready to play.

**Feature Name:** CLM PRO Setup Super App

**Jira ID:**

**Prototype:** https://nathanrpso.github.io/clm-setup-super-app/ *(update when published)*

**Video Walk-Through:**

---

## Table of Contents

- Objectives
- Actors
- Requirements
- Out of Scope
- User Flows
- Success Metrics & Criteria
- Test Cases
- Assumptions & Constraints & Dependencies & Risks
- Engineering Cost
- Development Plan
- Open Questions
- Sign Off

---

## 🎯 Objectives

### Primary Objectives

Get new CLM PRO owners from first app launch to first shot as efficiently as possible, with full device binding, firmware currency, and calibration complete.

### Secondary Objectives

| | |
|---|---|
| Reduce setup failures and support contacts through clear phase-by-phase guidance and inline verification | |
| Establish account binding for ongoing session tracking and data association | |
| Provide a fast bypass path for users who have already physically installed their device | |
| Provide a reliable re-entry path for re-calibration without requiring full re-setup | |

---

## 👤 Actors

| Actor Name | Actor Behaviour |
|---|---|
| New CLM PRO Owner | First app launch after purchase. Has received the device, is at the point of physical setup. Needs guidance from room measurement through calibration. |
| Pre-Installed Owner | Has already mounted the device (e.g. professional installer used). Wants to skip room confirmation and installation steps and go straight to device connection. |
| Returning Owner (Re-Calibration) | Device already set up. Moved the unit or changed screen distance. Re-enters calibration from Settings without repeating earlier phases. |
| Guest / Explorer | Does not yet have a device or account. Wants to browse the main app. Bypasses setup entirely. |

---

## 📋 Requirements

> **Note on scope:** The Select Device screen, Device Connection flow (Ethernet instructions → scan → connect → bind), Firmware Update flow, and Activate Membership screen are already built. Requirements below cover the **Setup Hub** and the **three new phase flows** (Room Confirmation, Installation Verification, Calibration) that are net-new for this build.

---

### Already Built — Not in Scope for New Build

| Screen | Description |
|---|---|
| Auth (Login / Sign Up) | Tab-switch form with email, password, confirm password, "remember me", guest bypass |
| Account Created | Success confirmation screen after sign-up |
| Select Device | CLM PRO / MLM2 PRO device card selection |
| Ethernet Instructions | 3-step power-on and cable connection guide |
| Device Scan | Network scan result with device ID and signal strength, confirm or troubleshoot |
| Connection Success | Confirmation chip + glowing LED device illustration |
| Device Bind | Editable device name, device ID, installed height read-back |
| Firmware Info | Changelog, current vs. new version, update warnings |
| Firmware Install | Progress bar, do-not-disconnect warning |
| Firmware Done | Success confirmation |
| Activate Membership | CLM: $19.99/mo payment. MLM: 30-day free trial. Payment modal. |
| Setup Complete (Done) | Summary card: device name, ID, installed height, screen distance, firmware version, calibration date |
| Main App (Play) | Post-setup play screen — out of scope for this PRD |

---

### APP — Setup Hub

| Scope | Requirement | User Story | Priority | Notes |
|---|---|---|---|---|
| APP | Setup Hub Screen | As a new owner, I want a single dashboard showing all setup phases and my progress, so I always know where I am and what's next. | HIGH | Hub shows 4 phase cards: (1) Room Confirmation, (2) Installation + Verification, (3) Device Connection & Firmware, (4) Calibration. Each card shows phase number, title, description, and a status CTA. |
| APP | Phase Card States | System must visually differentiate card states to guide the user through the correct sequence. | HIGH | States: Default (numbered, "Start →"), Locked (greyed out, lock icon, not clickable), Done (green number replaced with ✓, "Done →"), Skipped (amber styling, "Skipped →"), Firmware Pending (amber, "Update Required"). |
| APP | Phase Unlock Logic | Phases unlock sequentially based on completion of dependencies. | HIGH | Room unlocks first. Install unlocks after Room done. Device unlocks after Room done. Calibration unlocks after Install AND Device done. |
| APP | Overall Progress Bar | As a user, I want to see how much of the total setup I've completed. | MEDIUM | Progress bar below phase cards showing X / 4 complete. Turns green at 4/4. Hides "Leave setup for now" link when all phases complete. |
| APP | All Phases Complete State | As a user, I want clear confirmation when all phases are done so I know I'm ready to play. | HIGH | Success mark + "ALL PHASES COMPLETE!" message + "VIEW SETUP SUMMARY →" CTA appears when count = 4. |
| APP | Device Status Chip | As a user, I want to see my connected device's status at the top of the hub. | MEDIUM | Chip shows device illustration (CLM bar), device name, and device ID. Shows "—" and "Please complete your device setup" until Phase 3 (Device Connection) is complete. LED dot off until connected, green when done. |
| APP | Skip / Leave Setup | As a user who wants to explore the app first, I want to exit setup without completing it. | MEDIUM | "Leave setup for now →" link at bottom of hub. Navigates to main Play screen. Setup incomplete banner shown in Play screen. |
| APP | Pre-Installed Bypass | As a user whose device is already physically mounted, I want to skip Room Confirmation and Installation steps. | HIGH | After selecting CLM PRO, user is shown "Already installed?" screen before entering hub. Option: "Yes, it's installed" → marks Room and Install phases as skipped, enters hub with Device phase unlocked. Option: "Not yet" → enters hub normally. |

---

### APP — Phase 1: Room Confirmation

| Scope | Requirement | User Story | Priority | Notes |
|---|---|---|---|---|
| APP | Room Measurements Form | As a user, I want to confirm my room dimensions meet CLM PRO requirements before I start mounting. | HIGH | Fields: ceiling height, room depth (ball flight direction), room width (side to side), ceiling material. All fields required to proceed. |
| APP | Ceiling Material Selection | As a user, I want to identify my ceiling type so the system can flag any hardware I need. | HIGH | 4 button options: Drywall (standard stud framing), Concrete / Masonry (masonry / block), Wood / Beam (exposed timber), Other (unsure or unusual). |
| APP | Unit Toggle (Imperial / Metric) | As a user, I want to enter dimensions in my preferred unit. | HIGH | Global Imperial / Metric toggle. Converts entered values on toggle. Persists per account. Units displayed per field. |
| APP | Stepper Controls | As a user, I want +/− controls to adjust measurements precisely. | MEDIUM | Stepper buttons per field. Clamps to min/max range. Ceiling height: 7.5–13 ft / 2.3–4.0 m. Room depth: 10–30 ft / 3.0–9.1 m. Room width: 8–25 ft / 2.4–7.6 m. Step: 0.5 ft / 0.1 m. |
| APP | Boundary Indicator | System must indicate when a value is at its min or max clamped boundary. | LOW | Operator symbol ("<" or ">") appears in red at left of field when at boundary. |
| APP | Live 3D Room Diagram | As a user, I want a live room illustration so I can visually verify my measurements are correct. | HIGH | Isometric 3D SVG diagram updates live as fields are filled. Shows ball flight (dashed green), screen/net (red), sensing zone (red fill). Legend shown below diagram. Perspective flip button available. |
| APP | Inline Compatibility Check | As a user, I want to see my room's compatibility result immediately after entering all fields. | HIGH | "Check My Room" button appears once all fields are filled. Triggers inline result below diagram. Same thresholds as Room Validator. |
| APP | Compatibility Result — Compatible | User's room meets all requirements. | HIGH | Green result card. Measurement summary (3-up grid). Continue button enabled. |
| APP | Compatibility Result — Conditional | User's room has caveats. | HIGH | Amber result card. Specific issue cards with guidance. User must check acknowledgment checkbox before continuing. Acknowledgment recorded to account. |
| APP | Compatibility Result — Not Compatible | User's room fails one or more hard requirements. | HIGH | Red result card. Specific blocker cards. Continue blocked. Support contact link shown. |
| APP | Compatibility Thresholds | System applies defined thresholds consistently. | HIGH | Ceiling < 8.86 ft → error. Ceiling > 10.5 ft → warning (drop mount). Depth < 13.78 ft → error. Depth < 16.4 ft → warning (tight). Width < 9.84 ft → error. Width < 13.78 ft → warning (tight). Concrete → warning (masonry kit). Other → warning (contact support). |
| APP | Saved Measurements Banner | As a user who previously used the Room Validator tool, I want my measurements pre-filled. | HIGH | If account email matches a Room Validator submission, banner appears offering "Load Saved" measurements. User can edit any pre-filled value. Dismissing banner removes it without loading. |
| APP | Installation Readiness Checklist | As a user, I want to confirm I have everything ready before starting physical installation. | HIGH | 5 checkboxes: (1) I have the tools needed (drill, bits, stud finder, level, ladder) with sub-bullets, (2) I have identified my mounting location, (3) A power outlet is within reach of the mount point, (4) My ethernet cable can reach the mount point, (5) My impact screen or net is in place. All 5 must be checked to proceed. |
| APP | Continue to Hub After Checklist | Completing the checklist marks Phase 1 as done and returns user to hub. | HIGH | "NEXT →" button enabled only when all checkboxes checked. On click: S.phases.room = true, navigate to hub. |

---

### APP — Phase 2: Installation + Verification

| Scope | Requirement | User Story | Priority | Notes |
|---|---|---|---|---|
| APP | Installation Guide Screen | As a user who hasn't yet mounted their device, I want in-app guidance to start the installation process. | HIGH | Screen displays a video placeholder (YouTube link, 8 min walkthrough). Text: "Follow the installation guide below, then continue to verify your setup." Info box: "Your CLM PRO must be fully mounted and secured before continuing." External link: "View full installation guide →". CTA: "I'VE INSTALLED MY CLM PRO →" proceeds to verification checklist. |
| APP | Installation Verification Checklist | As a user who has mounted their device, I want to self-confirm all installation items before connecting. | HIGH | 4 checkboxes: (1) Unit is mounted securely — no wobble or flex, (2) Unit is approximately level (precise calibration done later), (3) Cables are managed and not in the swing path, (4) Unit powers on — LED is lit. |
| APP | Installed Height Input | As a user, I want to record the height of my mounted device so the system can use it during calibration. | HIGH | Required number input embedded in checklist as item 5. Label: "Confirm the installed height from floor to bottom of unit". Field uses same stepper UI and unit toggle as room measurements. Range: 7–16 ft / 2.1–4.9 m. All 4 checkboxes + height value required to unlock "NEXT →". Value stored to device record and used in Calibration Phase C. |
| APP | Installed Height Unit Toggle | As a user, I want to enter installed height in my preferred unit. | MEDIUM | Click on unit label (ft/m) toggles unit. Converts current value on toggle. Independent from room measurements unit toggle. |
| APP | Power On Confirmation | As a user, I want to confirm my device is powered on before connecting. | HIGH | Question: "Is your CLM PRO powered on and showing a red LED?" Device illustration with red LED dot shown. Two buttons: "YES, IT'S ON" → completes Phase 2; "NO / NOT SURE" → shows troubleshooting. |
| APP | Power Troubleshooting | As a user whose device is not powering on, I want step-by-step help. | HIGH | Inline troubleshooting steps: (1) Check power cable at unit and adapter — ensure fully seated, (2) Try a different power outlet, (3) If LED is red — note the error code and contact support. "Still having trouble? Contact Support →" link. Back button to return to question. |
| APP | Complete Phase 2 | Confirming power on marks Phase 2 as done and returns to hub. | HIGH | "YES, IT'S ON" sets S.phases.install = true, navigates to hub. |

---

### APP — Phase 4: Calibration

| Scope | Requirement | User Story | Priority | Notes |
|---|---|---|---|---|
| APP | Calibration Phase Progress Bar | As a user, I want to track my progress through the 4 calibration sub-phases. | HIGH | 4-segment progress bar at top of each calibration screen. Active segment pulses. Completed segments filled solid red. Labels: A, B, C, D. |
| APP | Cal A — Centerline Alignment | As a user, I want to align the device's aim line to the center of my hitting position. | HIGH | Live camera feed shown (preview of hitting area with vignette and scan line). Azimuth slider: range ±15°, step 0.5°. Green acceptance zone shown on slider track: ±5°. Aim line indicator overlaid on cam feed rotates with slider. Readout grid: Pitch, Roll, Aim Line values. Aim Line tile turns green when ≤ 5°, red when > 5°. "CONFIRM ALIGNMENT →" button disabled until |azimuth| ≤ 5°. |
| APP | Cal B — Screen Distance | As a user, I want to set the distance from my hitting position to the screen. | HIGH | Slider range: 6–15 ft, step 0.5 ft. Large numeric display of selected value (e.g. "9.0 ft"). Visual distance diagram showing hitting position and screen. Distance value saved to device record. |
| APP | Cal C — Environment Check | As a user, I want the system to automatically verify my environment before test shots. | HIGH | 3 automated checks run sequentially against live device feed: (1) Lighting Adequacy, (2) Pitch & Roll Verification, (3) Height Confirmation (displays installed height from Phase 2). Each check transitions through: pending → running (spinning indicator) → pass (green ✓) / fail (red ✗). All checks must pass for "PROCEED →" button to enable. |
| APP | Cal C — Check Failure Handling | As a user who fails an environment check, I want guidance on what to fix. | HIGH | Fail state: red ✗ indicator, red value displayed. Inline guidance shown per failed check. User may retry the check or navigate back to adjust. |
| APP | Cal D — Test Shots | As a user, I want to hit test shots to confirm the calibration is working correctly. | HIGH | User prompted to hit 3 shots. Shot counter displayed: "X / 3". Each detected shot appears as a card: shot number, "Shot detected" status, green ✓ pass indicator. "SIMULATE SHOT" button used in prototype (production: awaits real shot event from device). "COMPLETE CALIBRATION ✓" button appears after 3 shots registered. |
| APP | Cal D — Retry Calibration | As a user whose shots are not registering, I want a way to restart calibration. | MEDIUM | "Not registering? Retry from Phase A" link shown after first shot attempt. Navigates back to Cal A. |
| APP | Complete Calibration | Completing 3 test shots marks Phase 4 as done and navigates to Setup Complete screen. | HIGH | S.phases.cal = true. Navigate to 'done' (Setup Complete / Summary). |
| APP | Back Navigation Within Calibration | As a user, I want to go back between calibration sub-phases. | MEDIUM | "← BACK" button on Cal B and Cal C. Cal A has "← Back to Hub" link. |

---

## ⛔ Out of Scope

| Scope | Requirement | Explanation | Status |
|---|---|---|---|
| APP | B2B / commercial venue account model | Different account structure, handled separately | |
| APP | Multi-device setup in a single session | Single device per setup flow | |
| APP | Mobile app version of this flow | PC app only for this version | |
| APP | In-app installation instruction content | Links out to CLM Setup Install Guide | |
| APP | MLM2 PRO full setup flow | Hub-MLM placeholder exists; steps TBC | WAITING |
| APP | Offline mode | Requires network connection for device scan and firmware | WAITING |
| APP | Social features, payment integrations | Main app, post-setup | |

---

## 🔁 User Flows

---

**1 — New Owner — Full Setup (CLM PRO, not pre-installed)**

Launches app
→ Logs in or creates account
→ Select Device screen — selects CLM PRO
→ Clicks "CONTINUE →"
→ "Already Installed?" screen — selects "Not yet"
→ Setup Hub (all phases locked except Room Confirmation)
→ Clicks Room Confirmation
→ Enters ceiling height, room depth, room width, ceiling material
→ Clicks "Check My Room"
→ Sees Compatible result
→ Clicks "CONTINUE →"
→ Completes Installation Readiness Checklist (5 items)
→ Returns to Hub (Phase 1 marked done)
→ Clicks Installation + Verification
→ Watches video / reads installation guide
→ Clicks "I'VE INSTALLED MY CLM PRO →"
→ Completes Installation Verification Checklist + enters installed height
→ Clicks "NEXT →"
→ Power On Confirmation — clicks "YES, IT'S ON"
→ Returns to Hub (Phase 2 marked done)
→ Clicks Device Connection & Firmware
→ [Existing: ethernet instructions → scan → confirm device → connection success → device bind → firmware info → firmware install → firmware done]
→ Returns to Hub (Phase 3 marked done)
→ Clicks Calibration
→ Cal A: adjusts azimuth slider to ≤5° → clicks "CONFIRM ALIGNMENT →"
→ Cal B: sets screen distance with slider
→ Cal C: passes all 3 automated environment checks
→ Cal D: hits 3 shots → clicks "COMPLETE CALIBRATION ✓"
→ Setup Complete / Summary screen
→ Activates membership

---

**2 — Pre-Installed Owner — Skip Phases 1 & 2**

Launches app
→ Logs in
→ Select Device — selects CLM PRO → "CONTINUE →"
→ "Already Installed?" screen — selects "Yes, it's installed"
→ Setup Hub (Room and Install phases marked as skipped, Device phase unlocked)
→ Clicks Device Connection & Firmware
→ [Existing connection flow]
→ Returns to Hub (Phase 3 done)
→ Clicks Calibration
→ Completes Cal A → B → C → D
→ Setup Complete

---

**3 — Room Not Compatible — Blocked**

User enters room measurements
→ Clicks "Check My Room"
→ Sees red Not Compatible result card
→ Reads specific blocker cards (e.g. "Ceiling too low", "Room too shallow")
→ Continue button absent / blocked
→ Sees support contact link
→ Contacts support or exits setup

---

**4 — Room Conditionally Compatible — Acknowledge and Proceed**

User enters measurements
→ Clicks "Check My Room"
→ Sees amber Conditionally Compatible result
→ Reads specific issue cards (e.g. "High ceiling — drop mount required")
→ Checks acknowledgment checkbox confirming they understand the caveats
→ Continue button enables
→ Proceeds to Installation Readiness Checklist

---

**5 — Saved Measurements Pre-fill**

User logs in with email that matches a Room Validator submission
→ Navigates to Room Confirmation (Phase 1)
→ "Load Saved" banner appears at top of measurements form
→ Clicks "Load Saved"
→ All fields pre-populated with saved values
→ Ceiling material pre-selected
→ "Check My Room" button appears
→ Clicks "Check My Room"
→ Reviews result and proceeds

---

**6 — Power On Troubleshooting**

User reaches Power On Confirmation screen
→ Device is not lit
→ Clicks "NO / NOT SURE"
→ Troubleshooting steps appear (3 steps)
→ Follows steps — resolves power issue
→ Clicks "← Back"
→ Confirms "YES, IT'S ON"
→ Phase 2 marked done, returns to hub

---

**7 — Calibration — Retry from Phase A**

User reaches Cal D — Test Shots
→ Hits shots but none register
→ Clicks "Not registering? Retry from Phase A"
→ Returned to Cal A — Centerline Alignment
→ Re-adjusts azimuth → confirms
→ Re-runs Cal B (screen distance)
→ Re-runs Cal C (environment checks)
→ Re-runs Cal D — shots register successfully
→ Clicks "COMPLETE CALIBRATION ✓"

---

**8 — Leave Setup and Return Later**

User is partway through setup
→ Clicks "Leave setup for now →" at bottom of hub
→ Navigates to main Play screen
→ Sees amber "Device setup incomplete" banner
→ Clicks "SETUP →" on banner
→ Returned to Setup Hub
→ Phase progress preserved exactly where they left off

---

## 📊 Success Metrics & Criteria

| Goal | Metric & Criteria |
|---|---|
| Setup completion rate | % of users who reach Cal D complete from Setup Hub entry |
| Drop-off by phase | % of users who exit at each phase (Hub → Phase 1, Phase 1 → Phase 2, etc.) |
| Pre-installed bypass usage | % of users who use "Yes, it's installed" bypass |
| Calibration first-attempt success | % of users who complete Cal D without retrying from Phase A |
| Support contact rate during setup | Clicks on support links within setup flow |
| Time to first shot | Minutes from account creation / login to Cal D complete |

---

## 🧪 Test Cases

| # | Description | Preconditions | Steps to Execute |
|---|---|---|---|
| 1 | Hub phase lock logic enforced | No phases completed | Open hub → verify Install, Device, Cal cards are locked; Room card is available |
| 2 | Hub updates correctly on phase completion | Room phase complete | Complete Room flow → return to hub → verify Room card shows ✓, Install card unlocks |
| 3 | Compatible room enables Continue | All measurement fields filled with valid values | Enter ch=10, rd=18, rw=14, cm=drywall → Check My Room → verify green result, Continue button enabled |
| 4 | Not Compatible room blocks Continue | Ceiling below minimum | Enter ch=7.5 → Check My Room → verify red result, Continue button absent |
| 5 | Conditional room requires acknowledgment | High ceiling entered | Enter ch=11 → check → verify amber result, Continue disabled until checkbox checked |
| 6 | Saved measurements banner appears for matching email | Account email matches Room Validator submission | Log in with matching email → navigate to Room Confirmation → verify banner displayed |
| 7 | Readiness checklist blocks NEXT until all 5 checked | Room Confirmation complete | Open checklist → verify NEXT disabled → check all 5 → verify NEXT enabled |
| 8 | Installation height persists to Cal C | Height entered in Phase 2 | Enter height 9.5 ft → complete Phase 2 → navigate to Cal C → verify "Height Confirmation (9.5 ft)" label |
| 9 | Power troubleshooting inline toggle | User clicks NO on Power Confirmation | Click "NO / NOT SURE" → verify troubleshooting block appears, question block hidden → click Back → verify question returns |
| 10 | Azimuth alignment gate enforces ≤5° | Cal A loaded | Default slider to >5° → verify Confirm button disabled → adjust to ≤5° → verify enabled |
| 11 | 3 test shots required to unlock Complete | Cal D loaded | Simulate 1 shot → verify Complete button absent → simulate 2 more → verify Complete button appears |
| 12 | Pre-installed bypass marks phases skipped | "Yes, it's installed" selected | Select bypass → navigate to hub → verify Room and Install cards show skipped state |
| 13 | | | |

---

## 🔬 Research

*(TBC)*

---

## 🔧 Assumptions & Constraints & Dependencies & Risks

| Assumptions | Constraints |
|---|---|
| Compatibility thresholds from Room Validator are identical to those used in Phase 1 | PC app only — no mobile version for this release |
| Room Validator email submissions are accessible to the PC app for pre-fill matching | Device scan requires CLM PRO and PC on same local network |
| Installed height entered in Phase 2 is passed directly into Cal C automated check | Firmware update requires stable Ethernet connection for the full duration |
| Phase completion state persists across app sessions (user can close and resume) | Calibration must complete fully before device is considered set up |

| Dependencies | Risks |
|---|---|
| Room Validator email-to-measurement lookup endpoint (Cloud) | Hardware compatibility thresholds may change before launch |
| Device network scan API | User may attempt to set up over Wi-Fi — Ethernet strongly recommended, lack of guidance increases failure rate |
| Firmware version check + install API | Cal A live camera feed requires device to be connected before calibration begins |
| Device binding API (account ↔ device) | Pre-installed bypass skips room check — may result in misconfigured installs if user is mistaken |
| CLM Setup Install Guide URL (for external link-outs) | Drop-off at Phase 1 likely to be high if measurements are not at hand |

---

## 💰 1 — Estimated High-Level Engineering Cost

| Department Name | High-Level Cost |
|---|---|
| Project Management | LOW ☐ MEDIUM ☐ HIGH ☐ |
| App / Cloud Team | LOW ☐ MEDIUM ☐ HIGH ☐ |
| Hardware / Algo Team | LOW ☐ MEDIUM ☐ HIGH ☐ |
| Web Team | LOW ☐ MEDIUM ☐ HIGH ☐ |
| QA Team | LOW ☐ MEDIUM ☐ HIGH ☐ |

---

## 📅 2 — PRD Development Plan

| PRD Name | Estimated Completion Dates | Weeks | | | | | Total Duration (Weeks) |
|---|---|---|---|---|---|---|---|
| | Baseline | Updates | W1 | W2 | W3 | W4 | |
| UI/UX Design | | | | | | | |
| Algo SW Development | | | | | | | |
| Cloud Development | | | | | | | |
| App Development | | | | | | | |
| QA Test | | | | | | | |
| UAT | | | | | | | |

---

## 💰 3 — Estimated Engineering Cost

| Team Name | Man | Months | Total Man×Months |
|---|---|---|---|
| Project Management | | | |
| UI/UX Design | | | |
| Algo SW Development | | | |
| Cloud Development | | | |
| App Development | | | |
| QA Test | | | |
| UAT | | | |

---

## ❓ Open Questions

| From | To | Question | Answer | Date Answered |
|---|---|---|---|---|
| Nathan | Cloud Team | What is the endpoint / mechanism for matching account email to Room Validator submissions for pre-fill? | | |
| Nathan | Hardware / Algo | Confirm exact azimuth tolerance for Cal A (prototype uses ≤5°) | | |
| Nathan | Hardware / Algo | Confirm installed height range for Cal C (prototype uses 7–16 ft / 2.1–4.9 m) | | |
| Nathan | Hardware / Algo | Confirm screen distance range for Cal B (prototype: 6–15 ft) | | |
| Nathan | App Team | Does phase completion state persist across app sessions server-side or locally? | | |
| Nathan | App Team | What is the re-calibration re-entry point from Settings — does this skip directly to Cal A? | | |
| Nathan | Design | Is the live camera feed in Cal A available in the PC app or is it a placeholder for a future phase? | | |
| Nathan | Product | Should the MLM2 PRO hub (hub-mlm) be a separate PRD or can it extend this one? | | |

---

## ✅ Sign Off

| Name, Surname, Date | Name, Surname, Date |
|---|---|
| | |
