
# 📘 NetBox IPAM Deployment Guidelines

This document outlines guidelines for deploying NetBox as a centralized IP Address Management (IPAM) solution across hybrid infrastructure environments including on-prem, AWS, and GCP. The design supports infrastructure as code, automation, and visibility.

---

## ✅ Objectives

- Centralize IP Address Management (IPAM)
- Model Multi-Cloud Network Topology
- Enable Infrastructure as Code (IaC)
- Improve Visibility and Auditability
- Lay the Foundation for Automation

---

## 🧱 Architecture Overview

### Component Layout

- NetBox container (API + UI)
- PostgreSQL database
- Redis (for caching & tasks)
- Gunicorn + NGINX reverse proxy
- Optional integrations: LDAP, logging, metrics

### Deployment Model

- Based on OCI containers
- Stored in JFrog Artifactory
- Deployed via Helm Charts
- Managed by GitOps (ArgoCD or Flux)

📎 [NetBox Helm Chart](https://github.com/netbox-community/netbox-chart)  
📎 [NetBox Docker Image](https://github.com/netbox-community/netbox-docker)

---

## 🧭 Design Guidelines

### Regions and Sites

| Environment | Region        | Site                |
|-------------|---------------|---------------------|
| On-Prem     | `global`      | `on-prem-dc1`       |
| AWS         | `aws`         | `aws-us-east-1`     |
| GCP         | `gcp`         | `gcp-europe-west1`  |

### Tenants

Use for:
- Cloud Providers (e.g., AWS, GCP)
- OR Platform Types (e.g., Kubernetes, VMs)

### Prefix Roles

| Type            | Role      | Example         |
|-----------------|-----------|-----------------|
| AWS VPC CIDR    | `vpc`     | 10.100.0.0/16   |
| GCP Pod Range   | `k8s-pod` | 10.200.0.0/14   |
| On-prem subnet  | `mgmt`    | 192.168.10.0/24 |

### Tags

| Tag Type | Example           |
|----------|-------------------|
| Env      | `env:prod`        |
| Cloud    | `cloud:aws`       |
| App      | `app:web`         |
| Team     | `team:infra`      |
| Platform | `platform:k8s`    |

---

## 🔄 Automation Strategy

Use Terraform and GitLab CI to automate NetBox updates:

- `boto3`, `gcloud`, `kubectl`, `kube-state-metrics`
- Push infrastructure changes to NetBox via CI

---

## 🔐 Access Control

- Use RBAC or LDAP
- Track changes via audit logs
- Limit editing scope by team

---

## 📝 Governance

- Store tagging/naming rules in README or NetBox Notes
- Version-control automation scripts
- Plan regular sync jobs and backups

---

## 📎 References

- [NetBox GitHub](https://github.com/netbox-community/netbox?tab=readme-ov-file#netboxs-role)
- [Terraform Provider](https://github.com/netbox-community/terraform-provider-netbox)
