# CLM-Setup-AppFlow: Requirements

## Overview

The post-purchase setup experience within the CLM PRO PC application. Guides a new owner from first app launch through room confirmation, installation verification, device connection, and calibration — ending with the unit ready to play. This is the primary digital product experience for new CLM PRO owners.

This flow is auth-gated and assumes the user has downloaded the PC app after purchase. It references the public installation guide (CLM-Setup-InstallGuide) for physical setup detail rather than reproducing it, and hands off to the main app experience upon completion.

Re-calibration is a re-entrant path into Phase 4 accessible from app Settings at any time post-setup.

---

## Goals

- Get new owners from first launch to first shot as efficiently as possible
- Reduce setup failure and support contact through clear guidance and verification at each stage
- Establish the user's account and bind their device to it for ongoing session tracking and data access
- Provide a reliable re-entry path for re-calibration without requiring full re-setup

---

## Users

**Primary:** Home CLM PRO owner, first launch after purchase and physical delivery.
**Secondary:** Owner who has already mounted the unit and is completing software setup days later.
**Tertiary:** Existing owner re-entering for re-calibration after moving the unit or changing screen distance.
**Out of scope (this version):** B2B / commercial venue operators — different account model, handled separately.

---

## Access & Auth

- Requires account creation or login to proceed past Phase 1
- Account creation is the first step of the flow, not mid-flow
- Auth persists across sessions — user can close the app and resume where they left off
- Progress tracked per user account and per device binding
- Re-calibration re-entry point available in Settings for authenticated users with a bound device

---

## Flow Structure

The setup flow has four phases. Each phase is a distinct section with its own progress indicator. Users can see all phases from a top-level overview screen on first launch.

```
App Launch
└── Overview Screen (what to expect)
    ├── Phase 1: Account Setup
    ├── Phase 2: Room Confirmation
    ├── Phase 3: Installation Verification
    └── Phase 4: Connect & Calibrate
        └── [Re-entry point from Settings]
```

---

## Phase 1: Account Setup

**Goal:** Create or log into a Rapsodo account and activate the product.

### Screen 1.1 — Welcome
- CLM PRO branding, brief welcome message
- Two options: "Create Account" / "I already have an account"
- Note: existing Rapsodo account holders (e.g., from mobile products) can use existing credentials

### Screen 1.2 — Create Account
**Fields:**
- Full name
- Email address
- Password (with confirmation)
- "Receive product updates and tips" checkbox (opt-in, unchecked by default)

**Behavior:**
- Email must be unique — inline validation against existing accounts
- Password: minimum 8 characters, at least one number
- On success: account created, session established, 30-day trial activated automatically
- Trial activation modal shown immediately after account creation (see Modal: Trial Activation)

### Screen 1.2b — Log In (existing account)
**Fields:**
- Email
- Password
- "Forgot password" link (opens browser to account recovery page)

**Behavior:**
- On success: session established, proceed to Phase 2
- If account has already completed setup with a different device: show notice, allow proceeding

### Modal: Trial Activation
- Triggered immediately after new account creation
- Communicates: 30-day free trial of full feature set
- Trial end date displayed
- Payment method collection (optional at this stage — can add later)
- CTA: "Start My Trial" — dismisses modal and proceeds to Phase 2
- This is the only place subscription/trial is surfaced in the setup flow

---

## Phase 2: Room Confirmation

**Goal:** Confirm the installation space is ready before the user touches the hardware.

**Note:** This phase is lightweight — it is a verification step, not an instruction step. Detailed measurement guidance lives in CLM-Setup-InstallGuide.

### Screen 2.1 — Room Measurements

**Inputs:**

| Field | Type | Unit Toggle | Notes |
|---|---|---|---|
| Ceiling height | Number | ft/in or m/cm | At the planned mount point |
| Room depth | Number | ft/in or m/cm | Direction of ball flight |
| Room width | Number | ft/in or m/cm | Side to side |
| Screen size (diagonal) | Number | inches | Impact screen or net |
| Ceiling material | Select | — | Drywall, Drop ceiling, Concrete/Masonry, Wood/Beam, Other |

**Pre-fill behavior:**
- If user completed CLM-Setup-RoomValidator before purchase and an email match is found, offer to pre-fill measurements with a "Use my saved measurements" prompt
- User can edit any pre-filled value

**Behavior:**
- All fields required
- Unit toggle persists per account
- Inline validation against compatibility thresholds (same thresholds as CLM-Setup-RoomValidator)
- On submit: show compatibility result inline (compatible / conditionally compatible / not compatible)

### Screen 2.2 — Compatibility Result

**States:**

| State | Display |
|---|---|
| Compatible | Green confirmation, summary of entered measurements, proceed CTA |
| Conditionally Compatible | Amber, specific issues listed with guidance, proceed with acknowledgment required |
| Not Compatible | Red, specific blockers, link to support, option to re-enter measurements |

