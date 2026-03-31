# Polaris: Real-Time ADS-B Telemetry & Aerial Data Pipeline

Polaris is a high-performance tracking station and data engineering pipeline designed to ingest, decode, and visualize real-time ADS-B (Automatic Dependent Surveillance–Broadcast) telemetry. Built on a **Raspberry Pi 5** architecture, the project demonstrates the integration of Software Defined Radio (SDR) with modern data processing workflows to monitor global aviation traffic from a localized terrestrial position.

## Project Overview

In modern aviation, ADS-B is the primary standard for tracking aircraft positions via satellite navigation. Polaris captures these raw 1090MHz radio frequency broadcasts, decodes the Mode S packets, and transforms them into structured data for analysis. This project serves as a testbed for handling high-velocity streaming data, signal optimization, and edge-compute resource management.



## Technical Stack

* **Hardware:** Raspberry Pi 5 (8GB), FlightAware Pro Stick Plus (SDR USB Stick), and a 1090MHz ADS-B specialized high-gain antenna.
* **Operating System:** Fedora Linux.
* **Data Ingestion:** `dump1090-fa` for radio frequency decoding and digital signal processing (DSP).
* **Pipeline & Logic:** Python-based scripts for data normalization, coordinate validation, and API distribution.
* **Observability:** Integrated with the **Theia** monitoring framework to track CPU utilization, SDR thermal performance, and message-per-second (MPS) rates.

## Core Features

* **Real-Time Aerial Tracking:** Decodes aircraft identification, altitude, velocity, latitude, and longitude with sub-second latency.
* **Automated Data Logging:** Efficiently stores historical flight telemetry to identify local traffic patterns and seasonal aviation trends.
* **Edge Processing:** Performs data cleaning and altitude filtering locally on the Raspberry Pi before transmitting to upstream dashboards or databases.
* **High-Availability Design:** Engineered with automated service recovery and systemd watchdog timers to ensure 24/7/365 operational uptime.

## Engineering & Research Goals

Beyond simple tracking, Polaris explores several complex engineering challenges:
1. **RF Signal Optimization:** Tuning gain levels and managing signal-to-noise ratios (SNR) in a high-interference urban environment.
2. **Stream Processing:** Minimizing the "glass-to-glass" latency between radio packet reception and visual mapping.
3. **Hardware Benchmarking:** Evaluating the thermal and computational overhead of the Raspberry Pi 5 when processing high-density airspace data (up to 2,000+ messages per second).

## Ecosystem Integration

Polaris is a core component of the broader laboratory ecosystem:
* **Theia:** Provides the telemetry dashboard for hardware health and signal strength.
* **TON618:** Utilizes secure VPN tunnels for remote management of the Polaris tracking node.

---

Maintained by Scienz_Guy | 2026
