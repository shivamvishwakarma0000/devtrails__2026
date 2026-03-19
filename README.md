# RoRak — Income Protection for Q-Commerce Delivery Riders

> **RoRak** is short for *RoziRaksha*   
> *"Teri rozi ki Raksha, hamesha."*
> 
> *Under Parametric Insurtech*

---

## Team

| Name | Role |
|------|------|
| Mansi Jangid    | Strategy, Research, Product Design |
| Shivam Vishwakarma   | Development                        |
| [Name 3]   | UI/UX Design                       |

**Solution:** RoziRaksha  
**Repository:** github.com/Mansi29j/devtrails_2026  
**Persona:** Q-Commerce Delivery Riders — Blinkit / Zepto   

---

## 1. The Problem

Q-Commerce riders (Blinkit, Zepto) earn entirely based on orders completed.
When external disruptions hit their zone — heavy rain, dangerous air quality,
waterlogging, or civic shutdowns — deliverable orders drop sharply or stop
completely. Riders who are active and available to work earn nothing during
these periods, through no fault of their own.

**There is currently no income protection for this segment.**
When a disruption occurs, the rider absorbs the full financial loss.

### Key Numbers

| Metric | Figure | Source |
|--------|--------|--------|
| Net daily earnings after fuel and costs | ₹500 – ₹1,000 | bitveen.com |
| Fuel cost as % of gross earnings | 20 – 40% (self-funded) | alphareach.tech |
| Weekly net take-home (metro, 6 days) | ₹3,000 – ₹6,000 | Calculated |
| Rider working radius from dark store | ~2 km | Platform operations |

> Riders cover all operational costs themselves — fuel, phone data, maintenance.
> Net income, not gross, is the correct baseline for any income protection model.

---

## 2. The Solution — RoRak

RoRak is a **parametric income insurance platform** for Q-Commerce delivery riders.

**What this means in simple terms:**
A pre-agreed external event (heavy rain, dangerous AQI) is detected automatically
by a weather or environment API. If the rider was active and in the affected zone,
their estimated lost income is transferred to their UPI — no claim form, no call,
no paperwork required from the rider.

**Coverage:** Income lost during verified external disruption events only.  
**Excluded:** Health, life, accidents, vehicle damage, vehicle repair — strictly.

---

## 3. Disruption Triggers

Five external data sources are monitored. No rider input triggers a payout.

| # | Trigger | Threshold | Data Source |
|---|---------|-----------|-------------|
| 1 | Heavy Rainfall | > 15mm/hour in rider's pincode | OpenWeatherMap (free tier) |
| 2 | Extreme Heat | > 42°C in rider's zone | OpenWeatherMap (free tier) |
| 3 | Dangerous Air Quality | AQI > 300 | OpenAQ (free, open source) |
| 4 | Waterlogging Alert | BMC flood alert in active pincode | BMC Open Data (public) |
| 5 | Civic Disruption | Verified curfew or shutdown | Simulated / mock API |

---

## 4. Weekly Premium Model

Premium is **weekly** — aligned with how Q-Commerce riders are paid
(Monday–Sunday cycle, credited Tuesday or Wednesday via UPI).

### How the premium is calculated

```
Base premium = small fixed % of rider's estimated weekly net income
Adjusted by  = zone risk level (high flood history zone pays slightly more)
```

| Rider Type | Est. Weekly Net Income | Weekly Premium | Premium % of Net |
|------------|----------------------|----------------|-----------------|
| Metro — high risk zone | ₹4,500 – ₹6,000 | ₹38 – ₹50 | ~0.8 – 1.0% |
| Metro — standard zone | ₹4,500 – ₹6,000 | ₹34 – ₹42 | ~0.7 – 0.9% |
| Tier-2 city | ₹2,500 – ₹4,000 | ₹22 – ₹28 | ~0.8 – 0.9% |

### How the payout is calculated

Payout is based on **net income lost** — not gross.

```
Example:
Net hourly rate  = (₹1,000 gross − ₹350 fuel − ₹50 maintenance) ÷ 10 hrs
                 = ₹60 per hour net

3-hour disruption payout = ₹60 × 3 = ₹180
```

---

## 5. AI and ML — What We Plan to Use

| What | How |
|------|-----|
| Zone risk scoring | Use historical IMD rainfall data by pincode to assign a simple risk level (low / medium / high) per zone — used to adjust weekly premium |
| Live trigger monitoring | OpenWeatherMap API checked at intervals — when threshold crossed, payout sequence begins |
| Fraud prevention (basic) | Verify rider was active on platform + GPS location matches triggered zone before any payout |

