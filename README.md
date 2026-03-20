# 🛡️ GigShield
### Parametric Weather Insurance for Gig Economy Delivery Workers

> *"When the storm hits, your income shouldn't."*

**DEVTrails 2026 — Phase 1 Submission**
**Team:** _(your team name)_
**GitHub Repo:** _(your repo link)_

---

## Table of Contents

1. [Problem Statement](#problem-statement)
2. [Persona-Based Scenarios & Workflow](#persona-based-scenarios--workflow)
3. [Weekly Premium Model & Parametric Triggers](#weekly-premium-model--parametric-triggers)
4. [AI/ML Integration](#aiml-integration)
5. [Adversarial Defense & Anti-Spoofing Strategy](#adversarial-defense--anti-spoofing-strategy)
6. [Tech Stack & Development Plan](#tech-stack--development-plan)

---

## Problem Statement

India has **15+ million active gig workers** — delivery partners, cab drivers, and logistics riders — who operate entirely outdoors with zero income protection when weather makes work impossible or unsafe.

When a cyclone hits Chennai, a flood warning shuts Bengaluru's outer ring road, or a red-alert heatwave makes outdoor work a medical emergency, these workers face a brutal choice:

> **Risk their safety to earn, or go home and earn nothing.**

Traditional insurance does not serve them:

- Requires lengthy claim filing, documentation, and agent visits
- Demands proof of income loss that informal workers cannot provide
- Settles in days to weeks — useless during a 6-hour weather emergency
- Charges fixed monthly premiums that eat into already-thin earnings

**GigShield solves this with parametric micro-insurance** — automatic payouts triggered by objectively measurable weather events, with no claim forms, no agents, and no delays.

| | Traditional Insurance | GigShield |
|---|---|---|
| Claim filing | Manual, documented | None required |
| Payout time | Days to weeks | Under 3 minutes |
| Premium model | Fixed monthly | ₹15/day opt-in |
| Fraud protection | Manual review | AI multi-signal engine |
| Target user | Salaried, formal | Gig, informal |

---

## Persona-Based Scenarios & Workflow

### Persona 1 — Ravi, Swiggy Delivery Partner, Chennai

**Background:** Ravi, 28, makes ₹600–800/day on a good day. He has no savings buffer. A single lost shift due to weather means skipping a meal or defaulting on his bike EMI.

**Scenario:** Cyclone Michaung makes landfall. IMD issues a Red Alert for Chennai. Ravi was mid-route when the rain intensified and he had to stop.

**GigShield Workflow:**

```
1. Ravi opens GigShield app at shift start → opts in for ₹15 coverage
2. IMD Red Alert fires for Chennai at 14:32
3. GigShield detects Ravi's verified location inside the alert zone
4. Fraud Scoring Engine runs 5-layer verification (confidence: 91%)
5. Claim auto-triggered — no action required from Ravi
6. ₹350 hits Ravi's UPI wallet at 14:35
7. SMS: "GigShield payout of ₹350 credited. Stay safe, Ravi."
```

**Outcome:** Ravi goes home. He loses the shift but not the day.

---

### Persona 2 — Priya, Dunzo Partner, Bengaluru

**Background:** Priya, 32, is a single mother doing 6–8 deliveries a day. She can't afford to lose even one shift. She is also skeptical of apps after a bad experience with a fake insurance scheme.

**Scenario:** Flash flood warning for Koramangala. Priya is at home — her zone is affected but she hadn't started her shift yet.

**GigShield Workflow:**

```
1. Priya opted in for the week's coverage (₹90 weekly plan, Sunday night)
2. Flood warning issued for her registered home zone
3. GigShield checks: Priya's shift status = not active today
4. Result: No payout triggered — parametric requires active shift OR
           location verified inside affected zone during alert window
5. Priya receives advisory: "Red Alert in your zone. Stay safe.
   Coverage active on your next opted-in shift."
```

**Outcome:** No false payout. System correctly identifies non-active shift. Priya trusts the platform because it was honest.

---

### Persona 3 — Arjun, Zomato Partner, Mumbai (Edge Case)

**Background:** Arjun, 24, is caught in a heavy rain alert zone but has poor network connectivity. His GPS is fluctuating.

**Scenario:** Orange-to-Red escalating alert in Andheri. Arjun is genuinely stuck but his phone shows weak signal and inconsistent GPS.

**GigShield Workflow:**

```
1. Alert fires. Arjun's claim auto-triggers.
2. Fraud Scoring Engine: GPS inconsistent → Confidence score: 67% (Tier 2)
3. System flags as Soft Flag — does NOT reject
4. Worker app shows: "Verifying your location — takes 2 min. No action needed."
5. System passively re-checks: cell tower confirms Andheri sector ✓
                               barometer shows pressure drop ✓
                               trajectory shows 40 min in zone pre-alert ✓
6. Confidence rescored: 88% → Auto-approved
7. Payout ₹350 issued with original trigger timestamp
```

**Outcome:** Arjun gets paid. Bad network during a real storm never penalises a genuine worker.

---

### Application Workflow — End to End

```
┌─────────────────────────────────────────────────────────────────┐
│                        WORKER SIDE                              │
│                                                                 │
│  Open App → Opt In (₹15/day or ₹90/week) → Start Shift        │
│       ↓                                                         │
│  Background: Location + Sensor data collected continuously      │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                     PLATFORM SIDE                               │
│                                                                 │
│  Weather API polls IMD every 5 min per zone                     │
│       ↓                                                         │
│  Red Alert detected in Zone X                                   │
│       ↓                                                         │
│  Query: Active opted-in workers in Zone X                       │
│       ↓                                                         │
│  For each worker → run Fraud Scoring Engine                     │
│       ↓                                                         │
│  Score > 85%  →  Auto-Approve  →  UPI Payout  →  SMS          │
│  Score 50–85% →  Passive Re-check (2 min)     →  Re-score      │
│  Score < 50%  →  Human Review Queue           →  2hr SLA       │
└─────────────────────────────────────────────────────────────────┘
```

**Platform choice: Mobile-first PWA (Progressive Web App)**

We chose a PWA over a native app for the following reasons:
- No app store friction — delivery workers install via a single link
- Works on low-end Android devices (₹5,000 range smartphones)
- Offline-capable for areas with poor connectivity
- Single codebase serves both the worker-facing app and the admin dashboard

---

## Weekly Premium Model & Parametric Triggers

### Premium Structure

| Plan | Cost | Coverage | Best For |
|---|---|---|---|
| Daily Opt-In | ₹15/day | ₹200–500 per trigger event | Workers with irregular schedules |
| Weekly Plan | ₹90/week | ₹200–500 per trigger event | Workers with 6-day work weeks |
| Monthly Plan | ₹350/month | ₹200–500 per trigger event | Full-time delivery partners |

**How the premium is deducted:** Workers link their UPI ID at registration. Premium is auto-debited at shift start (daily) or Sunday midnight (weekly/monthly). No premium = no coverage for that shift. Workers can pause anytime.

**Payout tiers based on alert severity:**

| Alert Level | Payout | Trigger Condition |
|---|---|---|
| Orange Alert | ₹200 | IMD Orange + worker in affected zone |
| Red Alert | ₹350 | IMD Red + worker in affected zone |
| Extreme Alert / Cyclone | ₹500 | IMD Extreme + worker in affected zone |

---

### Parametric Triggers — Definition & Justification

A **parametric trigger** fires automatically when a pre-defined, objectively measurable condition is met — without requiring the worker to file a claim or prove loss.

**Our triggers are defined as:**

```
TRIGGER fires when ALL of the following are simultaneously true:

  1. IMD / OpenWeatherMap issues Orange, Red, or Extreme alert
     for the worker's current zone

  2. Worker has active coverage for the current shift
     (premium paid, shift marked active within last 2 hours)

  3. Worker's presence in the alert zone is verified
     by the Fraud Scoring Engine with confidence ≥ 85%
```

**Why parametric over traditional claims?**

Traditional insurance requires proof of loss — documentation a gig worker cannot realistically provide in the middle of a weather emergency. Parametric insurance uses an objective, third-party data source (government weather alerts) as the sole trigger. This makes it:

- **Instant** — the trigger is a data event, not a human decision
- **Objective** — no adjuster subjectivity, no disputes
- **Scalable** — 100,000 simultaneous claims processed identically
- **Fair** — every worker in the same zone under the same alert gets the same payout

**Weather data sources:**

- **Primary:** India Meteorological Department (IMD) API — official government alerts
- **Secondary:** OpenWeatherMap API — real-time weather data per coordinate
- **Tertiary:** Rainfall data from nearest weather station for cross-validation

---

## AI/ML Integration

### 1. Dynamic Premium Calculation

The base premium (₹15/day) is adjusted dynamically per worker based on a risk model:

**Input features:**
- Historical weather frequency in the worker's primary zones
- Worker's average daily earnings (determines fair payout calibration)
- Seasonal risk index (monsoon season = higher risk = premium adjustment)
- Zone-level historical claim rate

**Model:** Gradient Boosted Regression (scikit-learn) trained on 2 years of IMD weather data cross-referenced with zone-level delivery activity data.

**Output:** A per-worker daily risk score that adjusts the premium within a ±₹5 band. Workers in historically high-risk monsoon zones pay slightly more; workers in low-risk zones pay slightly less. Communicated transparently in the app.

---

### 2. Fraud Detection — Confidence Scoring Engine

Every claim is scored in real time across five independent signal layers. The final confidence score (0–100) determines the claim tier.

**Signal weights in the composite score:**

| Signal Layer | Weight | Rationale |
|---|---|---|
| Device sensor fusion (barometer, accelerometer, gyro) | 25% | Hardware-level, cannot be faked by app |
| Network triangulation (cell tower + IP + WiFi) | 25% | Network-level, requires physical presence |
| Behavioural trajectory analysis | 20% | Historical baseline, detects teleportation |
| Temporal correlation vs alert issuance | 15% | Reaction time anomaly detection |
| Fraud ring detection | 15% | Coordinated syndicate pattern recognition |

**Model:** Rule-based scoring engine for production reliability, with an Isolation Forest anomaly detector running in parallel for unknown fraud patterns. The rule-based layer ensures explainability; the ML layer catches novel attacks.

---

### 3. Fraud Ring Detection (Coordinated Syndicate Defence)

Detailed in the Adversarial Defense section below. Three parallel checks:
- **Temporal burst analysis** — claim rate statistical anomaly detection
- **Graph clustering** — NetworkX-based social graph of claimants
- **Device fingerprint deduplication** — multi-account / device farm detection

---

## Adversarial Defense & Anti-Spoofing Strategy

> ⚠️ **Threat Brief:** A coordinated syndicate of 500 delivery workers organized via Telegram used GPS-spoofing applications to fake their locations inside a red-alert weather zone while safely at home — draining the liquidity pool. Simple GPS verification is obsolete.

### The Core Problem with GPS-Only Verification

GPS coordinates are app-level data. Any free Android application can override them in under 30 seconds. A platform that trusts GPS alone is trivially exploitable. GigShield's architecture assumes GPS is **untrusted by default** and uses it only for zone classification — not for presence verification.

**Presence verification is built from signals that cannot be spoofed at the app level.**

---

### How GigShield Differentiates a Genuine Worker from a Spoofer

A real worker stranded in a storm and a spoofer sitting at home produce fundamentally different **signal signatures** across five layers:

---

#### Layer 1 — Device Sensor Fusion (The Physics Test)

GPS spoofing apps manipulate software coordinates. They cannot simultaneously fake the physical sensor readings of a device that is genuinely outdoors in a storm.

| Sensor Signal | Genuine Worker in Storm | Spoofer at Home |
|---|---|---|
| Barometric pressure | Drops — matches storm atmospheric signature | Stable — home atmospheric pressure |
| Accelerometer variance | High — natural movement, vibration | Near zero — phone stationary on desk |
| Gyroscope entropy | High — organic device handling | Near zero — no natural motion |

A genuine storm produces measurable physical effects on every sensor in the phone. A person faking their location from their living room cannot replicate these without being in the storm.

---

#### Layer 2 — Network Triangulation Cross-Check (The Location Triangle)

Three independent network-level location signals are cross-checked against the claimed GPS coordinate:

| Signal | Spoofable? | What it reveals |
|---|---|---|
| GPS coordinate | ✅ Yes (app-level) | Zone claim only — not trusted |
| Cell tower ID | ❌ No (requires root) | Physical tower the device is registered to |
| WiFi BSSID probes | ❌ No (hardware) | Nearby access points — home router vs field |
| IP geolocation | ⚠️ Partial (VPN) | Home ISP block vs mobile data in field |

**Detection logic:** If GPS claims Zone X but the cell tower maps to a residential sector 12 km away and the IP resolves to a home broadband ISP — the claim scores near zero on this layer regardless of GPS.

---

#### Layer 3 — Behavioural Trajectory Analysis (The Route Test)

GigShield builds a 30-day movement baseline per worker. On a claim event, the prior 90 minutes of location history is audited:

- **Genuine worker:** Gradual arrival in zone along a plausible delivery route
- **Spoofer:** Location teleports from home to alert zone with no intermediate trajectory

Any displacement velocity exceeding 200 km/h (impossible by road) is automatically flagged as a spoofing indicator.

---

#### Layer 4 — Temporal Correlation with Alert Issuance (The Reaction Time Test)

Real workers were already in the field when the weather deteriorated. Spoofers only know where to spoof *after* the alert is made public.

- Worker in zone **before** T_alert → strong genuine signal
- Worker appears in zone **after** T_alert → spoofing indicator
- Lag < 2 minutes after public alert → high suspicion

Legitimate workers on active shifts will have been in their delivery zones for minutes to hours before the alert fires. The reaction window is measurable and scored.

---

#### Layer 5 — Fraud Ring Detection (The Syndicate Test)

500 workers don't act identically by coincidence. Coordinated fraud produces statistical signatures impossible in genuine organic behaviour.

**5A. Temporal Burst Analysis**
- Normal event claim rate: 50–200/minute rising over 15–30 minutes
- Fraud ring signature: 300–500 claims within 2–4 minutes
- Trigger: any burst exceeding 3× the 10-minute rolling average fires an immediate ring alert

**5B. Graph Clustering Analysis**
- Device subnet clustering: multiple claimants on same home router/hotspot
- Cell tower sector density: 50+ claims from same physical cell sector (all "workers" co-located)
- GPS coordinate clustering: multiple accounts pinned to exact same lat/long

**5C. Device Fingerprint Deduplication**
- Hardware fingerprint hash detects multi-account from single device
- Catches device farms running multiple spoofed instances simultaneously

---

### The Three-Tier Response System

We do not use binary approve/reject. A genuine worker with a degraded network in a real storm must never be denied unfairly.

```
┌─────────────────────────────────────────────────────────────┐
│  TIER 1 — AUTO-APPROVE          Confidence Score > 85%      │
│                                                             │
│  All 5 layers consistent. Payout triggered instantly.       │
│  Worker SMS: "Payout of ₹X approved. Stay safe."           │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  TIER 2 — SOFT FLAG / PASSIVE RE-CHECK   Score 50–85%       │
│                                                             │
│  1–2 weak signals (plausibly caused by storm itself).       │
│  Worker sees: "Verifying location — 2 min. No action needed"│
│  System passively re-checks sensors after 3 minutes.        │
│  Stabilises → Auto-approved.  Doesn't → Tier 3.            │
│  Worker never asked to upload photos or call support.       │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  TIER 3 — HUMAN REVIEW          Score < 50% OR Ring Signal  │
│                                                             │
│  Claim HELD — not rejected. 2-hour SLA for review.         │
│  Worker: "Claim under review. You'll hear back in 2 hours." │
│  Confirmed fraud → denied + account flagged                 │
│  False positive → full payout + ₹50 goodwill credit        │
│  All outcomes feed back into model retraining.             │
└─────────────────────────────────────────────────────────────┘
```

> **Core UX Principle:** We assume innocent until proven guilty. A genuine worker in a real storm with a bad network gets 2 minutes and a passive re-check — not a rejection.

---

### Network Drop Grace Period

Genuine workers in severe weather often lose connectivity entirely. A worker with zero data connectivity cannot be verified — but also **cannot be a GPS spoofer** (spoofing apps require internet to operate and to monitor alert status).

- If a worker's device was verified inside the zone at T_alert and subsequently loses all connectivity, their last verified state is held for **up to 60 minutes**
- When connectivity resumes, system re-verifies — if still in zone, payout is issued with the original trigger timestamp
- Workers in the worst of the storm are never penalised for losing the signal a spoofer cannot lose

---

## Tech Stack & Development Plan

### Tech Stack

| Layer | Technology | Justification |
|---|---|---|
| Frontend | Next.js + Tailwind CSS | Fast development, SSR for low-end devices |
| Mobile | PWA (React) | No app store friction, works on ₹5k Android phones |
| Backend | Node.js + Express | Team familiarity, non-blocking I/O for concurrent claims |
| Database | Supabase (PostgreSQL) | Managed, real-time subscriptions, built-in auth |
| Cache | Redis | Fraud score caching, claim burst rate tracking |
| ML / Fraud Engine | Python + scikit-learn + NetworkX | Isolation Forest + graph clustering |
| Maps | Leaflet.js | Open source, no API cost, excellent for zone overlays |
| Weather | IMD API + OpenWeatherMap | Primary + fallback, free tier sufficient for Phase 1 |
| Payments | UPI Autopay (Razorpay) | Native Indian payment rail, instant transfers |
| Auth | Supabase Auth + OTP | Phone number OTP — no email required for workers |
| Hosting | Vercel (frontend) + Railway (backend) | Free tier, instant deployment |

---

### Development Plan

#### Phase 1 — Ideation & Foundation *(Current — March 20)*

- [x] Problem definition and persona research
- [x] Core architecture design
- [x] Fraud detection strategy (this document)
- [ ] Repository setup and README
- [ ] Minimal prototype: Worker dashboard (weather status + claim status screen)
- [ ] 2-minute pitch video

#### Phase 2 — Core Build *(March 21 – April)*

- [ ] Worker onboarding flow (phone OTP + UPI linking)
- [ ] Premium opt-in and deduction via UPI Autopay
- [ ] Weather alert ingestion service (IMD API poller)
- [ ] Fraud Scoring Engine v1 (rule-based, 5 layers)
- [ ] Auto-claim trigger pipeline
- [ ] Admin dashboard (live claim map + fraud score view)

#### Phase 3 — Intelligence & Polish *(April – Final)*

- [ ] ML premium calculator (Gradient Boost model)
- [ ] Isolation Forest anomaly detector (Layer 5 enhancement)
- [ ] NetworkX fraud ring graph (syndicate detection)
- [ ] End-to-end testing with simulated fraud scenarios
- [ ] Performance optimisation for concurrent claim bursts
- [ ] Final demo and submission

---

### Repository Structure

```
gigshield/
├── apps/
│   ├── web/              # Next.js worker PWA
│   └── admin/            # Admin dashboard
├── services/
│   ├── api/              # Node.js + Express backend
│   ├── fraud-engine/     # Python fraud scoring service
│   └── weather-poller/   # IMD + OpenWeatherMap ingestion
├── ml/
│   ├── premium-model/    # Gradient Boost premium calculator
│   └── anomaly/          # Isolation Forest ring detector
├── infra/
│   └── supabase/         # DB schema + migrations
└── README.md
```

---

*GigShield — DEVTrails 2026 — Phase 1 Submission*
*Protecting the workers who keep the economy moving, one shift at a time.*
