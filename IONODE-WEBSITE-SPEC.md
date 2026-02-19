# IOnode Website Spec
### ionode.io — static HTML/CSS/JS site

---

## Context & References

- **This Working directory:** `ionode.io/`
- **WireClaw website reference:** `../wireclaw.io/` — study its HTML structure, CSS design system, nav, footer, card patterns. IOnode's site is a sibling — same quality, different color identity.
- **Firmware files:** `ionode.io/firmware/dist/` (esp32, esp32c3, esp32c6, esp32s3) + `ionode.io/firmware/manifest.json` — already in place
- **Flash page reference:** `../wireclaw.io/flash.html` — adapt directly
- **Web UI mock reference:** will be created later as `ionode.io/web-ui.html` — leave a placeholder nav link for now

---

## Color Identity

IOnode uses **orange** as its accent color. Apply globally via CSS variables:

```css
--accent:      #ff8c00;
--accent-dim:  rgba(255, 140, 0, 0.15);
--accent-glow: rgba(255, 140, 0, 0.25);
--border-a:    rgba(255, 140, 0, 0.25);
```

Dark background, typography, card structure — keep identical to WireClaw's design system. The only difference is the accent color.

---

## Navigation

Same nav pattern as WireClaw. Links:

```
[IOnode]  [Learn More]  [Web UI]  [OpenClaw]  [Flash]  [GitHub ↗]
```