The AI integration will grow progressively across Phase 2 and Phase 3.

---

## 6. Tech Stack

| Layer | Tool | Why |
|-------|------|-----|
| Frontend | React.js (mobile-first) | Works on any phone browser |
| Backend | Node.js + Express | Lightweight API handling |
| Database | Firebase (free tier) | Real-time, no server setup needed |
| Weather | OpenWeatherMap API | Free tier, pincode-level data |
| Air Quality | OpenAQ API | Free, open source |
| Payments | Razorpay test mode | UPI simulation, India-native |
| Hosting | Vercel + Railway | Both free tiers |

---

## 7. Workflows
 
### 7.1 Rider Journey — What the Rider Experiences
 
```
STEP 1 — Sign Up (2 minutes)
Rider opens RoRak on phone browser
→ Enters name, UPI ID, active pincode, delivery platform (Blinkit / Zepto)
→ System assigns zone risk level based on pincode
→ Weekly premium displayed (e.g. ₹42/week for Andheri, Mumbai)
→ Rider activates coverage — ₹42 debited from UPI
 
        ↓
 
STEP 2 — Active Coverage Week
Rider works normally on Blinkit / Zepto
RoRak monitors their zone silently in the background
Rider does nothing — no app to check, no buttons to press
 
        ↓
 
STEP 3 — Disruption Occurs (e.g. heavy rain in Andheri)
RoRak detects rain crossing 15mm/hr threshold via OpenWeatherMap
System checks — was this rider active during this window?
System checks — was this rider in the affected pincode?
Both confirmed → payout calculated → UPI transfer initiated
 
        ↓
 
STEP 4 — Rider Receives Payout
UPI notification arrives on rider's phone:
"RoRak credited ₹180 for 3hrs rain disruption — Andheri West."
Rider did not file a claim.
Rider did not call anyone.
Rider did not fill any form.
The money just arrived.
 
        ↓
 
STEP 5 — End of Week
Coverage auto-renews next Monday
New zone risk score calculated
New weekly premium shown for rider to approve
```
 
---
 
### 7.2 System Workflow — What Happens Behind the Scenes
 
```
TRIGGER DETECTION
OpenWeatherMap API polled every 15 minutes per active rider zone
Condition crosses defined threshold (e.g. rain > 15mm/hr)
                        ↓
GATE 1 — External Verification
API response confirmed and logged with timestamp
Disruption window start time recorded
                        ↓
GATE 2 — Activity Check
Active platform session — online and available on Blinkit/Zepto — mid-delivery OR waiting for orders—
Was this rider's app session active  during the disruption window?
YES → proceed   |   NO → no payout, log reason
                        ↓
GATE 3 — Location Check
Rider's last known GPS coordinates checked —
Does location fall within the triggered pincode zone?
YES → proceed   |   NO → no payout, log reason
                        ↓
PAYOUT CALCULATION
Net hourly rate calculated from rider's onboarding profile
Multiplied by verified disruption duration
Payout amount confirmed against weekly policy terms
                        ↓
PAYMENT
UPI transfer initiated via Razorpay (test mode in Phase 2)
Rider receives credit notification on phone
Full audit record written — trigger data, gate results, amount, timestamp
                        ↓
DASHBOARD UPDATE
Rider dashboard updated — payout shown under "This Week"
Admin dashboard updated — event logged under zone activity
```
 
---


## 8. Roadmap

| Phase | Period | Goal |
|-------|--------|------|
| Seed (Phase 1) | Mar 4 – Mar 20 | Problem research, solution design, README, 2-min video |
| Scale (Phase 2) | Mar 21 – Apr 4 | Working prototype — onboarding, premium calculation, trigger demo |
| Soar (Phase 3) | Apr 5 – Apr 17 | Payout simulation, dashboards, fraud checks, final pitch |

---

## 9. Sources

| Source | Used For |
|--------|----------|
| alphareach.tech | Rider pay structure — base pay, surge, incentives |
| bitveen.com | Net earnings after expenses |
| NDTV rider interviews | Real daily earnings range |
| BMC Open Data Portal | Mumbai ward flood zone data |
| IMD Open Data | Historical pincode-level rainfall |
| OpenAQ | Historical AQI by city zone |

---

*RoRak by RoziRaksha · Guidewire DEVTrails 2026*