**For Conditionally Compatible:** User must check a box acknowledging the caveat before proceeding. This is recorded against their account.

### Screen 2.3 — Installation Readiness Checklist
A brief checklist the user self-confirms before moving to physical installation:

- [ ] I have all tools ready (drill, bits appropriate for my ceiling type, stud finder, level, ladder)
- [ ] I have identified my mount location
- [ ] I have a power outlet within reach of the mount point
- [ ] My ethernet cable is routed (or I plan to use Wi-Fi)
- [ ] My screen/enclosure is in place

**Link to full installation guide:** "Need help with any of these? View the full installation guide" — opens CLM-Setup-InstallGuide in browser.

---

## Phase 3: Installation Verification

**Goal:** Confirm the CLM PRO unit is physically mounted and ready for software setup.

**Important:** This phase does not teach installation. It verifies it. All instruction detail is in CLM-Setup-InstallGuide.

### Screen 3.1 — Installation Status
- Question: "Have you already mounted your CLM PRO unit?"
- Two paths:
  - "Yes, it's mounted" → proceed to verification checklist
  - "Not yet" → show interstitial with link to CLM-Setup-InstallGuide, hold on this screen

### Screen 3.2 — Installation Verification Checklist
User self-confirms each item:

- [ ] Unit is mounted securely — no wobble or flex
- [ ] Unit is approximately level (within a few degrees — precise calibration done in Phase 4)
- [ ] Cables are managed and not in the swing path
- [ ] Unit is powered on (LED is lit)
- [ ] I've noted the installed height from floor to bottom of unit: ____ ft/in

**Installed height input:** Required field. This value is stored against the device record and used during calibration.

**LED status reference:** Collapsed "What do the LEDs mean?" section expanding to show LED state guide (same content as CLM-Setup-InstallGuide Step 5.3).

**Link:** "View installation instructions" — opens CLM-Setup-InstallGuide in browser at Step 5.

All items must be checked and height entered before proceeding.

### Screen 3.3 — Power On Confirmation
- Prompt: "Is your CLM PRO powered on and showing a white LED?"
- Yes → proceed to Phase 4
- No → troubleshooting inline:
  - Check power cable connection at unit and adapter
  - Try a different power outlet
  - If LED is red: link to support with error code guidance
  - "Still having trouble?" → opens support contact

---

## Phase 4: Connect & Calibrate

**Goal:** Connect the device to the network, bind it to the user's account, update firmware, calibrate, and validate with test shots.

This phase is also the re-entry point for re-calibration from Settings (see Re-Calibration Re-Entry below).

### Screen 4.1 — Network Connection

**Sub-flow:**

1. App scans the local network for CLM PRO devices
2. Shows list of discovered devices with signal strength and device ID
3. User selects their device
4. If no device found after scan: troubleshooting tips shown
   - Confirm device is powered on (white LED)
   - Confirm device and PC are on the same network
   - Try ethernet connection instead of Wi-Fi
   - Rescan option
   - Manual IP entry option (advanced)

**Connection modal:**
- Shown when user selects a device from list
- Displays: device name, device ID, firmware version, signal strength
- CTA: "Connect to this device"
- On success: device shown as connected, proceed to 4.2

**Wi-Fi setup path (if no ethernet):**
- If device is not found and user indicates they're using Wi-Fi: guided Wi-Fi credentials entry
- App sends credentials to device via temporary direct connection
- Returns to scan after credentials set

### Screen 4.2 — Device Binding
- Binds the connected device to the user's account
- Displays: device name (editable), device ID, installed height (from Phase 3)
- User confirms or edits device name
- On bind: device record created in account, associated with user
- If device was previously bound to another account: show warning, require explicit confirmation to transfer

### Screen 4.3 — Firmware Update
- App checks for available firmware update
- **If up to date:** Brief confirmation, auto-proceed after 2 seconds
- **If update available:**
  - Show current version and new version
  - Estimated update time
  - Warning: do not power off during update
  - CTA: "Install Update"
  - Progress bar during installation
  - Auto-restart notification
  - On completion: re-connect automatically, proceed to 4.4

### Screen 4.4 — Calibration

Calibration is a multi-phase process. Each phase has its own screen and completion state.

**Calibration Phase A — Centerline Alignment**
- Instruction: position a golf club or alignment rod at the center of your hitting position on the floor
- Live device feed shown (camera preview of the hitting area)
- Azimuth adjustment: slider or directional controls to align the device's horizontal aim
- Live readout: pitch, roll, azimuth values
- Target indicators showing acceptable ranges
- Confirm when azimuth is within tolerance

**Calibration Phase B — Screen Distance**
- Input: distance from hitting position to screen (slider, 6–15 ft range, or manual entry)
- Visual diagram showing the measurement
- Value stored against device record

