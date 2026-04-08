# k8s-gitops-config

This repository serves as the Single Source of Truth for the GitOps-driven (Flux CD) configuration of my Kubernetes infrastructure. By leveraging a dual-cluster architecture and automated reconciliation, this setup reduces deployment time by approximately 70% and eliminates manual configuration errors.


🏗 Architecture Overview

The infrastructure is split into two distinct environments to ensure high security and operational isolation:

1. Management Cluster, k3d/k3s (Control Plane)

The "Brain" of the infrastructure, responsible for secrets and cross-cluster resource provisioning.

    HashiCorp Vault: Centralized secret management. Injected into workloads via Vault Agent Injector/ESO, ensuring zero secrets are stored in Git.

    Tofu Controller: Automates infrastructure-as-code (IaC) using OpenTofu. It reconciles Terraform/Tofu modules directly from Git into the cluster state.

2. Main Cluster, KVM/k8s (Workload Plane)

The production-like environment where applications run, provisioned via a separate bare-metal/libvirt automation repository (https://github.com/winterlyembrace/k8s-infrastructure).

    Cilium CNI: Advanced eBPF-based networking.

        L2 Announcement Policies: Enables bare-metal LoadBalancer support without BGP.

        CiliumLoadBalancerIPPool: Management of dedicated VIP ranges for external access.

    Application Stack: * Frontend: React (Vite) optimized for containerized environments.

        Backend: FastAPI (Python) high-performance API.

        Kubernetes Visualizer: A custom-built tool for real-time cluster monitoring.

 
🚀 Key Features

  Declarative Networking: Cilium L2 announcements are managed as code, allowing instant provisioning of LoadBalancer IPs in a local network.

  Automated IaC: Tofu Controller ensures that any change to the infrastructure modules is automatically applied, preventing configuration drift.

  Rapid Scaling: New services are deployed with a single git push, including networking and secret injection, cutting the "idea-to-production" time significantly.


📈 Impact

By moving from manual kubectl applications and static secret management to this GitOps flow:

    Deployment Speed: Increased by ~70%.

    Reliability: 100% reproducible environments through declarative manifests.

