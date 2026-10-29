# 🌐 Open Cloud Agents: Decentralized Pipelines

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Reusable Workflows](https://img.shields.io/badge/GitHub%20Actions-Reusable%20Workflows-2088FF.svg?logo=githubactions&logoColor=white)](.github/workflows/)
[![Well-Architected](https://img.shields.io/badge/Guardrails-AWS%20%7C%20Azure%20%7C%20GCP-orange.svg)](https://github.com/open-cloud-agents/open-cloud-agents)

**Centralized Governance, Decentralized Execution.**

This repository hosts the official reusable GitHub Actions workflows, composite actions, and CI/CD security pipelines for the **[Open Cloud Agents](https://github.com/open-cloud-agents/open-cloud-agents)** ecosystem.

Any repository can adopt enterprise-grade **cloud architecture reviews, IaC state destruction prevention (`forces replacement`), ISO/IEC 42001 AI safety audits, and zero-trust secret leak detection** in just 4 lines of YAML.

---

## 🏛️ Decentralized Pipeline Architecture

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        open-cloud-agents/pipelines (Central Governance)                 │
│  ├── .github/workflows/cloud-guardrails.yml       (Unified 4-stage audit)              │
│  ├── .github/workflows/iac-security-audit.yml     (Forces replacement & perimeter)     │
│  ├── .github/workflows/ai-safety-governance.yml   (ISO 42001 & OWASP LLM prompt AST)  │
│  └── actions/setup-ocagents/action.yml            (Zero-friction CLI runner setup)     │
└───────────────────────────────────────────┬────────────────────────────────────────────┘
                                            │
               ┌────────────────────────────┼────────────────────────────┐
               ▼ (uses: @main)              ▼ (uses: @main)              ▼ (uses: @main)
┌──────────────────────────────┐ ┌──────────────────────────────┐ ┌──────────────────────────────┐
│  open-cloud-agents           │ │  customer-terraform-repo     │ │  community-microservice      │
│  (Core Framework Repo)       │ │  (Production AWS / Azure)    │ │  (Kubernetes & AI Apps)      │
└──────────────────────────────┘ └──────────────────────────────┘ └──────────────────────────────┘
```

---

## 🚀 4-Line Quickstart

Add this file to `.github/workflows/guardrails.yml` in **any** repository:

```yaml
name: Cloud Guardrails

on: [push, pull_request]

jobs:
  audit:
    uses: open-cloud-agents/pipelines/.github/workflows/cloud-guardrails.yml@main
    with:
      enable-secret-scan: true
      enable-iac-audit: true
      enable-ai-safety: true
```

That's it! Every commit and pull request will now automatically be audited for:
1. 🔒 **Secret Leaks:** Scans for unredacted API tokens, private keys, and credentials via Gitleaks.
2. 💥 **State Destruction Traps:** Detects dangerous Terraform modifications (e.g. `cluster_identifier`) that force resource replacement and data loss.
3. 🤖 **AI Safety & Lineage:** Verifies dataset cryptographic hash integrity (ISO 42001) and scans for prompt injection strings.
4. 🐚 **Shell Security:** Enforces POSIX/bash security standards via ShellCheck.

---

## 📦 Available Reusable Workflows

| Reusable Workflow | Purpose | Invocation |
|---|---|---|
| [`cloud-guardrails.yml`](.github/workflows/cloud-guardrails.yml) | **Unified 4-Stage Security Audit:** Secrets, IaC destruction traps, AI safety, and shell linting. | `uses: open-cloud-agents/pipelines/.github/workflows/cloud-guardrails.yml@main` |
| [`iac-security-audit.yml`](.github/workflows/iac-security-audit.yml) | **IaC & Terraform Auditor:** Blocks silent `forces replacement` database wipes and `0.0.0.0/0` perimeter exposures. | `uses: open-cloud-agents/pipelines/.github/workflows/iac-security-audit.yml@main` |
| [`ai-safety-governance.yml`](.github/workflows/ai-safety-governance.yml) | **AI Safety & Lineage:** Audits ISO/IEC 42001 dataset provenance and OWASP Top 10 for LLMs prompt defenses. | `uses: open-cloud-agents/pipelines/.github/workflows/ai-safety-governance.yml@main` |

---

## 🧰 Available Composite Actions

You can also use modular composite actions inside your existing custom workflow jobs:

### 1. Setup `ocagents` CLI (`actions/setup-ocagents`)

Installs the standalone Open Cloud Agents CLI directly in your GitHub Actions runner:

```yaml
steps:
  - name: Install ocagents CLI
    uses: open-cloud-agents/pipelines/actions/setup-ocagents@main

  - name: List Available Specialist Agents
    run: ocagents -agents list
```

---

## ⚙️ Configuration Reference (`cloud-guardrails.yml`)

| Input | Type | Default | Description |
|---|---|---|---|
| `enable-secret-scan` | `boolean` | `true` | Executes Gitleaks credential leak detection across all commits. |
| `enable-iac-audit` | `boolean` | `true` | Scans Terraform, OpenTofu, and CloudFormation for state destruction traps. |
| `enable-ai-safety` | `boolean` | `true` | Audits dataset lineage (ISO 42001) and scans for prompt injection vectors. |
| `enable-shell-lint` | `boolean` | `true` | Lints shell scripts with ShellCheck for POSIX/bash safety. |
| `python-version` | `string` | `"3.11"` | Python runtime version used for validation tools. |
| `fail-on-warning` | `boolean` | `false` | When `true`, non-critical warnings block the pipeline. |

---

## 📄 License & Attribution

* Licensed under the [Apache License 2.0](LICENSE) (c) 2026 Ibrahim Jimoh.
* Maintained by **[Open Cloud Agents](https://github.com/open-cloud-agents)**.
* Co-authored-by: `Antigravity <antigravity@google.com>`.
