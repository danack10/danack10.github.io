---
title: "Cloud Native Architecture: Zero-Trust Bare Metal Kubernetes"
date: 2026-06-16
summary: "End-to-end automated provisioning and GitOps deployment of a secure Kubernetes (K3s) cluster from bare metal."
tags:
  - Platform Engineering
  - GitOps
  - Kubernetes
  - Infrastructure as Code
tech_stack:
  - OpenTofu
  - KVM (Ubuntu / Kali)
  - K3s
  - ArgoCD & Helm
  - HashiCorp Vault
  - Cert-Manager
  - Prometheus & Grafana
  - Renovate Bot
  - GitHub Actions (CI)
  - Tailscale & Ngrok
links:
  - type: github
    url: https://github.com/danack10/k3s-whatsapp-chatbot
    label: View The Entire Project
featured: true
share: false
status: "In Progress"
role: "DevSecOps & Platform Engineer"
duration: "Ongoing"
---

An ongoing, 10-phase infrastructure build demonstrating modern DevSecOps principles. This active laboratory project showcases how I transform bare-metal hardware into a highly available, self-healing, and secure Kubernetes environment using GitOps methodologies.

## Overview

I wanted to move beyond simple cloud provider tutorials and understand how the underlying compute layers actually work. I engineered this cluster to enforce zero-trust security and eliminate manual configuration drift, proving that enterprise-grade automation can be built from bare metal using virtualization, secure tunneling, and GitOps.

## Infrastructure Capabilities

### 1. Virtualized Compute & Zero-Trust Access
- **KVM Hypervisor** - Ubuntu guest virtual machine running efficiently on my bare-metal Kali Linux host.
- **Tailscale Mesh VPN** - Hardened, zero-trust SSH access tunnel completely isolating the host from the public internet.
- **Declarative Provisioning** - Utilizing OpenTofu to dynamically provision and manage the infrastructure state.

### 2. Cluster Orchestration & GitOps Automation
- **K3s Orchestration** - Lightweight, highly available Kubernetes deployment.
- **App-of-Apps Paradigm** - Bootstrapping the entire cluster automatically via a single ArgoCD root manifest, managing applications as code.
- **Dynamic Ingress & Isolated Edge** - Traefik ingress managing routing alongside standalone Cert-Manager TLS distribution, protected by an isolated Ngrok tunnel interface.

### 3. Supply Chain & CI/CD
- **Continuous Integration** - GitHub Actions automates the building and pushing of Docker images for immutable rollbacks.
- **Automated Lifecycle** - Renovate Bot continuously monitors dependencies, automatically merging minor patches while holding major releases for my approval.
- **Zero Manual Drift** - ArgoCD automatically detects and overwrites manual changes, enforcing strict GitOps compliance.

### 4. Secret Management & Telemetry
- **HashiCorp Vault** - Centralized, encrypted storage for sensitive credentials and API keys.
- **External Secrets Operator (ESO)** - Dynamically injects Vault secrets directly into Kubernetes pods.
- **Full-Stack Telemetry** - Dedicated Prometheus and Grafana instance processing cluster metrics.

## System Architecture

    ┌─────────────────┐    (Builds Docker Image)    ┌───────────────────────┐
    │ GitHub Actions  │────────────────────────────▶│  Container Registry   │
    │  (CI Pipeline)  │                             │     (GHCR / Hub)      │
    └─────────────────┘                             └───────────────────────┘
             │                                                  │
             │ (Updates Manifests)                              │ (Pulls Image)
             ▼                                                  ▼
    ┌──────────────┐     ┌───────────────┐     ┌───────────────────────┐
    │              │     │  ArgoCD Root  │     │ Kubernetes (K3s)      │
    │  Git Repo    │────▶│  Bootstrap    │────▶│ ┌───────────────────┐ │
    │ (Manifests)  │     │ (App-of-Apps) │     │ │  Traefik Ingress  │ │
    └──────────────┘     └───────────────┘     │ └───────────────────┘ │
                                               │ ┌───────────────────┐ │
    ┌──────────────┐     ┌───────────────┐     │ │ k3s-whatsapp-bot  │ │
    │              │     │   External    │     │ │ (n8n + Postgres)  │ │
    │  HashiCorp   │◀────│    Secrets    │◀────│ └───────────────────┘ │
    │    Vault     │     │   Operator    │     │ ┌───────────────────┐ │
    │              │     │               │     │ │Standalone Cert-Mgr│ │
    └──────────────┘     └───────────────┘     └───────────────────────┘
    [ Infrastructure: KVM Ubuntu Guest on Kali Metal | Secured via Tailscale / Ngrok ]

