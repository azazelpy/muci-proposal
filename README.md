# 📋 MuCi Proposal — Winning IT Services Bid for Paraguay's National Science Museum

> **Status:** ✅ AWARDED — 6-year contract (2024-2030)  
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
│        │ (OPNsense)│ │ (TrueNAS)│ │  Cluster │               │
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
                    │  NOC + OMV      │
                    └─────────────────┘
```

---

## Key Deliverables (First 90 Days)

| Week | Milestone | Acceptance Criteria |
|------|-----------|---------------------|
| 1-2 | **Transition** | Inventory complete, credentials in Vault, runbooks documented |
| 3-4 | **Monitoring Live** | Prometheus + Grafana + Alertmanager → Telegram/Email |
| 5-6 | **Backup Validated** | Veeam/rsync → OMV ZFS, test restores weekly |
| 7-8 | **DR Drill** | Planetarium failover to cold spare < 2h (RTO met) |
| 9-10 | **Hardening** | CIS benchmarks, vuln scan 0 critical, pen test report |
| 11-12 | **Steady State** | All SLAs green 30 consecutive days |

---

## BCP/DRP Highlights

| Scenario | RPO | RTO | Strategy |
|----------|-----|-----|----------|
| Planetarium server failure | 1h | 2h | Cold spare on-site + config in Git |
| Storage array failure | 1h | 4h | ZFS replication to OMV (async) |
| Site power loss | 0 | 0 | UPS (30min) → Generator (72h fuel) |
| Ransomware | 1h | 4h | Immutable backups, air-gapped copy |
| WAN outage | 0 | 30s | Dual ISP (Tigo + Personal) + SSTP failover |

---

## Budget Breakdown (Annual)

| Category | Year 1 | Year 2-6 | Notes |
|----------|--------|----------|-------|
| **Personnel (5 FTE)** | US$420,000 | US$441,000 | +5% annual adjustment |
| **Infrastructure** | US$85,000 | US$45,000 | Y1: hardware refresh |
| **Licenses & Cloud** | US$35,000 | US$38,000 | Veeam, Monitoring, Microsoft 365 |
| **Security** | US$25,000 | US$28,000 | SIEM, pen test, training |
| **Contingency (10%)** | US$56,500 | US$55,200 | |
| **TOTAL / YEAR** | **US$621,500** | **US$607,200** | |
| **6-YEAR TOTAL** | **~US$3.65M** | | |

---

## Governance & Reporting

| Cadence | Artifact | Audience |
|---------|----------|----------|
| **Daily** | NOC shift log, backup status | Internal |
| **Weekly** | Incident trend, SLA dashboard, capacity | Service Manager |
| **Monthly** | Executive SLA report, budget burn, risk register | MuCi Director |
| **Quarterly** | Strategic review, roadmap, tech debt | MuCi Board + MINIX CTO |
| **Annual** | Contract renewal prep, major upgrade plan | Procurement + Legal |

---

## Security & Compliance

- **ISO 27001 aligned** (not certified — museum budget)
- **Data sovereignty** — all data in Paraguay (OMV + cloud region SA-East-1)
- **GDPR/Paraguay PDP** — DPIA for visitor tracking, consent management
- **Vulnerability management** — Monthly scans, 30-day patch SLA for Critical
- **Penetration test** — Annual (external), quarterly (internal automated)

---

## Why MINIX Won

1. **Proven track record** — Lacerie network (10 sites, 99.97% uptime 2023)
2. **Planetarium experience** — Digistar integration, show control systems
3. **Local team** — Spanish/Guaraní support, on-site in 30 min
4. **No vendor lock-in** — All configs as code, open standards
5. **Cost efficiency** — 23% below next bidder, superior SLA
6. **Innovation** — OmniRoute LLM gateway for runbook automation, Karina bot for invoice processing

---

## License

MIT — MINIX Proposal Archive 🇵🇾

*This document sanitized from winning proposal v2.0 (47 KB, 16 sections + 10 anexos). Financial details and specific technical configurations redacted per confidentiality clause.*