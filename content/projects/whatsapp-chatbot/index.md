---
title: "Cloud Native Architecture: Zero-Trust Bare Metal Kubernetes"
date: 2026-01-28
summary: "End-to-end automated provisioning and GitOps deployment of a secure Kubernetes (K3s) cluster from bare metal."
tags:
  - Platform Engineering
  - GitOps
  - Kubernetes
  - Infrastructure as Code
tech_stack:
  - OpenTofu
  - K3s
  - ArgoCD
  - HashiCorp Vault
  - Traefik
  - Kali Linux
links:
  - type: github
    url: https://github.com/danack10/k3s-whatsapp-chatbot
    label: View The Entire Project
featured: true
share: false
status: "In Progress"
role: "Platform Engineer"
duration: "Ongoing"
---

An ongoing, 10-phase infrastructure build demonstrating modern DevSecOps principles. This active laboratory project is transforming bare-metal hardware into a highly available, self-healing, and secure Kubernetes environment using GitOps methodologies.

## Overview

I wanted to move beyond simple cloud provider tutorials and understand how the underlying compute layers actually work. I engineered this cluster to enforce zero-trust security and eliminate manual configuration drift, proving that enterprise-grade automation can be built from bare metal.

## Infrastructure Capabilities

### 1. Infrastructure as Code (IaC)
- **Declarative Provisioning** - Utilizing OpenTofu (Terraform) to dynamically provision and manage virtual infrastructure.
- **Reproducible Environments** - Infrastructure state is strictly maintained, allowing the entire cluster to be torn down and rebuilt predictably.

### 2. Container Orchestration & Networking
- **Bare-Metal K3s** - Lightweight, highly available Kubernetes deployment natively on Kali Linux.
- **Dynamic Ingress** - Traefik configured as the primary ingress controller to manage robust routing and load balancing for all internal microservices.

### 3. Continuous Delivery (GitOps)
- **ArgoCD Synchronization** - Cluster state is bound directly to the Git repository.
- **Zero Manual Drift** - ArgoCD automatically detects and overwrites any manual changes made via `kubectl`, enforcing strict GitOps compliance.

### 4. Secret Management
- **HashiCorp Vault** - Centralized, encrypted storage for all sensitive credentials and API keys.
- **External Secrets Operator (ESO)** - Dynamically injects Vault secrets directly into Kubernetes pods, preventing hardcoded credentials in deployment manifests.

## System Architecture

    ┌──────────────┐     ┌───────────────┐     ┌───────────────────────┐
    │              │     │               │     │ Kubernetes (K3s)      │
    │  Git Repo    │────▶│    ArgoCD     │────▶│ ┌───────────────────┐ │
    │ (Manifests)  │     │  (Controller) │     │ │  Traefik Ingress  │ │
    │              │     │               │     │ └───────────────────┘ │
    └──────────────┘     └───────────────┘     │ ┌───────────────────┐ │
                                               │ │ k3s-whatsapp-bot  │ │
    ┌──────────────┐     ┌───────────────┐     │ │ (n8n + Postgres)  │ │
    │              │     │   External    │     │ └───────────────────┘ │
    │  HashiCorp   │◀────│    Secrets    │◀────│ ┌───────────────────┐ │
    │    Vault     │     │   Operator    │     │ │   ESO Injector    │ │
    │              │     │               │     │ └───────────────────┘ │
    └──────────────┘     └───────────────┘     └───────────────────────┘

## Engineering Outcomes

- 🚀 **Automation**: 100% automated deployment pipeline from code push to cluster synchronization.
- 🔒 **Security**: Zero hardcoded secrets in the repository; fully ephemeral credential injection using Vault and ESO.
- 📉 **Configuration Drift**: Reduced to 0% through strict ArgoCD reconciliation loops.

## The 10-Phase Engineering Roadmap

This cluster is designed as a living DevSecOps laboratory. I am currently executing Phase 7 of a comprehensive 10-phase build.

**Phase 1-4: The Foundation (✅ Completed)**
- [x] Bare-metal provisioning natively on Kali Linux.
- [x] Infrastructure as Code (IaC) deployment via OpenTofu.
- [x] Kubernetes (K3s) orchestration and Traefik ingress routing.

**Phase 5-6: Automation & Secrets (✅ Completed)**
- [x] Continuous Delivery pipeline established via ArgoCD (GitOps).
- [x] Zero-trust secret injection via HashiCorp Vault and External Secrets Operator.

**Phase 7-8: Edge Security & Observability (⏳ In Progress)**
- [ ] Integrating Cloudflare Tunnels for secure, unexposed ingress.
- [ ] Deploying Prometheus & Grafana for cluster observability and metric tracking.

**Phase 9-10: Purple Teaming & Detection (📅 Planned)**
- [ ] **Offensive Simulation**: Executing automated attack paths against the cluster to test resilience.
- [ ] **Detection Lab**: Building a SIEM pipeline to capture and alert on the offensive testing telemetry.
