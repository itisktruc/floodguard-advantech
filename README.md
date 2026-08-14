# FloodGuard AIoT

An **Age of Information (AoI)-aware AIoT framework** for real-time urban flood monitoring and water leakage detection. Built for Advantech hardware architecture (WISE-4610, EVA-2510, and WISE-6610 V2), this system dynamically balances bandwidth, battery life, and early warning responsiveness by evaluating data freshness alongside flood risk scores at the edge.

---

## Key Technical Highlights

* **Age of Information (AoI) Tracking**: Gateway tracks data latency ($\Delta t = t_{\text{current}} - t_{\text{generated}}$) to catch stale, delayed, or dropped telemetry before it affects downstream decisions.


* **Risk-Driven Adaptive Reporting**:
* **Low Risk / Stable**: Lowers telemetry uplink rates to conserve energy and bandwidth.


* **High Risk / Rapid Surge**: Triggers burst transmission for sub-second updates and local relay alerts during critical events.




* **Edge Intelligence & Fallbacks**: Runs lightweight risk scoring models (Random Forest / XGBoost) on the edge gateway. Maintains rule-based fallbacks to stay fully operational if cloud connectivity drops.


* **Closed-Loop Interactive Simulation**: Web control panel allowing operators to inject artificial rainfall spikes, pipe leaks, or network drops to validate system resilience.



---

## System Architecture

```
                    ┌─────────────────────────┐
                    │    Sensor Simulator     │ (Async Python Workers)
                    │ (Rainfall, Water Level) │
                    └────────────┬────────────┘
                                 │ MQTT (Raw JSON Payload)
                                 v
                    ┌─────────────────────────┐
                    │    Local MQTT Broker    │
                    └────────────┬────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────┐
│ WISE-6610 V2 Edge Gateway                                        │
│                                                                 │
│   ┌──────────────────┐  ┌──────────────────┐  ┌───────────────┐ │
│   │  AoI Processing  │  │  Risk Scoring /  │  │   Adaptive    │ │
│   │    Calculations  │  │  ML Inference    │  │   Reporting   │ │
│   └────────┬─────────┘  └────────┬─────────┘  └───────┬───────┘ │
└────────────┼─────────────────────┼────────────────────┼─────────┘
             └─────────────────────┼────────────────────┘
                                   │ Enriched MQTT (JSON)
                                   v
                    ┌─────────────────────────┐
                    │ WISE-IoT Cloud Platform │
                    └────────────┬────────────┘
                                 │
           ┌─────────────────────┴─────────────────────┐
           v                                           v
┌─────────────────────┐                     ┌─────────────────────┐
│ Grafana Dashboard   │                     │ Web Control Panel   │
│ (GIS, Risk Levels)  │<───────────────────>│ (Streamlit / React) │
└─────────────────────┘   Interactive Override  └─────────────────────┘

```

---

## Directory Layout

```
floodguard-aiot/
├── docker-compose.yml          # Local MQTT broker & base services
├── docs/                       # System architecture diagrams & specs
├── data/                       # Sample telemetry, raw logs & labels
├── simulator/                  # Multi-station async sensor telemetry generator
├── ai_model/                   # ML training (RF/XGBoost) & edge feature pipelines
├── edge_gateway/               # Edge runtime (AoI calculation & adaptive reporting)
├── cloud_ingestion/            # WISE-IoT platform ingestion & alert scripts
├── dashboard/                  # Grafana templates & Streamlit simulation control panel
├── evaluation/                 # Metrics & benchmark comparison scripts
└── tests/                      # Unit & integration test suites

```
