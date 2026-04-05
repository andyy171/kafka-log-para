# Kafka Infrastructure Observability Platform

A production-inspired Kafka-centered platform for infrastructure monitoring, operational observability, and reliability analysis in distributed systems.

---

## 1. Project Overview

This project is a **Kafka-centered observability and reliability platform** designed for distributed infrastructure environments.

Its main goal is to demonstrate how **Apache Kafka** can serve as the **core operational event backbone** for infrastructure systems, rather than being treated as just another component in a larger pipeline.

Instead of sending monitoring signals, logs, or alerts directly to downstream systems, this platform uses Kafka as the central layer for:

- ingesting operational events
- buffering burst traffic
- decoupling producers from consumers
- distributing events to multiple downstream functions
- retaining data for replay and recovery

The **primary use case** of the project is **infrastructure monitoring**, while additional supporting event flows may include:

- system logs
- security-related alerts
- autoscale-needed signals

This project is built in a self-managed lab environment using virtual machines, with an emphasis on **Kafka operations**, **consumer lag visibility**, **reliability behavior**, and **production-inspired observability design**.

- Project Structure:
```
├── README.md
├── architecture/
│   ├── overview.md
│   ├── topology.png
│   └── flow.png
├── infra/
│   ├── inventory.ini
│   ├── ansible/
│   │   ├── deploy-kafka.yml
│   │   ├── deploy-monitoring.yml
│   │   └── deploy-consumers.yml
│   └── docker-compose.dev.yml
├── config/
│   ├── kafka/
│   │   ├── broker-1.properties
│   │   ├── broker-2.properties
│   │   ├── broker-3.properties
│   │   └── create-topics.sh
│   ├── prometheus/
│   │   └── prometheus.yml
│   ├── grafana/
│   │   ├── dashboards/
│   │   └── provisioning/
│   └── consumers/
│       ├── monitoring-consumer.yaml
│       ├── alert-consumer.yaml
│       └── replay-consumer.yaml
├── producers/
│   ├── monitoring-simulator/
│   ├── syslog-simulator/
│   ├── security-simulator/
│   └── autoscale-signal-generator/
├── consumers/
│   ├── monitoring-consumer/
│   ├── alert-consumer/
│   └── replay-consumer/
├── scripts/
│   ├── start-demo.sh
│   ├── stop-broker.sh
│   ├── restart-broker.sh
│   ├── reset-offset.sh
│   └── load-test.sh
└── results/
    ├── screenshots/
    └── notes.md
```


---

## 2. Problem Statement

In real distributed infrastructure, operational signals are generated continuously from many sources:

- host-level metrics
- service health states
- system and application logs
- anomaly or security-related events
- scaling pressure indicators

If those signals are pushed directly to dashboards, storage, or alerting systems, the platform can become tightly coupled, harder to scale, and more fragile under burst traffic or downstream failures.

This project explores a more resilient approach:

- use Kafka as the central ingestion and distribution platform
- separate event production from event consumption
- allow multiple operational consumers to work independently
- keep events available for replay and reprocessing
- observe system behavior under load and failure conditions

---

## 3. Objectives

The project aims to demonstrate:

- how Kafka acts as an ingestion backbone for infrastructure events
- how partitioning supports scalability and parallelism
- how buffering helps absorb burst traffic
- how consumer lag reflects downstream pressure
- how retained events can be replayed for debugging or recovery
- how Kafka behaves under broker failure and recovery scenarios
- how an infrastructure-focused observability platform can be designed with Kafka at the center

---

## 4. Core Use Cases

### Primary Use Case
- **Infrastructure Monitoring**
  - CPU, memory, disk pressure
  - service health state changes
  - node or process status events
  - infrastructure threshold breaches

### Supporting Use Cases
- **System Logging**
  - syslog-style events
  - service error logs
  - runtime operational logs

- **Security Alerts**
  - failed authentication bursts
  - suspicious access patterns
  - simple anomaly-like operational security events

- **Autoscale-needed Signals**
  - threshold-based scale recommendations
  - sustained high-load conditions
  - capacity pressure signals

---

## 5. High-Level Architecture

The platform follows this overall flow:

```text
Operational Signals
        |
        v
Collectors / Producers / Simulators
        |
        v
Kafka Cluster (core event backbone)
        |
        +--> Monitoring Consumers
        +--> Alert Consumers
        +--> Replay / Archive Consumers
        +--> Analysis / Capacity Consumers
        |
        v
Dashboards / Alerts / Operational Insights