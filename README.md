# Errand_Flow_Sime_Darby_Hackathon_2026
An AI driven automatically schedule and planning task with connecting township ecosystem and other systems

## 📌 Overview

Most people run errands in the order they pop into their head. Existing apps only address parts of the problem:
* **Calendar Planners:** Focus on time slots, but completely ignore physical location and travel time.
* **Route Tools:** Built for multi-stop delivery fleets, not dynamic human schedules with indoor/outdoor weather constraints.
* **Booking Tools:** Operate in silos, ignoring what else you have planned that day.

**Errand Flow** unifies time, geography, weather forecast, real-time crowd density, and precondition dependencies (like queue tickets or guardhouse access) into a single, adaptive schedule.

---

## ✨ Key Features

* 🗣️ **Bahasa Rojak NLP Input:** Native parsing of mixed Malay-English text as Malaysians naturally write (e.g., *"Kena ambik parcel kat pejabat pos, lepas tu renew passport kat Imigresen"*).
* ⏱️ **Route & Order Comparison:** Side-by-side view comparing your original intuitive task order vs. the AI-optimized order, highlighting total travel time and time saved.
* 🌦️ **Weather-Aware Scheduling:** Automatically shifts outdoor errands into dry time windows with a real-time *Schedule Impact* visualizer.
* 👥 **Crowd & Wait Optimization:** Incorporates popularity data to estimate queue times. Tasks with pre-booked virtual tickets calculate a **75% reduced wait time**.
* 🏡 **Township & Property Integration (Sime Darby Property scenario):**
  * Schedules community amenities (e.g., swimming pool) around peak crowd hours.
  * Filters and selects technician maintenance slots based on route convenience and pool downtime.
  * Pre-registers service technicians at the neighborhood guardhouse automatically.
* 🛡️ **Human-in-the-Loop Execution:** Every booking, queue ticket, or cancellation requires user confirmation with a clear before-and-after preview.
* ⚡ **Dynamic Replanning:** Re-optimizes the entire remaining day in seconds if a restaurant cancels, a storm hits, or a queue time spikes.

---

## 🏗️ System Architecture & Workflow

1. **Understand (LLM Stage):** Extracts free-text input into structured JSON containing location, estimated duration, time windows, priority, and outdoor/indoor status.
2. **Precondition Resolution:** Identifies hidden dependencies (e.g., *Passport Renewal* $\rightarrow$ requires *Queue Ticket*; *Dinner* $\rightarrow$ requires *Reservation*).
3. **Deterministic Optimization Engine:** A dedicated non-LLM solver orders tasks to maximize priority completion while minimizing travel time, queue delays, and rain exposure.
4. **Agentic Coordination:** Prepares action payloads (Cancel/Book/Queue) for user approval.
5. **Dynamic Re-Solver:** Listens for schedule disruptions (weather alerts, merchant cancellations) and recalculates route topologies instantly.

---

## 🔍 Prototype Status: Real vs. Simulated

To evaluate the system design during the prototype phase, certain live feeds are simulated using controlled deterministic data:

| Component | Prototype Status | Production Path |
| :--- | :--- | :--- |
| **Optimization Solver** | **Real** (Priorities, time windows, travel, queue, rain logic) | Google OR-Tools (for larger $N$-task sets) |
| **NLP Task Parsing** | **Simulated** (Preloaded scenarios) | Claude API / Fine-tuned LLM |
| **Replanning Engine** | **Real** (Instant recalculation upon disruption) | Event-driven microservice |
| **Weather & Crowds** | **Simulated** (Sample hourly data) | Open-Meteo API & Google Popular Times |
| **Map & Matrix Routing** | **Simulated** (Generated distance grid) | Google Maps / OneMap / OSRM API |
| **Township System** | **Simulated** (Sime Darby Property mock) | SDP App / Elmina Valley Community API |
| **Booking Execution** | **Simulated** (User preview & confirmation state) | Official APIs $\rightarrow$ Browser Automation $\rightarrow$ Human Fallback |

---

## 🎥 3-Minute Demo Flow

If you are evaluating or watching the demo, follow these steps in order:

1. *Download the HTML file:" Download the index.html and open it in browser.
2. **Parse Rojak Input:** Use Input the sample Bahasa Rojak paragraph to auto-generate structured tasks.
3. **Review Baseline:** Observe the route comparison map and calculated time savings.
4. **Simulate Disruption (Restaurant Cancellation):** Trigger a restaurant cancellation event and approve the replacement reservation.
5. **Simulate Weather Shift (Storm Alert):** Trigger a rain event; watch outdoor tasks automatically pivot to dry afternoon slots.
6. **Township Actions:** Add a community pool swim session and request a home technician slot. Notice how conflicting/peak slots are filtered out, and the guardhouse pass is automatically queued.

---

## ⚠️ Known Limits & Future Roadmap

* **Integration Dependencies:** Production readiness depends heavily on third-party access to local property management platforms (e.g., Sime Darby Property ecosystems) and government/corporate booking systems.
* **Physical Queue Volatility:** On-ground physical queue variations require real-time user delay feedback and dynamic buffer adjustments.
* **Privacy & Data Security:** User location telemetry and calendar data must be encrypted with local on-device privacy safeguards.
