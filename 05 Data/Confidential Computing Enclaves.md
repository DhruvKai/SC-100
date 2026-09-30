---
tags:
  - sc100
type: concept
domain:
  - data
  - infrastructure
aliases:
  - Confidential Computing
  - Azure Confidential Computing
  - Intel SGX
  - SGX enclaves
  - Application enclaves
  - Encryption in use
status: needs-verification
---

# Confidential Computing Enclaves

## Purpose

Hardware-isolated regions of CPU and memory (Intel **SGX**) where code and data stay **encrypted while being processed**. Nobody outside the enclave can read them, not even the host OS, a debugger or the cloud operator.

---

## Why Architects Choose It

- Closes the **encryption in use** gap in the at-rest / in-transit / in-use triad (see [[Data Classification and Protection]]).
- **Smallest trusted computing base (TCB)**: only the enclave code is trusted. The host OS, hypervisor and other apps on the VM are **untrusted**.
- Protects against **insider/operator access** and a compromised host. Supports [[Zero Trust]] "assume breach" for the data plane.
- Enables **multi-party computation**: parties combine sensitive data without revealing it to each other (e.g. fraud detection across banks).
- Trade-off: **code changes are needed**. The app must be partitioned, unlike confidential VMs.

---

## How Enclaves Work

- **Lockbox model**: encrypted code/data goes in, the enclave gets a key, processes the data, re-encrypts it and sends it out.
- If untrusted code tries to change enclave memory, **SGX disables the environment**.
- **Application partitioning**:

| Partition | Trust | Contains |
| --- | --- | --- |
| **Host** | Untrusted | The rest of the app. It can't see or change enclave code after load. |
| **Enclave** | Trusted | Sensitive code, cached data, memory, secrets |

- **Attestation**: prove to a relying party that the right code runs in a genuine enclave before releasing secrets. Uses **Microsoft Azure Attestation**.
- **Encrypted Page Cache (EPC)** limits enclave memory:

| VM series | Hardware | EPC max |
| --- | --- | --- |
| **DCsv2** | Intel SGX | 168 MiB |
| **DCsv3 / DCdsv3** | Intel SGX | 256 GB |

- **Dev tools** (open-source): **Open Enclave SDK**, **Intel SGX SDK**, **EGo SDK**, **Confidential Consortium Framework (CCF)**.

---

## When to Use

- Protecting specific algorithms, keys or data while they're being processed, with the smallest possible TCB.
- Multi-party data analytics or ML where no party may see the others' data.
- Protecting against privileged insiders and compromised hosts.
- Developers can modify the app (or use an enclave-aware framework).

## When NOT to Use

- **Lift-and-shift with no code changes** → **Confidential VMs** (AMD SEV-SNP / Intel TDX).
- Only need boot integrity (Secure Boot/vTPM) → **Trusted Launch** (not confidential computing itself; see [[Trusted Platform Module (TPM)]]).
- Only need column-level DBA-blind encryption in SQL → **Always Encrypted** (optionally *with secure enclaves*) ([[SQL Data Protection (TDE, Ledger, TDS 8.0)]]).
- Large memory workloads on DCsv2 (168 MiB EPC). Use DCsv3 or confidential VMs.

---

## Architecture Decisions

```
Need data protected while in use?
  ↓
Can you change/partition the code?
  ├─ Yes → Application enclave (Intel SGX, DCsv2/DCsv3) — smallest TCB
  └─ No  → Confidential VM (AMD SEV-SNP DCasv5/ECasv5, Intel TDX DCesv6/ECesv6)

Containers?
  ├─ AKS → Confidential VM node pools (or SGX enclave-aware containers)
  └─ ACI → Confidential containers (CCE policies + attestation)

SQL queries on encrypted columns?
  ↓
Always Encrypted with secure enclaves

GPU / AI inference on sensitive data?
  ↓
Confidential GPU VMs (NCCadsH100v5)

Keys in a TEE / tamper-evident records?
  ↓
Managed HSM / Azure Confidential Ledger
```

---

## Azure Confidential Computing Landscape

| Offering | Isolation level | Code changes | Hardware |
| --- | --- | --- | --- |
| **Application enclaves** | Process/app partition | **Yes** (SDK/framework) | Intel SGX (DCsv2, DCsv3/DCdsv3) |
| **Confidential VMs** | Whole VM | **No** (rehost) | AMD SEV-SNP (DCasv5/ECasv5), Intel TDX (DCesv6/ECesv6) |
| Confidential GPU VMs | VM + GPU TEE | No | NCCadsH100v5 |
| Confidential AKS nodes | Worker node (CVM) | No | AMD SEV-SNP |
| Confidential containers (ACI) | Container group | No (CCE policy) | — |
| Always Encrypted w/ secure enclaves | Database engine | Minimal | VBS / SGX |
| Azure Confidential Ledger | Service | — | Tamper-evident store |
| Microsoft Azure Attestation | Service | — | Verifies TEEs |
| Key Vault Managed HSM | Service | — | Single-tenant HSM ([[Key Vault]]) |

