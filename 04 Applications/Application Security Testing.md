---
tags:
  - sc100
type: concept
domain:
  - applications
aliases:
  - SAST
  - DAST
  - IAST
  - RASP
  - Fuzzing
  - Fuzz Testing
  - Security Regression Testing
---

# Application Security Testing

## Purpose

Choosing the right testing technique (**SAST, DAST, IAST, RASP, fuzzing, regression testing**) to catch code vulnerabilities before release, while keeping testing overhead low.

---

## Why architects choose it

- Each technique finds a different class of bug at a different SDLC stage, so the choice depends on the requirement wording ("undetected", "minimize overhead", "after a change").
- **Automated** techniques (fuzzing, regression suites) run on every build at low marginal cost. Manual-heavy techniques (manual code review, IAST combos) add overhead.
- Fits the shift-left model in [[DevOps Security]] and [[Shift left (WAF)]].

---

## When to use

- **Fuzzing:** automatically send random/malformed input to find unhandled-input bugs (buffer overflows, injection, crashes). Example tool: Microsoft **OneFuzz** (GitHub project).
- **Security regression testing:** after **any** change (release, patch, upgrade), confirm no new vulnerabilities were introduced. It often includes automated fuzzing.
- **SAST:** analyse source code without running it (CodeQL), early in CI.
- **DAST:** attack a running app from outside (black box), in test/staging.
- **IAST:** instrument the running app during testing (agent inside + test traffic), combining static and dynamic insight.
- **RASP:** runtime protection inside the production app that detects/blocks attacks live.

---

## When NOT to use

- SAST or IAST when the requirement says **"minimize testing overhead"** in this question pattern (they are framed as time-consuming or manual-heavy).
- CI/CD as a **testing** answer. CI/CD is a delivery mechanism (Azure Pipelines already provides it) and doesn't find vulnerabilities by itself.
- RASP as a pre-release testing answer, since it is runtime protection.

---

## Comparison

| Technique | Runs against | Box | Stage | Finds | Overhead |
| --- | --- | --- | --- | --- | --- |
| **SAST** | Source code (not running) | White | Code/build | Pattern flaws (injection, unsafe APIs) | Can be high (manual review/triage) |
| **DAST** | Running app, from outside | Black | Test/staging | Runtime/config flaws, exposed endpoints | Medium |
| **IAST** | Running app + internal agent | Grey | Test/QA | Precise flaws with code location | Medium–high |
| **RASP** | Production app (in-process) | — | Runtime | Blocks live attacks | Runtime perf cost |
| **Fuzzing** | Inputs to running code | Black/grey | Build/test (automated) | Crashes, buffer overflows, input-handling bugs | **Low** (automated) |
| **Security regression testing** | Changed code | — | Every change | Re-introduced or newly introduced vulns | **Low** when automated |
| **SCA** | Third-party dependencies | — | Build | Known CVEs in packages | Low |

---

## Exam Scenario

**Scenario:** A containerized app on [[Container and Kubernetes Security|AKS]] is deployed via Azure Pipelines. A rushed fix had code errors that opened a **code injection** vulnerability, and it was exploited, causing an outage. The task is to improve and secure DevOps **while minimizing testing overhead**. Pick two that mitigate undetected code vulnerabilities.

| Option | Verdict | Why |
| --- | --- | --- |
| **Add fuzzing to the pipeline** | ✅ | Automated malformed-input testing finds injection/overflow bugs |
| **Implement regression testing** | ✅ | Every change (like the fix) is re-tested so new vulnerabilities aren't introduced |
| Distribute source code and use SAST | ❌ | Time-consuming; doesn't minimize overhead |
| IAST before release | ❌ | Combines automated and manual testing; doesn't minimize overhead |
| Implement CI/CD | ❌ | Already have it (Azure Pipelines); delivery, not testing |

Pattern: **"fix introduced a new vuln"** → regression testing. **"malformed input/injection, automated"** → fuzzing.

---

## Common Exam Confusion

- **SAST vs. DAST** — inside-out on code at rest vs. outside-in on a running app.
- **DAST vs. IAST** — external black box vs. an agent inside the running app.
- **IAST vs. RASP** — testing-time instrumentation vs. production-time protection.
- **SAST vs. SCA** — your code vs. third-party packages (see [[DevOps Security]]).
- **Regression testing vs. fuzzing** — "did the change break or reopen something?" vs. "what does random input break?" They complement each other; regression suites often include fuzzing.
- **CI/CD vs. security testing** — pipeline vs. the checks you put in the pipeline.

---

## Exam Tips

- "Minimize testing overhead" → favour **automated** techniques: fuzzing and regression tests.
- "Code injection / buffer overflow / malformed input" → fuzzing.
- "Ensure a patch/upgrade didn't introduce vulnerabilities" → security regression testing.
- "Runtime protection in production" → RASP, not IAST.

---

## AZ-500 overlap

- AZ-500 doesn't cover application security testing techniques.
- Related known items: Defender for Cloud DevOps security, container image scanning.

---

## What's new in SC-100

- Choosing testing techniques by SDLC stage, vulnerability class and overhead constraint.
- Embedding automated security tests in CI/CD as a design requirement ([[Threat Modeling]] → test → deploy).

---

## Keywords

- SAST, DAST, IAST, RASP, SCA
- Fuzzing, fuzz testing, malformed/random input, OneFuzz
- Security regression testing, patch/upgrade/new release
- Code injection, buffer overflow
- Minimize testing overhead, automated testing
- Azure Pipelines, CI/CD

---

## Related Services

- [[DevOps Security]]
- [[Shift left (WAF)]]
- [[Threat Modeling]]
- [[Container and Kubernetes Security]]
- [[API Management and Security]]
- [[Microsoft Defender for Cloud]]
- [[Azure Web Application Firewall]]
- [[Full Lifecycle Application Security Strategy]] — where SAST/DAST fit in the SDLC

---

## References

- [A brief introduction to fuzzing and why it's an important tool for developers](https://www.microsoft.com/en-us/research/blog/a-brief-introduction-to-fuzzing-and-why-its-an-important-tool-for-developers/) — Microsoft Research
- [What is Azure Pipelines?](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines) — Microsoft Learn
- [[Exam Objectives]]
