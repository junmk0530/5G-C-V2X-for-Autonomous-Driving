# 5G C-V2X for Autonomous Driving
### Technology Survey and Case Study Analysis

> A case study analysis of 5G C-V2X deployment and performance verification across EU cross-border trials.  
> **Korea University Sejong Campus** · DCCS307 Computer Networks · Academic Presentation 2026

---

## 📽️ Presentation Video

[![5G C-V2X Presentation](https://img.youtube.com/vi/gk2YCNwvN5Q/maxresdefault.jpg)](https://www.youtube.com/watch?v=gk2YCNwvN5Q)

▶️ **[Watch on YouTube](https://www.youtube.com/watch?v=gk2YCNwvN5Q)**

---

## 👥 Team

| Name | Role | Contribution |
|---|---|---|
| **Sungjune Kong** | Leader | 3GPP standard evolution (Rel-14 → Rel-16), C-V2X architecture (PC5 / Uu), PPT presentation |
| **Junmok Lee** | PPT Design | 5G-MOBIX trial cases (GR-TR / ES-PT), 5G-PPP verification results, slide design |
| **Lana Kim** | Presentation & Q&A | 5G-DRIVE D4.4 field trial results, IEEE/Elsevier performance papers, Q&A preparation |

---

## 📋 Table of Contents

1. [Problem Definition](#1-problem-definition)
2. [Research Scope & Methodology](#2-research-scope--methodology)
3. [Standards & Architecture](#3-standards--architecture)
4. [Technical Enablers](#4-technical-enablers)
5. [Field Trials & Verification](#5-field-trials--verification)
6. [KPI Compliance Summary](#6-kpi-compliance-summary)
7. [Key Trade-offs & Limitations](#7-key-trade-offs--limitations)
8. [Q&A Discussion](#8-qa-discussion)
9. [Conclusion & Future Directions](#9-conclusion--future-directions)
10. [References](#10-references)

---

## 1. Problem Definition

Autonomous driving at SAE Level 4/5 demands communication performance that existing networks fundamentally cannot deliver.

| Technology | Avg. Latency | Reliability | Coverage | QoS Guarantee |
|---|---|---|---|---|
| **DSRC (802.11p)** | ~30ms | No guarantee | ~300m | ✗ |
| **LTE V2X** | ~50ms | No guarantee | Cellular | ✗ |
| **5G NR V2X** | **< 5ms** | **> 99.999%** | Cellular + Sidelink | ✅ |

> **Core problem:** Neither DSRC nor LTE can guarantee deterministic, safety-critical QoS for advanced autonomous driving use cases such as remote driving, platooning, or cooperative collision avoidance.

---

## 2. Research Scope & Methodology

This project investigates 5G C-V2X through three lenses:

```
Concept → Industry Adoption → Experimental Verification
```

**Communication types analyzed:**
- **V2V** — Vehicle-to-Vehicle (PC5 sidelink, direct)
- **V2I** — Vehicle-to-Infrastructure (RSU)
- **V2N** — Vehicle-to-Network (Uu, via gNB / MEC)
- **V2P** — Vehicle-to-Pedestrian (safety proximity)

**EU Projects analyzed:** 5G-DRIVE · 5GCroCo · 5G-MOBIX · 5GCAR

---

## 3. Standards & Architecture

### 3GPP Release Evolution: Rel-14 → Rel-16

```
Rel-14 (2017) ──────────────────────────────────────────────────────────▶
  LTE-V2X introduced. PC5 sidelink for basic V2V safety messaging.
  → Basic: Collision warning, Emergency braking

Rel-15 (2018) ──────────────────────────────────────────────────────────▶
  5G NR foundation. eV2X defined. 64QAM, latency target 10ms, carrier
  aggregation up to 8 component carriers.
  → Advanced: Platooning, Extended sensors (design phase)

Rel-16 (2020) ──────────────────────────────────────────────────────────▶
  NR V2X complete. Enhanced PC5 sidelink (unicast + groupcast).
  URLLC applied. Uu integration. All 4 advanced use cases enabled.
  → Full: Remote driving, Cooperative automated driving
```

📄 *Source: [6] Xu et al., "Advancing C-V2X for Level 5 AD," Sensors 2023*

### PC5 vs Uu Interface

| Interface | Mode | Best For | Infrastructure Required |
|---|---|---|---|
| **PC5** | Direct sidelink (V2V, V2I) | Collision avoidance, Platooning | None |
| **Uu** | Cellular uplink via gNB | Remote driving, MEC, Slicing | gNB + Core |
| **NR V2X Dual** | Both simultaneously | Safety-critical redundancy | Both |

> **Key insight:** NR V2X (Rel-16) supports both interfaces simultaneously, providing communication redundancy essential for safety-critical services.

---

## 4. Technical Enablers

### 4.1 MEC-Based Latency Reduction

```
Traditional Cloud Path:
  Vehicle → gNB → Core Network → Cloud Server → Core → gNB → Vehicle
  Round-trip: ~50ms

MEC Path:
  Vehicle → gNB → MEC (at base station) → gNB → Vehicle
  Round-trip: < 5ms
```

Moving the V2X application server to the edge reduces round-trip latency by ~10× — enabling real-time cooperative perception and remote control.

### 4.2 Network Slicing & QoS Guarantee

| Slice | Service | Latency | Reliability | Bandwidth |
|---|---|---|---|---|
| **URLLC** | Safety messages, Emergency braking | < 1ms | > 99.999% | Low |
| **eMBB** | Video streaming, HD map update | — | Standard | > 25Mbps |
| **Remote Driving** | Steering/braking commands | < 5ms | > 99.999% | > 25Mbps UL |

> **Trade-off:** Strict slice isolation guarantees QoS but increases infrastructure cost and orchestration complexity. In dense urban environments, resource contention between slices requires careful admission control.

### 4.3 C-V2X Service Requirements (3GPP TS 22.186)

| Use Case | Max Latency | Min Reliability | Min Data Rate |
|---|---|---|---|
| Forward Collision Warning | < 100ms | > 99% | — |
| Cooperative Lane Change | < 100ms | > 99.9% | — |
| **Platooning** | **< 25ms** | **> 99.99%** | — |
| **Remote Driving** | **< 5ms** | **> 99.999%** | **> 25Mbps UL** |

📄 *Source: [1][2][3] 5GAA Use Cases Vol. I–III*

---

## 5. Field Trials & Verification

### 5.1 5G-DRIVE Trial (EU–China Joint Project)

**Setup:** LTE-V2X vs 5G NR V2X tested on UK motorway and urban corridors.

| Metric | LTE-V2X | 5G NR V2X |
|---|---|---|
| Avg. Latency | 19ms | **< 5ms** |
| Packet Error Rate | < 1% | **< 0.01%** |

> **Finding:** 5G NR V2X outperforms LTE-V2X across all KPIs — latency reduced by ~75%, PER reduced by ~100×.

📄 *Source: [4] 5G-DRIVE D4.4 Final Report, EU H2020, 2022*

---

### 5.2 5G-MOBIX: Greece–Turkey — Platooning

**Problem:** Cross-border coverage gaps disrupt platooning convoy continuity at the Greece–Turkey border.

**Applied Technology:**
- 5G NR V2X + eRSU (enhanced Road Side Unit)
- "See What I See" — real-time camera feed shared via PC5/Uu
- eRSU maintains convoy continuity through coverage gaps

**KPI Requirements vs Results:**

| KPI | Requirement | Result |
|---|---|---|
| Latency | < 10ms | ✅ Met |
| Reliability | > 99.99% | ✅ Met |
| Message rate | 10~100 Hz | ✅ Met |

**Design reference:** 5G-MOBIX D2.1, Section 6 (p.55–63), Figure 16 & 17  
📄 *Source: [7] 5G-MOBIX D2.1 v2.0, 2020*

---

### 5.3 5G-MOBIX: Spain–Portugal — Remote Driving

**Problem:** Driverless shuttle requires uninterrupted control signal across the Spain–Portugal border.

**Applied Technology:**
- 5G NR V2X + dual-network redundancy
- Remote Operation Center (ROC) connected via Uu interface
- Dual-link maintains control signal through cross-border handover

**KPI Requirements vs Results:**

| KPI | Requirement | Result |
|---|---|---|
| E2E Latency | < 5ms | ✅ Met |
| Reliability | > 99.999% | ✅ Met |
| UL Data Rate | > 25Mbps | ✅ Met |

**Design reference:** 5G-MOBIX D2.1, Section 8 (p.79–91), Figure 24 & 25  
📄 *Source: [7] 5G-MOBIX D2.1 v2.0, 2020*

---

### 5.4 Verification: Advancing C-V2X (PMC 2023)

| Metric | NR V2X Performance | vs DSRC |
|---|---|---|
| Sidelink Latency | < 3ms | DSRC: ~30ms |
| URLLC Reliability | 99.999% | DSRC: No guarantee |
| PC5 Range | 1km+ | DSRC: ~300m |

> **C-V2X outperforms DSRC in range, reliability, and spectral efficiency across all metrics.**

📄 *Source: [6] Xu et al., Sensors 2023*

---

## 6. KPI Compliance Summary

| Trial / Use Case | Latency Req. | Reliability Req. | Result |
|---|---|---|---|
| Platooning — Greece–Turkey (5G-MOBIX) | < 10ms | > 99.99% | ✅ All KPIs Met |
| Remote Driving — Spain–Portugal (5G-MOBIX) | < 5ms | > 99.999% | ✅ All KPIs Met |
| 5G NR V2X — UK motorway (5G-DRIVE) | < 5ms | > 99.99% | ✅ All KPIs Met |

> **Conclusion:** 5G NR V2X meets all 3GPP Rel-16 requirements across real-world cross-border field trials — demonstrating readiness for Level 4/5 autonomous driving communication.

---

## 7. Key Trade-offs & Limitations

### Trade-off 1: Performance vs. Coverage

Real-world urban environments introduce variables not present in controlled trials:

- Dense vehicle traffic → wireless resource contention
- Building obstruction → signal degradation and NLOS conditions
- High handover frequency → potential transient latency spikes

**Mitigation:** NR V2X uses sensing-based resource selection (Mode 2), congestion control, and QoS-based priority management to maintain stability under high load.

### Trade-off 2: PC5 Security vs. Openness

PC5 direct sidelink communication — by design, infrastructure-free — introduces exposure to spoofing and malicious node injection.

**Current solution:**
- PKI (Public Key Infrastructure) + digital signature per message
- Timestamp synchronization + sequence validation
- Sensor fusion cross-validation (camera, LiDAR, radar)

**Remaining challenge:** Spoofing and malicious node detection remain active research problems, particularly in cross-border multi-operator environments.

### Trade-off 3: MEC Migration vs. Continuity

As vehicles move across MEC domains, service migration introduces potential latency spikes during handover.

**Mitigation:** Dual-network redundancy (PC5 + Uu simultaneously), distributed MEC pre-connection, and fallback safety mode ensure continuity even during edge server transitions.

### Trade-off 4: Network Slicing Overhead vs. QoS Isolation

Strict slice isolation guarantees deterministic QoS for safety-critical services but increases orchestration complexity and infrastructure cost.

**Observation:** As shown in the Bucharest 5G field test (ScienceDirect 2023), co-existence of V2X + eMBB + URLLC slices is achievable, but admission control and resource allocation algorithms become critical at scale.

---

## 8. Q&A Discussion

Key questions raised during the presentation and our answers:

<details>
<summary><b>Q1. When conflicting data arrives from dual networks, how does the system decide which to trust?</b></summary>

Each message carries a **timestamp and sequence number** for recency comparison. The vehicle performs **sensor fusion** — cross-validating network data against onboard camera, LiDAR, and radar. When confidence is low, the system defaults to a conservative **fallback mode** (e.g., speed reduction or safe-stop). NR V2X's dual-mode (PC5 + Uu simultaneously) provides redundancy so conflicting information triggers comparison rather than blind trust.

</details>

<details>
<summary><b>Q2. Can 5ms latency be guaranteed in real dense urban environments?</b></summary>

The 5G-MOBIX and 5G-DRIVE trials were conducted in real urban, motorway, and cross-border environments — not controlled labs. Results confirmed 5ms-level performance under live conditions. However, **dense urban environments introduce additional variables** (congestion, NLOS, handover). The system compensates via MEC, network slicing, congestion control, and sensor fusion — V2X is a complement to onboard sensing, not a sole dependency.

</details>

<details>
<summary><b>Q3. If 5G NR V2X already meets most requirements, why are 6G and AI scheduling still needed?</b></summary>

Current NR V2X is highly capable, but future autonomous driving demands will escalate: **ultra-HD real-time sensor sharing, cooperative perception, digital twin integration, and massive device density** (UAVs, smart city infrastructure). AI-native scheduling enables dynamic, context-aware prioritization of safety-critical traffic. 6G targets not just higher speed, but **ISAC (Integrated Sensing and Communication)** and native AI support — addressing scenarios where NR V2X's architecture becomes the bottleneck.

</details>

<details>
<summary><b>Q4. Is PC5 communication reliable when hundreds of vehicles are present simultaneously (e.g., rush hour)?</b></summary>

High vehicle density risks **radio resource collision and congestion**. NR V2X addresses this via **sensing-based resource selection (Mode 2 SB-SPS)**, QoS-based message prioritization, and congestion control algorithms. Additionally, PKI + digital signatures authenticate each vehicle so malicious or spoofed nodes are filtered before sensor fusion. Platooning in particular benefits from synchronized driving — vehicles react near-simultaneously rather than sequentially, improving traffic flow efficiency.

</details>

<details>
<summary><b>Q5. What happens when a vehicle's "See What I See" video stream drops?</b></summary>

"See What I See" is a **supplementary function**, not a primary dependency. If one vehicle's video stream fails, the receiving vehicle continues operation using its own onboard sensors (LiDAR, radar, camera). NR V2X dual-mode (PC5 + Uu) provides an alternate communication path. The system is designed to avoid **single points of failure** — a dropped stream triggers graceful degradation, not system failure.

</details>

<details>
<summary><b>Q6. Does MEC handover maintain latency and safety when a vehicle crosses into a new MEC domain?</b></summary>

5G-MOBIX and 5GCroCo cross-border trials validated **communication continuity across operator and country boundaries** — a far more complex scenario than single MEC migration. Dual-network redundancy and distributed MEC pre-connection allow the new edge server to prepare before handover completes. 5GCroCo D4.3 measured **cross-border handover time of ~100ms** — imperceptible in practice. Fallback redundancy ensures safety-critical services remain active throughout.

</details>

<details>
<summary><b>Q7. Who should lead the infrastructure investment — automakers, telecom operators, or governments?</b></summary>

No single entity can build this alone. The EU project model demonstrates the realistic answer: **public-private multi-stakeholder collaboration**.  
- **Telecom operators:** 5G gNB, MEC, network slicing deployment  
- **Automakers:** C-V2X chipset integration, onboard sensor systems  
- **Government / Public bodies:** RSU / smart road infrastructure, standardization, spectrum allocation, legal frameworks  

5G-MOBIX and 5GCroCo themselves involved government agencies, telecom operators, automakers, and research institutions across multiple countries — reflecting that C-V2X is a national transport infrastructure challenge, not a product feature.

</details>

---

## 9. Conclusion & Future Directions

### Key Findings

1. **5G NR V2X meets all 3GPP Rel-16 safety requirements** across real-world EU cross-border trials.
2. **MEC reduces round-trip latency from ~50ms to < 5ms**, enabling real-time cooperative perception.
3. **Network slicing guarantees QoS isolation** for safety-critical V2X traffic alongside eMBB services.
4. **eRSU and dual-network redundancy** solve coverage gaps and cross-border handover continuity.
5. **Technology flow confirmed:** 3GPP Standards → Industry Deployment → Field Verification.

### Future Directions

| Area | Description |
|---|---|
| **6G V2X** | AI-native network, ISAC, sub-1ms latency targets, digital twin support |
| **AI Resource Scheduling** | Dynamic, context-aware prioritization of safety-critical V2X traffic at scale |
| **Digital Twin Integration** | Real-time virtual replica of road environment for predictive V2X decisions |
| **Security** | Scalable PKI, misbehavior detection, cross-border trust frameworks |

---

## 10. References

```
[1] 5GAA, "C-V2X Use Cases and Service Level Requirements, Vol. I,"
    5G Automotive Association, 2019.

[2] 5GAA, "C-V2X Use Cases and Service Level Requirements, Vol. II,"
    5G Automotive Association, Tech. Rep. v2.0, 2020.

[3] 5GAA, "C-V2X Use Cases and Service Level Requirements, Vol. III,"
    5G Automotive Association, 2022.

[4] 5G-DRIVE Consortium, "Final Report of V2X Trials,"
    5G-DRIVE Project Deliverable D4.4, EU H2020, 2022.

[5] 5G-PPP, "5G for CCAM in Cross-Border Corridors,"
    5G Infrastructure Public Private Partnership White Paper, 2023.
    https://5g-ppp.eu/wp-content/uploads/2023/11/5GInfraPPP_10TPs_Brochure2023_web.pdf

[6] Z. Xu et al., "Advancing C-V2X for Level 5 Autonomous Driving
    from the Perspective of 3GPP Standards,"
    Sensors, vol. 23, no. 4, p. 2261, Feb. 2023.
    https://pmc.ncbi.nlm.nih.gov/articles/PMC9967342/

[7] 5G-MOBIX Consortium, "5G-Enabled CCAM Use Cases and Specifications,"
    5G-MOBIX Project Deliverable D2.1, v2.0, 2020.
    https://www.5g-mobix.com/assets/files/5G-MOBIX-D2.1-5G-enabled-CCAM-use-cases-specifications-V2.0.pdf

[8] 5G-PPP, "5G Infrastructure PPP — 10 Technical Priorities,"
    5G Infrastructure Public Private Partnership, Brochure, Nov. 2023.
    https://5g-ppp.eu/wp-content/uploads/2023/11/5GInfraPPP_10TPs_Brochure2023_web.pdf
```

---

<div align="center">

**Korea University Sejong Campus · DCCS307 Computer Networks · 2026**  
Team: Sungjune Kong · Junmok Lee · Lana Kim

</div>
