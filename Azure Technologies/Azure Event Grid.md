---
tags:
  - sc100
type: service
domain:
  - apps-data
  - ops-identity-compliance
aliases:
  - Event Grid
---

# Azure Event Grid

> **In one line:** Event Grid **pushes "something happened" notifications** (a blob was uploaded, a secret is about to expire) to whatever should react: a Function, Logic App or webhook.

## Purpose

Fully managed publish/subscribe **event routing** service for discrete, reactive events.

---

## Key Facts

| Item | Detail |
| --- | --- |
| **Model** | Publisher → **topic** → **event subscription** (with filters) → **handler** |
| **Topics** | System topics (Azure services), custom topics, partner topics; namespaces add an **MQTT** broker |
| **Handlers** | [[Azure Functions]], [[Logic Apps]], webhooks, [[Azure Event Hubs]], [[Azure Service Bus]], Storage queues |
| **Delivery** | Push, at-least-once, retries, **dead-lettering** to storage |
| **Security** | Entra ID auth for publishing, **disable local (SAS key) auth**, **managed identity** for delivery, private endpoints, webhook validation handshake |

## Security Use Cases

- [[Key Vault]] **SecretNearExpiry** event → Function rotates the secret.
- Defender for Storage **malware scanning result** → Function/Logic App quarantines or deletes the blob ([[Cloud Workload Protection (CWPP)]]).
- Resource-group / subscription change events → react to configuration drift.

---

## When to Use

- React to state changes in near real time, serverless.
- Fan one event out to many handlers.

## When NOT to Use

- High-volume telemetry or log streaming → [[Azure Event Hubs]].
- Ordered, transactional business messages → [[Azure Service Bus]].
- Defender for Cloud alert automation → [[Defender for Cloud Workflow Automation]] (Logic Apps).

---

## Exam Tips

- **"React to blob upload / resource change / secret expiry"** → Event Grid.
- **"Stream logs to a third-party SIEM"** → Event Hubs, not Event Grid.
- **Harden it:** disable SAS keys, use Entra ID + managed identity, private endpoints.

## AZ-500 Overlap

- Known: managed identities, private endpoints, Key Vault. New: Event Grid as the glue for **event-driven security automation**.

## What's New in SC-100

- Choose the right messaging service for automation design: route (Event Grid), stream (Event Hubs), or queue (Service Bus).

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Event Grid vs. Event Hubs** | Discrete events pushed to handlers vs. high-throughput stream that consumers pull |
| **Event Grid vs. Service Bus** | Lightweight notification ("it happened") vs. the message **is** the work, with transactions and ordering |
| **Event Grid vs. Azure Monitor action groups** | Resource events routing vs. alert-rule notifications ([[Azure Monitor Alerts and Action Groups]]) |

## Keywords

- Event routing, publish/subscribe, push, reactive, serverless
- System / custom / partner topics, event subscription, filters, handlers, dead-lettering
- BlobCreated, SecretNearExpiry, malware scanning results, MQTT broker

## Related Services

- [[Azure Event Hubs]] · [[Azure Service Bus]] · [[Azure Functions]] · [[Logic Apps]]
- [[Key Vault]] · [[Defender for Cloud Workflow Automation]] · [[Architecture Decisions]]

## References

- [What is Azure Event Grid?](https://learn.microsoft.com/en-us/azure/event-grid/overview): Microsoft Learn
- [Choose between Azure messaging services](https://learn.microsoft.com/en-us/azure/service-bus-messaging/compare-messaging-services): Microsoft Learn
