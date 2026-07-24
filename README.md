# 📋 MuCi Proposal — Winning IT Services Bid for Paraguay's National Science Museum

> **Status:** ✅ **AWARDED** — 6-year contract (2024-2030)  
> **Scope:** Full managed IT for Museo de Ciencias (MuCi) — Planetarium, exhibitions, labs, admin, 24/7 operations  
> **Value:** ~US$2.8M over 6 years  
> **Team:** 5 FTE (CTO, SysAdmin, DevOps, Support L1/L2, Security)

## Executive Summary

MINIX won the competitive public tender (Licitación Pública Nº 12/2023) to provide comprehensive managed IT services for **MuCi — Museo de Ciencias del Paraguay**, including the planetarium, interactive exhibitions, research labs, and administrative infrastructure.

**Key differentiators that won the bid:**
- **ITIL 4 aligned** service management with documented SLAs
- **BCP/DRP** with RPO 1h / RTO 2h for planetarium (zero-show tolerance)
- **24/7 NOC** with on-site guard + remote escalation
- **Local presence** in Paraguay with 10-site network expertise (Lacerie)
- **Open standards** — no vendor lock-in, all configs in GitOps

---

## Service Scope (6-Year Contract)

| Domain | Services | SLA Tier |
|--------|----------|----------|
| **Planetarium** | Digistar 7, projection, audio, show control, UPS | **Crítica** (10min/1h) |
| **Exhibitions** | Interactive kiosks, AR/VR, sensors, lighting control | **Alta** (30min/4h) |
| **Research Labs** | HPC cluster, data acquisition, LIMS, backups | **Media** (2h/8h) |
| **Administration** | ERP (Odoo), email, docs, VPN, WiFi, VoIP | **Media** (2h/8h) |
| **Security** | CCTV, access control, firewall, SIEM, vuln mgmt | **Crítica** (15min/2h) |
| **Infrastructure** | Servers, storage, network, cooling, power, BCP/DRP | **Crítica** (10min/1h) |

---

## SLA Matrix (Contractual)

| Priority | Response | Resolution | Availability | Penalty |
|----------|----------|------------|--------------|---------|
| **Crítica** (P1) | 10 min | 1 hour | 99.9% | 5% monthly fee / incident |
| **Alta** (P2) | 30 min | 4 hours | 99.5% | 2% monthly fee / incident |
| **Media** (P3) | 2 hours | 8 hours | 99.0% | 1% monthly fee / incident |
| **Baja** (P4) | 8 hours | 5 business days | Best effort | None |

**Planetarium show downtime = P1 automatically** — zero tolerance during public hours.

---

## Team Structure (5 FTE On-Site/Remote)

| Role | FTE | Location | Responsibilities |
|------|-----|----------|------------------|
| **CTO / Service Manager** | 1.0 | Hybrid (Asunción) | Strategy, vendor mgmt, SLA governance, budget |
| **Senior SysAdmin** | 1.0 | On-site (MuCi) | Planetarium, servers, storage, backups, DR |
| **DevOps Engineer** | 1.0 | Remote (Asunción) | GitOps, CI/CD, monitoring, automation, cloud |
| **Support L1/L2** | 1.5 | On-site (MuCi) | Helpdesk, exhibitions, AV, user support |
| **Security Analyst** | 0.5 | Remote | SIEM, vuln scanning, hardening, compliance |

**Guardia 24/7:** On-site security guard (outsourced) with runbook for first response + NOC escalation.

---

