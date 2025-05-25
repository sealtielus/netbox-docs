
# NetBox IPAM Deployment Guidelines

**Status:** IN PROGRESS **FOR REVIEW**  
**Owner:** Nucleus Tribe  
**Contributors:** @Pom Parungao, @Lorenzo Nino Buitizon, @Shella Mae Baltazar  
**Approvers:** @Aldrin Advincula, @GERALD EARL GERONIMO, @Adrienne Joy Gumobao  
**Due Date:** May 27, 2025  

---

## 1. Overview

This document outlines guidelines for deploying NetBox as a centralized IP Address Management (IPAM) solution across hybrid infrastructure environments including on-prem, AWS, and GCP. The design supports infrastructure as code, automation, and visibility.

---

## 2. Objectives

- ✅ Centralize IP Address Management (IPAM)
- ✅ Model Multi-Cloud Network Topology
- ✅ Enable Infrastructure as Code (IaC)
- ✅ Improve Visibility and Auditability
- ✅ Lay the Foundation for Automation

---

## 3. Architecture Overview

### 3.1 Component Layout

- NetBox container (API + UI)
- PostgreSQL database
- Redis (for caching & tasks)
- Gunicorn + NGINX reverse proxy
- Optional integrations: LDAP, logging, metrics

### 3.2 Deployment Model

- Based on OCI containers
- Stored in JFrog Artifactory
- Deployed via Helm Charts
- Managed by GitOps (ArgoCD or Flux)

📎 [NetBox Helm Chart](https://github.com/netbox-community/netbox-chart)  
📎 [NetBox Docker Image](https://github.com/netbox-community/netbox-docker)

---

## 4. Design Guidelines

### 4.1 Regions and Sites

| Environment | Region        | Site                |
|-------------|---------------|---------------------|
| On-Prem     | `global`      | `on-prem-dc1`       |
| AWS         | `aws`         | `aws-us-east-1`     |
| GCP         | `gcp`         | `gcp-europe-west1`  |

- ☐ Define and lock standard values for Region and Site fields

---

### 4.2 Tenants

- Option 1: Use Tenant for Cloud Provider (e.g., `AWS`, `GCP`)
- Option 2: Use Tenant for Platform Type (e.g., `Kubernetes`, `VMs`)
- ☐ Evaluate and finalize Tenant usage strategy

---

### 4.3 Prefix Roles

| Prefix Type      | Role      | Example         |
|------------------|-----------|-----------------|
| AWS VPC CIDR     | `vpc`     | 10.100.0.0/16   |
| GCP Pod Range    | `k8s-pod` | 10.200.0.0/14   |
| On-prem subnet   | `mgmt`    | 192.168.10.0/24 |

- ☐ Define roles per cloud/platform
- ☐ Document role naming convention

---

### 4.4 Tags

| Tag Type | Example           |
|----------|-------------------|
| Env      | `env:prod`        |
| Cloud    | `cloud:aws`       |
| App      | `app:web`         |
| Team     | `team:infra`      |
| Platform | `platform:k8s`    |

- ☐ Define and document custom tags

---

## 5. Automation Strategy

- Use Terraform to push NetBox definitions
- Pull data via:
  - `boto3` (AWS)
  - `gcloud` CLI (GCP)
  - `kubectl` / `kube-state-metrics` (K8s)

- ☐ Build GitLab CI job to sync resources regularly

---

## 6. Access Control

- ☐ Apply NetBox RBAC / LDAP groups
- ☐ Track audit logs
- ☐ Define access scope per team

---

## 7. Documentation & Governance

- ☐ Write README.md for all naming/tagging conventions
- ☐ Store automation code in Git
- ☐ Maintain version-controlled backup plans

---

## 8. References

📎 [NetBox GitHub](https://github.com/netbox-community/netbox?tab=readme-ov-file#netboxs-role)  
📎 [NetBox Terraform Provider](https://github.com/netbox-community/terraform-provider-netbox)

---

## 9. Timeline (Sample)

| Phase                  | Date         | Notes                           |
|------------------------|--------------|---------------------------------|
| Design Finalization    | May 27, 2025 | Approve guidelines              |
| CI/CD Sync Setup       | June 3, 2025 | GitLab job + terraform apply    |
| Data Import/Migration  | June 10, 2025| Import legacy IP data           |
| Production Rollout     | June 24, 2025| Full NetBox Go-Live             |