- `IOnode` → `index.html`
- `Learn More` → `learn-more.html`
- `Web UI` → `web-ui.html` (placeholder — page doesn't exist yet, link it anyway)
- `OpenClaw` → `openclaw-integration.html`
- `Flash` → `flash.html`
- `GitHub ↗` → `https://github.com/M64GitHub/IOnode` (opens in new tab)

Active state: highlight current page in nav.

---

## Footer

Keep WireClaw footer structure. Add:

```
Part of the WireClaw ecosystem — wireclaw.io
```

Link `wireclaw.io` to `https://wireclaw.io`. Subtle — same muted text color as other footer elements, not accent.

---

## Pages

---

### index.html — Homepage

**Hero section:**

Headline:
```
Flash any ESP32. It speaks NATS.
```

Subheadline:
```
IOnode turns any ESP32 into a NATS-addressable hardware node.
Every GPIO pin, ADC channel, sensor, and actuator becomes
reachable over the network via simple request/reply.
```

Two CTA buttons:
- `Flash Now` → `flash.html` (primary, accent)
- `Learn More` → `learn-more.html` (outline)

**What it does — three-column feature cards:**

```
[ Any ESP32 ]
Flash it, name it, point it at a NATS server.
Classic, C3, C6, S3 — all supported.

[ Instant Hardware Access ]
GPIO, ADC, PWM, UART, sensors, actuators.
Request/reply over NATS. 66ms round trip.

[ Built to be Hacked ]
Add any sensor in minutes. Zero NATS code.
Your hardware, your rules.
```

**The HAL protocol — code showcase:**

Short section showing how clean the API is:

```bash
# Read a sensor
nats req ionode-01.hal.temperature ""      # → 24.5

# Toggle a relay
nats req ionode-01.hal.fan.set "1"         # → ok

# Raw GPIO
nats req ionode-01.hal.gpio.4.get ""       # → 0
```

Label it: *"Works with any NATS client. Scripts, Node-RED, Home Assistant, OpenClaw."*

**OpenClaw teaser:**

A compact section — not full integration story, just a hook:

```
Pair with OpenClaw for natural language control.

"Turn on the fan on ionode-01"
"Read temperature across the fleet"
"Set GPIO 4 high if the CI build fails"

→ OpenClaw Integration
```

Link `→ OpenClaw Integration` to `openclaw-integration.html`.

Small footnote: *"Part of the [WireClaw](https://wireclaw.io) ecosystem"* — muted, below the section.

**Chip compatibility table:**

| Chip | Status | Notes |
|------|--------|-------|
| ESP32-C6 | ✓ Default | USB-CDC, RGB LED |
| ESP32-S3 | ✓ Supported | More RAM, RGB LED |
| ESP32-C3 | ✓ Supported | Smallest/cheapest |
| ESP32 | ✓ Supported | Classic |

**Bottom CTA:**

```
Ready to flash?
[ Flash from Browser ]   [ View on GitHub ]
```

---

### learn-more.html — Deep Dive

**Hero:** `Everything IOnode can do.`

**Section 1 — The HAL Protocol**

Explain the `.hal.` namespace. Show the full subject table (same as README NATS Subject Reference section). Core HAL subjects: GPIO, ADC, PWM, UART, system info. Registered sensor/actuator routing. Discovery.

Emphasize: single wildcard subscription, reserved keywords, flat and human-readable.

**Section 2 — Device Kinds**

Show the full table of supported kinds (sensors + actuators). Brief description of each. Show a `devices.json` example.

**Section 3 — The Web UI**

Brief intro to the embedded web UI. Four tabs: Config, Devices, Pins, Status. Screenshot placeholder (or describe the UI). Link to `web-ui.html` for the full showcase.

**Section 4 — Built to be Hacked**

The hackability contract. Show the three-step code example from the README (add enum, add switch case, add string mapping). Emphasize: zero NATS code, your sensor is immediately reachable.

**Section 5 — Fleet Usage**

Discovery, addressing multiple nodes, shell script examples. The `ion.sh` wrapper.

**Section 6 — Chip Compatibility**

Full table with RAM/flash notes per chip. The RAM comparison: WireClaw ~34KB free vs IOnode ~202KB free.

---

### openclaw-integration.html — OpenClaw Integration

**Hero:**
```
Natural language control for your hardware fleet.
```

**Section 1 — What is OpenClaw**

One short paragraph: OpenClaw is an AI agent that runs on your laptop or server and orchestrates NATS-connected devices using natural language. Link to `https://github.com/openclaw/openclaw`.

**Section 2 — Live Demo**

Reproduce the chat transcript as a styled UI element (chat bubbles, dark background, monospace for NATS output):

```
OpenClaw: Holy shit, ionode-01 is ALIVE! 🎉
Two devices on the network:
• wireclaw-01 — v0.4.0, 34.6KB free heap
• ionode-01 — v0.1.0, ESP32-S3, 202KB free heap

User: read gpio 1 from ionode-01 please
OpenClaw: GPIO 1 on ionode-01 reads 0 (low). 158ms RTT. ⚡

User: and gpio 0?
OpenClaw: GPIO 0 reads 1 (high). 63ms RTT.

User: now set gpio 1 to high please!
OpenClaw: GPIO 1 is now high. ⚡
```

**Section 3 — What OpenClaw Can Do**

Four cards:
- Discover your fleet
- Read sensors & control hardware
- Write automation scripts
- Cross-domain automation (CI → LED, calendar → relay)

**Section 4 — Install the Skill**

```bash
openclaw install ionode
export IONODE_NATS_URL="nats://192.168.1.100:4222"
```

Link to IOnode skill on GitHub.

**Section 5 — WireClaw + IOnode Together**

Brief: both speak `.hal.` protocol, OpenClaw addresses them identically. One-sentence intro to WireClaw with link to `https://wireclaw.io`. The design split quote:

> *WireClaw — AI reasoning on the chip, local rules engine, self-contained*
> *IOnode — lean and fast, automation logic lives in OpenClaw*

---

### flash.html — Flash from Browser

Adapt directly from `../wireclaw.io/flash.html`.

Changes:
- Title: `Flash IOnode`
- Accent color: `#ff8c00`
- Firmware manifest path: `firmware/manifest.json`
- Chip selector: ESP32-C6 (default), ESP32-S3, ESP32-C3, ESP32
- Text: adapt for IOnode (remove WireClaw-specific mentions)
- After flash: direct user to connect to `IOnode-Setup` WiFi AP, then `192.168.4.1`

The `firmware/dist/` subdirectories and `firmware/manifest.json` are already in place — do not modify them.

---

### web-ui.html — Web UI Showcase (PLACEHOLDER)

This page does not need to be implemented now. Create a minimal placeholder:
- Same nav/footer as other pages
- Headline: `IOnode Web UI`
- Body: `Coming soon — full web UI showcase`
- Link back to `learn-more.html`

The real content will be added later once screenshots/demo assets are ready.

---

## File Structure

```
ionode.io/
├── index.html
├── learn-more.html
├── openclaw-integration.html
├── flash.html
├── web-ui.html              ← placeholder only
├── firmware/
│   ├── manifest.json        ← already in place, do not touch
│   └── dist/                ← already in place, do not touch
│       ├── esp32/
│       ├── esp32c3/
│       ├── esp32c6/
│       └── esp32s3/
```

No build tools, no frameworks. Pure HTML/CSS/JS, same as WireClaw's site.

---

## Tone & Quality

- Same quality bar as wireclaw.io — study it carefully before writing a line
- Technical, direct, no marketing fluff
- Code examples everywhere — readers are developers
- The orange accent should feel energetic and hardware-focused
- Every page should look great on mobile

---

*Spec for ionode.io — Mario Schallner / IOnode project*
