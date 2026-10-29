# 🌐 Open Cloud Agents: Pipelines

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![GitHub Action](https://img.shields.io/badge/GitHub%20Action-open--cloud--agents%2Fpipelines%40v1-blue.svg?logo=githubactions&logoColor=white)](action.yml)
[![Reusable Workflows](https://img.shields.io/badge/Reusable%20Workflows-.github%2Fworkflows-2088FF.svg?logo=githubactions&logoColor=white)](.github/workflows/)
[![Well-Architected](https://img.shields.io/badge/Guardrails-AWS%20%7C%20Azure%20%7C%20GCP-orange.svg)](https://github.com/open-cloud-agents/ocagents)

**Centralized Governance, Decentralized Execution.**

This repository hosts the official GitHub Actions, composite actions, and decentralized CI/CD pipelines for the **[Open Cloud Agents](https://github.com/open-cloud-agents)** ecosystem.

Any repository can adopt enterprise-grade **cloud architecture reviews, IaC state destruction prevention (`forces replacement`), ISO/IEC 42001 AI safety audits, and zero-trust secret leak detection** in just 2 lines of YAML.

---

## ⚡ Quickstart

You can invoke Open Cloud Agents either as a **Direct Composite Action (Step)** or as a **Full Multi-Job Workflow**:

### Method 1: Direct Action Step (Recommended for existing jobs)

Add this single step to any existing workflow job:

```yaml
steps:
  - name: Checkout Code
    uses: actions/checkout@v4

  - name: Run Cloud Guardrails
    uses: open-cloud-agents/pipelines@v1 # or open-cloud-agents/pipeline@v1
    with:
      enable-iac-audit: true
      enable-ai-safety: true
      enable-secret-scan: true
```

### Method 2: Full Reusable Workflow (Zero configuration)

Create `.github/workflows/guardrails.yml` in **any** repository:

```yaml
name: Cloud Guardrails

on: [push, pull_request]

jobs:
  audit:
    uses: open-cloud-agents/pipelines/.github/workflows/cloud-guardrails.yml@v1
    with:
      enable-secret-scan: true
      enable-iac-audit: true
      enable-ai-safety: true
```

---

## 🏛️ Decentralized Architecture

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        open-cloud-agents/pipelines (Central Governance)                 │
│  ├── action.yml                                   (Root Composite Action @v1)          │
│  ├── .github/workflows/cloud-guardrails.yml       (Unified 4-stage reusable workflow)  │
│  ├── .github/workflows/iac-security-audit.yml     (Forces replacement & perimeter)     │
│  ├── .github/workflows/ai-safety-governance.yml   (ISO 42001 & OWASP LLM prompt AST)  │
│  └── actions/setup-ocagents/action.yml            (Zero-friction CLI runner setup)     │
└───────────────────────────────────────────┬────────────────────────────────────────────┘
                                            │
               ┌────────────────────────────┼────────────────────────────┐
               ▼ (uses: @v1)                ▼ (uses: @v1)                ▼ (uses: @v1)
┌──────────────────────────────┐ ┌──────────────────────────────┐ ┌──────────────────────────────┐
│  open-cloud-agents           │ │  customer-terraform-repo     │ │  community-microservice      │
│  (Core Framework Repo)       │ │  (Production AWS / Azure)    │ │  (Kubernetes & AI Apps)      │
└──────────────────────────────┘ └──────────────────────────────┘ └──────────────────────────────┘
```

---

## 🛡️ What Gets Audited?

Every commit and pull request is automatically validated against:

1. 🔒 **Secret Leaks:** Scans for unredacted API tokens, private keys, and credentials via Gitleaks.
2. 💥 **State Destruction Traps:** Detects dangerous Terraform modifications (e.g. `cluster_identifier`) that force resource replacement and database destruction.
3. 🤖 **AI Safety & Lineage:** Verifies dataset cryptographic hash integrity (ISO 42001) and scans for prompt injection strings.
4. 🐚 **Shell Security:** Enforces POSIX/bash security standards via ShellCheck.

---

## 📦 Available Invocations

| Type | Target | Purpose | Invocation |
|---|---|---|---|
| **Root Composite Action** | [`action.yml`](action.yml) | Runs full audit suite as a single step inside any existing runner job. | `uses: open-cloud-agents/pipelines@v1` |
| **Reusable Workflow** | [`cloud-guardrails.yml`](.github/workflows/cloud-guardrails.yml) | Complete standalone 4-stage audit workflow across multiple jobs. | `uses: open-cloud-agents/pipelines/.github/workflows/cloud-guardrails.yml@v1` |
| **Reusable Workflow** | [`iac-security-audit.yml`](.github/workflows/iac-security-audit.yml) | Focused IaC & Terraform auditor for database wipes and perimeter leaks. | `uses: open-cloud-agents/pipelines/.github/workflows/iac-security-audit.yml@v1` |
| **Reusable Workflow** | [`ai-safety-governance.yml`](.github/workflows/ai-safety-governance.yml) | ISO/IEC 42001 dataset provenance and OWASP LLM prompt defenses. | `uses: open-cloud-agents/pipelines/.github/workflows/ai-safety-governance.yml@v1` |
| **Modular Action** | [`setup-ocagents`](actions/setup-ocagents/action.yml) | Installs `ocagents` standalone CLI on the runner. | `uses: open-cloud-agents/pipelines/actions/setup-ocagents@v1` |

---

## ⚙️ Configuration Reference

| Input | Type | Default | Description |
|---|---|---|---|
| `enable-secret-scan` | `boolean` | `true` | Executes Gitleaks credential leak detection across all commits. |
| `enable-iac-audit` | `boolean` | `true` | Scans Terraform, OpenTofu, and CloudFormation for state destruction traps. |
| `enable-ai-safety` | `boolean` | `true` | Audits dataset lineage (ISO 42001) and scans for prompt injection vectors. |
| `enable-shell-lint` | `boolean` | `true` | Lints shell scripts with ShellCheck for POSIX/bash safety. |

---

## 📄 License & Attribution

* Licensed under the [Apache License 2.0](LICENSE) (c) 2026 Ibrahim Jimoh.
* Maintained by **[Open Cloud Agents](https://github.com/open-cloud-agents)**.
* Co-authored-by: `Antigravity <antigravity@google.com>`.
