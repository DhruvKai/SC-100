---
tags:
  - sc100
type: service
domain:
  - infrastructure
aliases:
  - IoT Edge
---

# Azure IoT Edge

> **In one line:** IoT Edge runs **cloud logic as containers on a local device** and can act as a **gateway**, so legacy or offline devices still connect to IoT Hub securely.

## Purpose

Device runtime that moves workloads (containerized **modules**) to the edge and connects downstream devices to [[IoT Device Authentication and Transport Security|IoT Hub]].

---

## Key Facts

| Item | Detail |
| --- | --- |
| **Runtime** | **Edge Agent** (deploys and monitors modules) + **Edge Hub** (local IoT Hub proxy, offline store-and-forward) |
| **Identity** | Device identity in IoT Hub (X.509 or TPM via DPS); each module gets its own **module identity** |
| **Security manager** | Uses a hardware root of trust (TPM/HSM) where available ([[Trusted Platform Module (TPM)]]) |
| **Gateway patterns** | **Transparent** (devices keep own identity), **protocol translation**, **identity translation** |
| **Nested edge** | Layered gateways for **ISA-95 / Purdue** networks: lower layers reach the cloud only via a parent ([[OT and ICS Security]]) |

---

## When to Use

- Devices that **can't do TLS, X.509 or MQTT/AMQP** → protocol/identity translation gateway.
- Local processing, filtering or AI with **intermittent connectivity**.
- **Segmented OT networks** where lower layers must never talk to the internet directly → nested edge.

## When NOT to Use

- Devices that already authenticate to IoT Hub and need no local compute.
- Threat detection on OT networks → Defender for IoT ([[OT and ICS Security]]).

---

## Exam Tips

- **"Legacy devices can't authenticate to IoT Hub"** → IoT Edge **field gateway** (translation).
- **"Purdue model, lower layers have no internet"** → **nested** IoT Edge.
- **"Downstream devices keep their own identities"** → **transparent** gateway.

## AZ-500 Overlap

- Not core AZ-500. Known: certificates, TPM, managed identities as concepts.

## What's New in SC-100

- IoT Edge is the **identity and network boundary** between OT/IoT and cloud: a Zero Trust choke point for constrained devices.

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **IoT Edge vs. IoT Hub** | Runs on the device/gateway vs. the cloud endpoint devices connect to |
| **Transparent vs. translation gateway** | Devices keep their own IoT Hub identities vs. gateway speaks for them |
| **IoT Edge vs. Defender for IoT** | Runtime/gateway vs. security monitoring and threat detection |

## Keywords

- Field gateway, edge modules, Edge Agent, Edge Hub, offline operation
- Transparent / protocol translation / identity translation gateway
- Nested edge, ISA-95, Purdue model, module identity, DPS, TPM

## Related Services

- [[IoT Device Authentication and Transport Security]] · [[OT and ICS Security]] · [[Trusted Platform Module (TPM)]]
- [[Securing Server and Client Endpoints]] · [[Zero Trust]]

## References

- [What is Azure IoT Edge?](https://learn.microsoft.com/en-us/azure/iot-edge/about-iot-edge): Microsoft Learn
- [IoT Edge as a gateway](https://learn.microsoft.com/en-us/azure/iot-edge/iot-edge-as-gateway): Microsoft Learn
- [Hierarchical (nested) IoT Edge](https://learn.microsoft.com/en-us/azure/iot-edge/tutorial-nested-iot-edge): Microsoft Learn
