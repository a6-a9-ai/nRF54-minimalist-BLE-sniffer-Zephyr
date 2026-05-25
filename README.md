# BLE Sniffer for nRF54L15

This project implements a high-efficiency, passive Bluetooth Low Energy (BLE) advertising packet sniffer on the Nordic nRF54L15-DK, powered by Zephyr RTOS. It is architected for a minimal memory footprint and zero-copy data pipelines to maximize throughput and minimize packet drops.

## Features
- **Passive Continuous Scanning:** Intercepts nearby BLE advertising packets without active scanning overhead or transmission footprints.
- **Dynamic Dual-Window Intervals:** Alternates scan windows to optimize packet catch rates across varying advertising intervals.
- **Advanced GAP Payload Parsing:** Extracts and decodes Flags, Local Names, TX Power, 16-bit/128-bit Service UUIDs, Service Data, and Manufacturer Data.
- **Embedded Hardware Lookups:** Built-in zero-latency dictionary mapping for common Ecosystems (Apple Find My, Google Fast Pair, Samsung SmartThings) and hardware manufacturers.
- **Deferred Logging Architecture:** Thread-isolated logging utilizing Zephyr's deferred logging mode via UART/RTT to eliminate timing jitter in critical sections.

---

## System Architecture

The sniffer decouples the physical RF reception from the asynchronous processing and logging layers to guarantee that high-frequency packet bursts do not result in buffer overruns.

```mermaid
graph TD
    A[Radio] --> B[Zephyr BLE Stack]
    B -->|device_found| C[Application]
    C -->|Analyse TLV| D[Decoder]
    D -->|Buffer| E[Logger Engine]
    E -->|Asynchrone| F[Console RTT/UART]

