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
  - KVM (Ubuntu / Kali)
  - K3s
  - ArgoCD & Helm
  - HashiCorp Vault
  - GitHub Actions (CI)
  - Tailscale
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

I wanted to move beyond simple cloud provider tutorials and understand how the underlying compute layers actually work. I engineered this cluster to enforce zero-trust security and eliminate manual configuration drift, proving that enterprise-grade automation can be built from bare metal using virtualization, secure tunneling, and GitOps.

## Infrastructure Capabilities

### 1. Virtualized Compute & Zero-Trust Access
- **KVM Hypervisor** - Ubuntu guest virtual machine running efficiently on a bare-metal Kali Linux host.
- **Tailscale Mesh VPN** - Hardened, zero-trust SSH access tunnel completely isolating the host from the public internet.
- **Declarative Provisioning** - Utilizing OpenTofu to dynamically provision and manage the infrastructure state.

### 2. Container Orchestration & Networking
- **K3s Orchestration** - Lightweight, highly available Kubernetes deployment.
- **Dynamic Ingress** - Traefik configured as the primary ingress controller to manage robust routing and load balancing.

### 3. CI/CD & GitOps
- **Continuous Integration** - GitHub Actions automates the building and pushing of Docker images for immutable rollbacks.
- **ArgoCD Synchronization** - Cluster state is bound directly to the Git repository using Helm and Kustomize.
- **Zero Manual Drift** - ArgoCD automatically detects and overwrites any manual changes, enforcing strict GitOps compliance.

### 4. Secret Management
- **HashiCorp Vault** - Centralized, encrypted storage for all sensitive credentials and API keys.
- **External Secrets Operator (ESO)** - Dynamically injects Vault secrets directly into Kubernetes pods.

## System Architecture

    ┌─────────────────┐    (Builds Docker Image)    ┌───────────────────────┐
    │ GitHub Actions  │────────────────────────────▶│  Container Registry   │
    │  (CI Pipeline)  │                             │     (GHCR / Hub)      │
    └─────────────────┘                             └───────────────────────┘
             │                                                  │
             │ (Updates Manifests)                              │ (Pulls Image)
             ▼                                                  ▼
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
    [ Infrastructure: KVM Ubuntu Guest on Kali Metal | Secured via Tailscale ]

## Engineering Outcomes

- 🚀 **Full CI/CD**: 100% automated pipeline from code push (GitHub Actions) to cluster synchronization (ArgoCD).
- 🔒 **Security**: Host isolated via Tailscale; zero hardcoded secrets via Vault and ESO.
- 📉 **Configuration Drift**: Reduced to 0% through strict ArgoCD reconciliation loops.

## Technical Deep Dives (Architecture Series)

To explore the raw code, YAML manifests, and how I solved specific architectural challenges across the lifecycle, read my detailed engineering write-ups:

- 📝 **[Stage I: Architecting a Bare-Metal KVM & K3s Foundation](/blog/stage-1-compute-foundation/)** - *Deep dive into Phases 1-4: Virtualizing Ubuntu on Kali via KVM, Tailscale SSH tunnels, and OpenTofu provisioning.*
- 📝 **[Stage II: CI/CD Pipeline, GitOps & Ephemeral Secrets](/blog/stage-2-automation-zero-trust/)** - *Deep dive into Phases 5-6: Docker builds via GitHub Actions, Helm/ArgoCD drift elimination, and Vault/ESO injection.*
- 📝 **[Stage III: Perimeter Defense & Observability](/blog/stage-3-perimeter-observability/)** (Coming Soon) - *Deep dive into Phases 7-8: Cloudflare Tunnels and Prometheus/Grafana telemetry.*
- 📝 **[Stage IV: Purple Teaming Laboratory](/blog/stage-4-purple-teaming/)** (Coming Soon) - *Deep dive into Phases 9-10: Executing automated attack paths against the cluster and Building a SIEM alerting pipeline.*

## The 10-Phase Engineering Roadmap

This cluster is designed as a living DevSecOps laboratory. I am currently executing Phase 7 of a comprehensive, capability-driven lifecycle.

**Stage I: The Compute Foundation (✅ Completed)**
- ✅ **Phase 1: Base Hypervisor** - Bare-metal Kali Linux hosting KVM.
- ✅ **Phase 2: Virtualization & Access** - OpenTofu provisioning of the Ubuntu guest and zero-trust Tailscale SSH tunneling.
- ✅ **Phase 3: Container Orchestration** - High-availability K3s cluster bootstrapping.
- ✅ **Phase 4: Edge Routing** - Dynamic ingress and load balancing via Traefik.

**Stage II: Automation & Zero-Trust (✅ Completed)**
- ✅ **Phase 5: CI/CD Pipeline** - GitHub Actions building Docker images and ArgoCD/Helm synchronizing the GitOps state.
- ✅ **Phase 6: Ephemeral Secrets** - Zero-trust credential injection via HashiCorp Vault and ESO.

**Stage III: Perimeter Defense & Observability (⏳ In Progress)**
- ⏳ **Phase 7: Zero-Trust Perimeter** - Integrating Cloudflare Tunnels for unexposed, secure ingress.
- ⏳ **Phase 8: Full-Stack Telemetry** - Deploying Prometheus & Grafana for cluster observability.

**Stage IV: Purple Teaming Laboratory (📅 Planned)**
- 📅 **Phase 9: Offensive Simulation (Red)** - Executing automated attack paths against the cluster to validate resilience.
- 📅 **Phase 10: Threat Detection (Blue)** - Building a SIEM alerting pipeline to capture the offensive testing telemetry.