# CLM Setup Hub - Super App Flow

Interactive prototype for the Rapsodo CLM PRO PC app onboarding flow - the guided experience that takes a user from an unboxed, ceiling-mounted CLM PRO to playing.

## Overview

A **Setup Hub** anchors the experience and tracks progress across four phases:

1. **Room Confirmation** - plan the room and confirm the CLM PRO will fit. Embeds the Golf Sim Planner as a webview, then an acknowledgement to continue.
2. **Installation + Verification** - mount the unit, then verify it is secure, level, powered on, and at the right height.
3. **Device Connection & Firmware** - connect over Ethernet, bind to the account, and update firmware.
4. **Calibration** - overhead wand-based calibration: alignment, screen distance, environment check, and test shots.

The flow opens with account setup and device selection, and ends with a setup-complete summary and the play view.

## What's in this build

New in this prototype: the **Setup Hub** plus the **Room Confirmation**, **Installation + Verification**, and **Calibration** steps. The account / device-selection start, the Device Connection & Firmware phase, and the post-calibration screens are already built in Super App.

## Design

Black background, Rapsodo red (#C42028), Barlow Condensed headings, grass at the foot of every screen. Content scales to fit the viewport so it never scrolls, with component sizes kept consistent across screens.

## Run

Single-file HTML/CSS/JS, no build step. Open `index.html` in a browser, or serve locally:

```bash
npx serve .
```

## Links

- Live: https://clm-setup-hub-super-app-0c29ca.gitlab.io/
- Repo: https://gitlab.com/rapsodoinc/rapsodous/golf/nathan/clm-setup-hub-super-app
- Golf Sim Planner (embedded in Room Confirmation): live https://sim-planner-2485d9.gitlab.io/ , repo https://gitlab.com/rapsodoinc/rapsodous/golf/nathan/sim-planner