## Technical Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    MuCi CAMPUS (San Lorenzo)                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │Planetarium│  │Exhibitions│  │  Labs    │  │  Admin   │    │
│  │Digistar 7 │  │Kiosks/AR  │  │HPC/NAS   │  │Odoo/ERP  │    │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘    │
│       │             │             │             │           │
│       └─────────────┴──────┬──────┴─────────────┘           │
│                            ▼                                 │
│                   ┌─────────────────┐                        │
│                   │  Core Switch    │  (CRS354-48G-4S+2Q+)   │
│                   │  10G backbone   │                        │
│                   └────────┬────────┘                        │
│                            │                                 │
│              ┌─────────────┼─────────────┐                   │
│              ▼             ▼             ▼                   │
│        ┌──────────┐ ┌──────────┐ ┌──────────┐               │
│        │ Firewall │ │  NAS/SAN │ │  Hyper-V │               │
│        │ (OPNsense)│ │ (TrueNAS)│ │ Cluster  │               │
│        └────┬─────┘ └────┬─────┘ └────┬─────┘               │
│             │            │           │                      │
│             └────────────┴───────────┘                      │
│                            │                                 │
│                   ┌────────▼────────┐                        │
│                   │  Lacerie WAN    │  (SSTP/GRE to HQ)      │
│                   │  100.126.5.100  │                        │
│                   └────────┬────────┘                        │
└────────────────────────────┼────────────────────────────────┘
                             │
                    ┌────────▼────────┐
                    │   MINIX HQ      │
                    │  (Asunción)     │
                    └─────────────────┘
```

---

## BCP/DRP Highlights

| Metric | Target | Implementation |
|--------|--------|----------------|
| **RPO (Planetarium)** | 1 hour | Continuous replication to HQ |
| **RTO (Planetarium)** | 2 hours | Cold spare + automated failover |
| **RPO (Admin/Labs)** | 4 hours | Hourly snapshots (TrueNAS) |
| **RTO (Admin/Labs)** | 8 hours | Restore from HQ replica |
| **Config Backup** | Daily | GitOps (`.rsc` + `.backup` to OMV) |
| **Failover Test** | Quarterly | Scheduled planetarium dark-day drill |

---

## Security & Compliance

- **Framework:** ISO 27001 aligned (not certified — museum budget)
- **SIEM:** Wazuh + custom parsers for MikroTik/OPNsense/Digistar
- **Vuln Management:** Weekly OpenVAS scans, monthly pen test (internal)
- **Access Control:** Tailscale + 2FA, just-in-time elevation
- **Incident Response:** 4-phase runbook (Detect → Contain → Eradicate → Recover)
- **Data Sovereignty:** All data in Paraguay (HQ + OMV), no cloud egress

---

## Key Deliverables (First 90 Days)

| Milestone | Target | Status |
|-----------|--------|--------|
| Kickoff & discovery | Week 1 | ✅ |
| Asset inventory (CMDB) | Week 2 | ✅ |
| Baseline monitoring | Week 3 | ✅ |
| SLA dashboard live | Week 4 | ✅ |
| BCP/DRP documented | Week 6 | ✅ |
| First failover test | Week 8 | ✅ |
| ITIL service catalog | Week 10 | ✅ |
| Security baseline | Week 12 | ✅ |

---

## Governance

| Cadence | Meeting | Participants |
|---------|---------|--------------|
| Daily | Standup (15 min) | On-site team |
| Weekly | Service review (1h) | CTO + MuCi IT Lead |
| Monthly | SLA report + billing | CTO + MuCi Director |
| Quarterly | Strategic review | CTO + MuCi Board |
| Annual | Contract renewal prep | CTO + Legal + Finance |

---

## Technology Stack

| Layer | Technology |
|-------|------------|
| **Hypervisor** | Hyper-V Server 2022 (clustered) |
| **Storage** | TrueNAS Scale (ZFS, replication) |
| **Firewall** | OPNsense (HA pair) |
| **Monitoring** | Prometheus + Grafana + Alertmanager |
| **Logging** | Loki + Promtail |
| **GitOps** | Gitea + ArgoCD (on-prem) |
| **CI/CD** | GitHub Actions (self-hosted runners) |
| **Config Mgmt** | Ansible (network), PowerShell DSC (Windows) |
| **Ticketing** | Zammad (self-hosted) |
| **Docs** | Wiki.js + MkDocs |
| **Secrets** | HashiCorp Vault (dev) / 1Password (team) |

---

## Budget Breakdown (Annual)

| Category | % | Notes |
|----------|---|-------|
| **Personnel (5 FTE)** | 62% | Salaries, benefits, training |
| **Infrastructure** | 18% | Hardware refresh, licenses, cloud burst |
| **Software/SaaS** | 8% | Odoo, Digistar support, monitoring |
| **Security** | 5% | Pen tests, certs, SIEM licensing |
| **Contingency** | 7% | Unplanned, emergency parts |

---

## License

MIT — MINIX Managed Services 🇵🇾