## Engineering Outcomes

- 🚀 **Full CI/CD**: 100% automated pipeline from code push (GitHub Actions) to cluster synchronization (ArgoCD via App-of-Apps).
- 🔒 **Security**: Host isolated via Tailscale; public endpoints limited via Ngrok; zero hardcoded secrets via Vault and ESO.
- 📉 **Configuration Drift**: Reduced to 0% through strict ArgoCD reconciliation loops.

## Technical Deep Dives (Architecture Series)

To explore the raw code, YAML manifests, and how I solved specific architectural challenges across the lifecycle, read my detailed engineering write-ups:

- 📝 **[Stage I: Architecting a Bare-Metal KVM & K3s Foundation](/blog/stage-1-compute-foundation/)** - *Deep dive into Phases 1-4: Virtualizing Ubuntu on Kali via KVM, Tailscale SSH tunnels, and OpenTofu provisioning.*
- 📝 **[Stage II: CI/CD Pipeline, GitOps & Ephemeral Secrets](/blog/stage-2-automation-zero-trust/)** - *Deep dive into Phases 5-6: Docker builds via GitHub Actions, Helm/ArgoCD drift elimination, and Vault/ESO injection.*
- 📝 **[Stage III: Cluster Bootstrapping, Public Edge & Infrastructure Observability](/blog/stage-3-perimeter-observability/)** - *Deep dive into Phases 7-8: ArgoCD App-of-Apps bootstrapping, standalone Cert-Manager TLS, and Prometheus/Grafana telemetry.*
- 📝 **[Stage IV: Purple Teaming Laboratory](/blog/stage-4-purple-teaming/)** - *Deep dive into Phases 9-10: Executing automated attack paths against the cluster and Building a SIEM alerting pipeline.*

## The 10-Phase Engineering Roadmap

This cluster is designed as my personal DevSecOps laboratory. I am currently executing Phase 9 of a comprehensive, capability-driven lifecycle.

**Stage I: The Compute Foundation (✅ Completed)**
- ✅ **Phase 1: Base Hypervisor** - Bare-metal Kali Linux hosting KVM.
- ✅ **Phase 2: Virtualization & Access** - OpenTofu provisioning of the Ubuntu guest and zero-trust Tailscale SSH tunneling.
- ✅ **Phase 3: Container Orchestration** - High-availability K3s cluster bootstrapping.
- ✅ **Phase 4: Edge Routing** - Dynamic ingress and load balancing via Traefik.

**Stage II: Automation & Zero-Trust (✅ Completed)**
- ✅ **Phase 5: CI/CD Pipeline** - GitHub Actions building Docker images and ArgoCD/Helm synchronizing the GitOps state.
- ✅ **Phase 6: Ephemeral Secrets** - Zero-trust credential injection via HashiCorp Vault and ESO.

**Stage III: Cluster Bootstrapping, Public Edge & Infrastructure Observability (✅ Completed)**
- ✅ **Phase 7: Structural Scaling** — Refactored the entire cluster deployment to utilize the ArgoCD App-of-Apps bootstrapping paradigm and managed a standalone Cert-Manager instance for dynamic TLS distribution.
- ✅ **Phase 8: Edge Control & Telemetry** — Enforced a strict zero-trust network perimeter by isolating administrative panels to local networks, exposing the core n8n engine webhooks securely via an Ngrok edge tunnel, and deploying the Prometheus & Grafana telemetry stack.

**Stage IV: Hardening, Shift-Left & Disaster Recovery (⏳ In Progress)**
- ⏳ **Phase 9: Shift-Left Security & Resiliency** — Integrating automated static application security testing (SAST) and container vulnerability scanning into the GitHub Actions CI pipeline, alongside deploying Velero for cluster-wide backup, restoration, and disaster recovery.
- ⏳ **Phase 10: Active Defense & Alerting Architecture** — Configuring Prometheus Alertmanager to catch infrastructure and security anomalies, and engineering automated response pathways to act as a production-grade incident detection system.