miniNORAD: Real-Time ADS-B Telemetry & Aerial Data Pipeline

miniNORAD is a high-performance tracking station and data engineering pipeline designed to ingest, decode, and visualize real-time ADS-B (Automatic Dependent Surveillance–Broadcast) telemetry. Built on a **Raspberry Pi 5** architecture, the project demonstrates the integration of Software Defined Radio (SDR) with modern data processing workflows to monitor global aviation traffic from a localized terrestrial position.

## Project Overview

In modern aviation, ADS-B is the primary standard for tracking aircraft positions via satellite navigation. The system captures these raw 1090MHz radio frequency broadcasts, decodes the Mode S packets, and transforms them into structured data for analysis. This project serves as a testbed for handling high-velocity streaming data, signal optimization, and edge-compute resource management.

## Technical Stack

* **Hardware:** Raspberry Pi 5 (8GB), FlightAware Pro Stick Plus (SDR USB Stick), and a 1090MHz ADS-B specialized high-gain antenna.
* **Operating System:** Fedora Linux.
* **Data Ingestion:** `dump1090-fa` for radio frequency decoding and digital signal processing (DSP).
* **Pipeline & Logic:** Python-based scripts for data normalization, coordinate validation, and API distribution.
* **Observability:** Integrated with the **Theia** monitoring framework to track CPU utilization, SDR thermal performance, and message-per-second (MPS) rates.

## Hardware Engineering & Environmental Hardening

To ensure 24/7/365 operational continuity, miniNORAD is housed in a custom-engineered, weatherproof enclosure designed to withstand the Mid-Atlantic climate of Wilmington, DE. This external deployment minimizes RF interference and maximizes line-of-sight signal acquisition for the 1090MHz antenna.

### Enclosure Specifications:
* **Climate Control:** Integrated passive and active cooling systems to manage the thermal output of the Raspberry Pi 5 during peak summer temperatures.
* **Environmental Sealing:** IP66-rated chassis protecting internal components from moisture, dust, and debris.
* **Power Delivery:** Surge-protected Power-over-Ethernet (PoE) implementation, allowing for a single-cable deployment for both data and high-stability power.

### Infrastructure Monitoring & Alerting:
The enclosure is a "smart" node integrated directly into the **Theia** observability suite. Polaris doesn't just track aircraft; it tracks its own health:
* **Thermal Watchdog:** Real-time monitoring of CPU and SDR temperatures. If thresholds are breached, the system triggers an automated fan-curve adjustment or a graceful thermal throttle.
* **Power Analytics:** Monitoring for voltage drops or power instability that could indicate hardware fatigue or cable degradation.
* **System Health Alerts:** Critical hardware failures (e.g., SDR disconnects or storage read/write errors) are dispatched via the **Telegraph** (Telegram) alert engine for immediate remediation.

## Maintenance & Sustainability
The physical design prioritizes modularity, allowing for rapid component swaps (SDR, filters, or compute modules) with minimal downtime. This reflects a commitment to high-availability (HA) infrastructure principles even at the edge-computing level.

## Core Features

* **Real-Time Aerial Tracking:** Decodes aircraft identification, altitude, velocity, latitude, and longitude with sub-second latency.
* **Automated Data Logging:** Efficiently stores historical flight telemetry to identify local traffic patterns and seasonal aviation trends.
* **Edge Processing:** Performs data cleaning and altitude filtering locally on the Raspberry Pi before transmitting to upstream dashboards or databases.
* **High-Availability Design:** Engineered with automated service recovery and systemd watchdog timers to ensure 24/7/365 operational uptime.
* **Messaging:** Telegram Bot API (Telegraph).
* **Database:** SQLite / JSON-based lookup tables for ICAO hex identification.
* **Logic:** Asynchronous Python (asyncio) to ensure non-blocking alert dispatch during high-traffic intervals.

## Engineering & Research Goals

Beyond simple tracking, Polaris explores several complex engineering challenges:
1. **RF Signal Optimization:** Tuning gain levels and managing signal-to-noise ratios (SNR) in a high-interference urban environment.
2. **Stream Processing:** Minimizing the "glass-to-glass" latency between radio packet reception and visual mapping.
3. **Hardware Benchmarking:** Evaluating the thermal and computational overhead of the Raspberry Pi 5 when processing high-density airspace data (up to 2,000+ messages per second).

## High-Priority Event Orchestration (Telegraph)

miniNORAD includes a specialized "Telegraph" notification engine designed to identify and alert on high-interest aerial targets in real-time. By cross-referencing live Hex codes against curated databases, the system pushes instant notifications to mobile devices when specific criteria are met.

### Alert Categories:
* **Military & Government:** Identification of transport, tactical, and surveillance aircraft operating within the local sector.
* **VIP & Executive:** Monitoring for civilian aircraft associated with state officials or high-profile transit.
* **Medevac & Emergency:** Real-time detection of life-flight and emergency medical services (EMS) utilizing priority transponder codes.

### Implementation Logic:
1. **Filtering:** A Python-based event listener monitors the `dump1090` JSON stream for specific ICAO Hex codes and squawk codes (e.g., 7700 for emergencies).
2. **Logic Engine:** Conditional triggers evaluate the aircraft's proximity, altitude, and heading to determine if an alert is warranted.
3. **Dispatch:** Verified events are formatted and dispatched via the Telegram Bot API ("Telegraph") to a private encrypted channel.

## Ecosystem Integration

miniNORAD is a core component of the broader laboratory ecosystem:
* **Theia:** Provides the telemetry dashboard for hardware health and signal strength.
* **TON618:** Utilizes secure VPN tunnels for remote management of the Polaris tracking node.

---

Maintained by Scienz_Guy | 2026