---

## Comparison

| Compare | Difference |
| --- | --- |
| **Enclave vs. confidential VM** | Enclave: smallest TCB (only enclave code trusted), code must be partitioned. CVM: whole VM encrypted in memory, guest OS inside the TCB, **no code changes**. |
| Confidential VM vs. Trusted Launch | CVM encrypts memory against the host/operator. Trusted Launch = Secure Boot + vTPM + boot integrity monitoring (no memory encryption). |
| Enclave vs. Always Encrypted | General-purpose TEE for any code vs. SQL column encryption. *With secure enclaves* adds rich queries on encrypted columns inside a TEE. |
| Encryption in use vs. at rest (TDE/CMK) vs. in transit (TLS) | Processing-time protection vs. storage vs. network. A complete design covers all three. |
| Intel SGX vs. AMD SEV-SNP / Intel TDX | App-level enclave vs. VM-level TEE |

---

## AZ-500 Overlap

AZ-500 covers encryption at rest/in transit, Key Vault, Azure Disk Encryption and Always Encrypted configuration. Confidential computing is mostly new.

## What's New in SC-100

- Recommend **encryption in use** as part of a data protection architecture.
- Choose **enclave vs. confidential VM** based on TCB size and code-change tolerance.
- Know **attestation** (Microsoft Azure Attestation) as the trust step before secrets are released.
- Microsoft uses confidential computing internally ([[Secure Future Initiative (SFI)]]): Entra ID key material, code signing, payments.

---

## Exam Tips

- "Protect data **while processed**, even from cloud operators/admins" → confidential computing.
- "Smallest TCB / only trust the app code" → **SGX enclave** (DCsv2/DCsv3).
- "Lift-and-shift, no code changes, protect in use" → **confidential VM**.
- "Verify the enclave before sending secrets" → **Microsoft Azure Attestation**.
- "Enclave memory size" → EPC: DCsv2 168 MiB, DCsv3 up to 256 GB.
- "Host = untrusted, enclave = trusted" — partition the app accordingly.

## Common Exam Confusion

- **Enclave vs. confidential VM**: code changes + smallest TCB vs. no changes + larger TCB.
- **Trusted Launch vs. confidential VM**: boot integrity vs. memory encryption.
- **Always Encrypted vs. enclaves**: SQL-specific vs. general-purpose.
- **DCsv2/DCsv3 (SGX, enclaves) vs. DCasv5/ECasv5 (SEV-SNP, CVMs)**: DC*s* = SGX; DC*as* = AMD confidential VM.

---

## Keywords

- Confidential computing, encryption in use, trusted execution environment (TEE)
- Intel SGX, enclave, Encrypted Page Cache (EPC)
- DCsv2, DCsv3, DCdsv3
- Trusted vs. untrusted component, application partitioning
- Trusted computing base (TCB)
- Microsoft Azure Attestation
- Open Enclave SDK, Intel SGX SDK, EGo, Confidential Consortium Framework
- Confidential VM, AMD SEV-SNP, Intel TDX
- Always Encrypted with secure enclaves, Azure Confidential Ledger

---

## Related Services

- [[Data Classification and Protection]]
- [[SQL Data Protection (TDE, Ledger, TDS 8.0)]]
- [[Key Vault]]
- [[Zero Trust]]
- [[Trusted Platform Module (TPM)]]
- [[Secure Future Initiative (SFI)]]
- [[Container and Kubernetes Security]]
- [[AI and Copilot Security Architecture]]
- [[Double Key Encryption (DKE)]]
- [[Shared Responsibility Model]]

---

## References

- [Build with SGX enclaves](https://learn.microsoft.com/en-us/azure/confidential-computing/confidential-computing-enclaves) — Microsoft Learn
- [Confidential computing development tools](https://learn.microsoft.com/en-us/azure/confidential-computing/application-development) — Microsoft Learn
- [Azure confidential computing products](https://learn.microsoft.com/en-us/azure/confidential-computing/overview-azure-products) — Microsoft Learn

---

## Verification Flag

- The TCB comparison (enclave = smallest TCB; CVM = guest OS inside the TCB) is standard Azure confidential computing guidance, but it isn't stated on the three pages fetched. Re-check against the confidential computing overview.
- Always Encrypted secure enclave types (VBS vs. SGX) differ between Azure SQL Database and SQL Server. Verify before quoting.