**Calibration Phase C — Environment Check**
- Automated checks run against the live device feed:
  - Lighting adequacy
  - Background clutter detection
  - Pitch and roll final verification
  - Height confirmation against entered value
- Each check shows pass/fail with inline guidance for failures
- All checks must pass before proceeding (or user acknowledges a warning for conditionally acceptable states)

**Calibration Phase D — Test Shots**
- Prompt: hit 2–3 shots to validate calibration
- Live shot data displayed per shot:
  - Club speed
  - Ball speed
  - Launch angle
  - Carry distance
  - Spin (if available)
- After each shot: pass/fail indicator against expected data ranges
- If shots register correctly: calibration confirmed
- If shots fail or don't register:
  - Guided troubleshooting (alignment, lighting, hitting position)
  - Option to re-run calibration from Phase A
  - Option to contact support

### Screen 4.5 — Setup Complete
- Confirmation: device connected, bound, firmware current, calibrated
- Summary card: device name, installed height, screen distance, calibration date
- Primary CTA: "Start Playing" — launches into main app experience
- Secondary CTA: "Go to Settings" — navigates to app settings

---

## Re-Calibration Re-Entry

Accessible from: **App Settings → Device → Re-Calibrate**

**Conditions for availability:**
- User is authenticated
- At least one bound device exists on the account

**Re-entry flow:**
- Skip Phases 1, 2, 3 entirely
- Optional: show prompt "Has anything changed since last calibration?" with options:
  - Screen distance changed
  - Unit was moved or re-mounted
  - Routine re-calibration
- Proceed directly to Calibration Phase A
- On completion: update calibration date on device record
- Return to Settings after completion (not to main app, to avoid confusion)

---

## Navigation & Progress

- Top-level progress indicator visible throughout: Phase 1 of 4, Phase 2 of 4, etc.
- Within each phase: step indicator (Step 2 of 3)
- Back navigation available within a phase; navigating back across phases requires explicit confirmation
- User can close the app mid-flow and resume from the last completed screen on next launch
- Sidebar or step list not shown during setup — linear focus only; Settings access is available but discouraged during initial setup via visual de-emphasis

---

## Data Model

**User record:**
- id, email, fullName, passwordHash
- receiveUpdates (boolean)
- trialActive (boolean), trialStartDate, trialEndDate
- paymentAdded (boolean)
- setupPhaseCompleted (1–4, tracks furthest phase completed)

**Device record:**
- id, userId (FK)
- deviceName (user-editable)
- deviceId (hardware identifier)
- firmwareVersion
- connected (boolean)
- calibrated (boolean)
- installedHeight (float, ft)
- screenDistance (float, ft)
- ceilingMaterial
- calibrationDate (timestamp)
- roomMeasurements (JSON: ceilingHeight, roomDepth, roomWidth, screenSize)

---

## API Endpoints

| Method | Path | Description |
|---|---|---|
| POST | /api/register | Create account |
| POST | /api/login | Authenticate |
| POST | /api/logout | End session |
| GET | /api/user | Get current user |
| PUT | /api/user/setup-phase | Update furthest completed phase |
| PUT | /api/user/subscription | Update trial/payment status |
| GET | /api/devices | Get user's bound devices |
| POST | /api/devices | Bind a new device |
| PUT | /api/devices/:id | Update device record (name, height, calibration state, etc.) |
| GET | /api/devices/:id/firmware | Check firmware version and update availability |
| POST | /api/devices/:id/firmware/install | Trigger firmware update |
| GET | /api/devices/scan | Initiate local network device scan |

---

## Edge Cases

| Scenario | Handling |
|---|---|
| User closes app mid-setup | Resume from last completed screen on next launch |
| User's measurements are not compatible | Block progression with guidance; support contact offered |
| Device not found on network scan | Inline troubleshooting, rescan, manual IP entry |
| Device already bound to another account | Warning shown, explicit transfer confirmation required |
| Firmware update fails mid-install | Error state with retry option, do-not-power-off warning held until resolved |
| Calibration test shots don't register | Troubleshooting steps shown, option to retry from Phase A or contact support |
| User has pre-existing Rapsodo account | Login path, skip account creation, check for existing device bindings |
| User had measurements from CLM-Setup-RoomValidator | Offer pre-fill if email matches |

---

## Out of Scope

- B2B / commercial venue account model
- Multi-device setup within a single session
- Mobile app version of this flow (PC app only for this version)
- In-app installation instruction content (link out to CLM-Setup-InstallGuide)
- Game loading, shot history, training programs (main app, post-setup)

---

## Success Metrics

- Setup completion rate (Phase 1 start → Phase 4 complete)
- Drop-off rate per phase
- Time to complete full setup
- Re-calibration re-entry rate (how often users return to calibrate)
- Support contact rate during setup flow
- Calibration success rate on first attempt
