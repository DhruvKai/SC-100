---
tags:
  - sc100
type: service
domain:
  - apps-data
aliases:
  - Service Bus
---

# Azure Service Bus

> **In one line:** Service Bus is an **enterprise message broker**. It reliably queues **high-value messages** (orders, payments) so they're processed once, in order, even if the receiver is down.

## Purpose

Fully managed message broker with **queues** (point-to-point) and **topics/subscriptions** (publish/subscribe) for decoupled applications.

---

## Key Facts

| Item | Detail |
| --- | --- |
| **Features** | FIFO **sessions**, **dead-letter queue**, **duplicate detection**, **transactions**, scheduled/deferred messages |
| **Tiers** | Basic (queues only), Standard (topics), **Premium** (dedicated capacity, private endpoints, CMK, geo-replication/geo-DR) |
| **Auth** | **Entra ID RBAC** (Data Sender / Data Receiver / Data Owner) + managed identity preferred; SAS keys can be **disabled** |
| **Network** | Private endpoints and VNet integration: **Premium** tier |
| **Encryption** | At rest by default; **CMK** needs Premium |

---

## Why Architects Choose It

- Decouples application tiers, so a failure or spike in one doesn't lose messages.
- Guaranteed processing for transactional workflows.

## When to Use

- Orders, payments, workflow commands: anything that **must not be lost or duplicated**.
- Load levelling between producers and slower consumers ([[Azure Well-Architected Framework (WAF)|WAF]] queue-based load leveling).

## When NOT to Use

- Telemetry or log streaming at massive scale → [[Azure Event Hubs]].
- Simple "something changed" notifications → [[Azure Event Grid]].

---

## Exam Tips

- **"Transactions, FIFO, duplicate detection, dead-letter"** → Service Bus.
- **"Private endpoint / CMK / isolation"** for Service Bus → **Premium** tier.
- **Secure by design:** Entra ID RBAC + managed identity, disable SAS (local auth).

## AZ-500 Overlap

- Known: managed identity, RBAC, private endpoints, CMK. SC-100 adds choosing the messaging service and the tier that unlocks network isolation.

## What's New in SC-100

- In the [[Securing IaaS and PaaS Services|PaaS hardening]] design, the tier decision controls whether network isolation is even possible.

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Service Bus vs. Event Hubs** | Per-message reliability and transactions vs. bulk telemetry stream with partitions |
| **Service Bus vs. Event Grid** | The message is the work (pull, ordered) vs. notification pushed to handlers |
| **Service Bus vs. Storage queues** | Rich broker features (sessions, topics, transactions) vs. simple, cheap, very large queues |
| **Queue vs. topic** | One consumer per message vs. many subscriptions each get a copy |

## Keywords

- Message broker, queue, topic, subscription, FIFO sessions, dead-letter queue
- Duplicate detection, transactions, scheduled messages, Premium tier
- Azure Service Bus Data Sender / Receiver / Owner, disable local auth

## Related Services

- [[Azure Event Hubs]] · [[Azure Event Grid]] · [[Azure Functions]] · [[Logic Apps]]
- [[Securing IaaS and PaaS Services]] · [[Private Link]] · [[Architecture Decisions]]

## References

- [What is Azure Service Bus?](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview): Microsoft Learn
- [Service Bus security baseline](https://learn.microsoft.com/en-us/security/benchmark/azure/baselines/service-bus-security-baseline): Microsoft Learn
