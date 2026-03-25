# VBMS Fiduciary (FID) Architecture Documentation

> **Source:** `fid_arch_page.pdf` — 3026_VBMS Fiduciary (FID) Architecture Documentation  
> **Status:** Final - updated for 3/18/26 submission  
> **Review Date:** 3/18/25

---

## Document Metadata

| Field | Value |
|---|---|
| **BIP Tenant Boundary** | VBMS Fiduciary (bip-bffs) |
| **BIP Tenant Component** | bffs |
| **System Acronym** | VBMS - FID |
| **VASI ID** | 3026 |
| **BIP Tenant App(s)** | bip-bffs |
| **GitHub Link(s)** | bip-bffs (DVA), bip-bffs-config (DVA) |
| **BIH Link(s)** | VBMS Fiduciary |
| **MSR Links(s)** | 2423 |
| **Solution and Support Links** | https://bffs-int.dev.bip.va.gov/webjars/swagger-ui/index.html |
| **C&P Solution Document** | FAST AAT Integration |
| **Sponsoring Organization** | Veterans Benefits Administration |
| **Number of Users** | About 1800 |
| **Estimated Monthly Cost (Prod)** | $3,342.46 |
| **Estimated Monthly Cost (PreProd)** | $11,643.46 |
| **Estimated Monthly Cost (Stage)** | $794.31 |
| **Estimated Monthly Cost (Dev)** | $666.55 |
| **Estimated Monthly Cost (Total)** | $16,446.78 |
| **Privacy** | Both PHI and PII |
| **Will Need 508 Compliance** | Yes |
| **Deployment Date** | November 2020 (R20.1.1) |
| **eMASS ID / ServiceNow ID** | AP0021065 |
| **Accreditation Boundary** | VBMS-Fiduciary |

### Privacy Details
Both PHI and PII data are handled. There is a significant amount of financial information (PII) gathered for both Beneficiaries and Fiduciaries, including name, address, SSN, EIN, date of birth, etc. Beneficiaries can be identified using flags as being blind or having spina bifida (PHI). Beneficiaries can be Veterans or dependents of Veterans (spouse or child). Fiduciaries can be Veterans, individuals, or organizations.

---

## Table of Contents

1. [Use Case View](./01-use-case-view.md)
   - [Capability Viewpoint Vision (CV-1)](./01-use-case-view.md#capability-viewpoint-vision-cv-1)
   - [Capability Viewpoint Taxonomy (CV-2)](./01-use-case-view.md#capability-viewpoint-taxonomy-cv-2)
   - [Use Cases & Main Actors](./01-use-case-view.md#use-cases--main-actors)
   - [Main User Functions](./01-use-case-view.md#main-user-functions)
   - [Fiduciary Use Case Matrix](./01-use-case-view.md#fiduciary-use-case-matrix)

2. [Operational View](./02-operational-view.md)
   - [Primary Systems High Level Operational Viewpoint (OV-1)](./02-operational-view.md#ov-1-primary-systems-high-level-operational-viewpoint)
   - [Operational Resource Flow (OV-2)](./02-operational-view.md#ov-2-operational-resource-flow)

3. [Logical View](./03-logical-view.md)
   - [Main Components Systems Viewpoint (SV-1)](./03-logical-view.md#sv-1-main-components-systems-viewpoint)
   - [Description of Primary Services (SvcV-1)](./03-logical-view.md#svcv-1-description-of-primary-services)
   - [Resource Flow of Primary Services (SvcV-2)](./03-logical-view.md#svcv-2-resource-flow-of-primary-services)
   - [Internal and External Systems-Systems Matrix (SV-3/SvcV-3)](./03-logical-view.md#sv-3svcv-3-internal-and-external-systems-systems-matrix)

4. [State Transitions (OV-6b)](./04-state-transitions.md)

5. [Data View](./05-data-view.md)
   - [Conceptual Data Viewpoint Model (DIV-1)](./05-data-view.md#div-1-conceptual-data-viewpoint-model)
   - [Logical Data Viewpoint Model (DIV-2)](./05-data-view.md#div-2-logical-data-viewpoint-model)
   - [Physical Data Viewpoint Model (DIV-3)](./05-data-view.md#div-3-physical-data-viewpoint-model)

6. [Process View](./06-process-view.md)
   - [Main Business Process Model (OV-6d)](./06-process-view.md#ov-6d-main-business-process-model)
   - [Process Sequence - Systems Event-Trace Description (SV-10c)](./06-process-view.md#sv-10c-process-sequence---systems-event-trace-description)

7. [Implementation View](./07-implementation-view.md)
   - [Primary System Technologies (SV-9)](./07-implementation-view.md#sv-9-primary-system-technologies)
   - [Deployment View](./07-implementation-view.md#deployment-view)
   - [System Configuration](./07-implementation-view.md#system-configuration)
   - [System Monitoring and Metrics](./07-implementation-view.md#system-monitoring-and-metrics)
   - [System Auditing](./07-implementation-view.md#system-auditing)
   - [System Logging](./07-implementation-view.md#system-logging)
   - [Security Requirements](./07-implementation-view.md#security-requirements)

---

## Quick Reference: Diagrams and Images

| Image | Description | Section |
|---|---|---|
| [CV-2 Capability Taxonomy](./images/page-02-img-1.png) | Capability taxonomy diagram | Use Case View |
| [OV-1 High Level Operational Viewpoint](./images/page-07-img-1.png) | Primary systems high-level operational diagram | Operational View |
| [OV-2 Operational Resource Flow](./images/page-08-img-1.png) | Operational resource flow diagram | Operational View |
| [SV-1 Main Components](./images/page-09-img-1.png) | Main components systems viewpoint diagram | Logical View |
| [SV-1 Component References](./images/page-10-img-1.png) | Component references table and details | Logical View |
| [SvcV-2 FID API to BGS (Part 1)](./images/page-12-img-1.png) | FID API to BGS resource flow (part 1) | Logical View |
| [SvcV-2 FID API to BGS (Part 2)](./images/page-13-img-1.png) | FID API to BGS resource flow (part 2) | Logical View |
| [SvcV-2 Beneficiary Resources](./images/page-14-img-1.png) | Beneficiary resource flow diagram | Logical View |
| [SvcV-2 Beneficiary Resources (cont.)](./images/page-15-img-1.png) | Beneficiary resource flow (continued) | Logical View |
| [SvcV-2 Claim Management (Part 1)](./images/page-16-img-1.png) | Claim Management & Summary resource flow (part 1) | Logical View |
| [SvcV-2 Claim Management (Part 2)](./images/page-17-img-1.png) | Claim Management & Summary resource flow (part 2) | Logical View |
| [SvcV-2 DocGen Resources (Part 1)](./images/page-18-img-1.png) | DocGen resource flow (part 1) | Logical View |
| [SvcV-2 DocGen Resources (Part 2)](./images/page-19-img-1.png) | DocGen resource flow (part 2) | Logical View |
| [SvcV-2 EP and Misuse Resources](./images/page-20-img-1.png) | EP and Misuse resource flow | Logical View |
| [SvcV-2 Accounting Audit Resource](./images/page-21-img-1.png) | Accounting Audit resource flow | Logical View |
| [SvcV-2 EP and Misuse (additional)](./images/page-21-img-2.png) | Additional EP/Misuse resource flow | Logical View |
| [SvcV-2 Event Processing Resource](./images/page-22-img-1.png) | Event Processing resource flow | Logical View |
| [SvcV-2 EP590 and EP290 Jobs](./images/page-23-img-1.png) | EP590 and EP290 jobs diagram | Logical View |
| [SvcV-2 Participant Lookup & Veteran](./images/page-24-img-1.png) | Participant Lookup and Veteran resources | Logical View |
| [SV-3/SvcV-3 Interface Matrix Diagram](./images/page-25-img-1.png) | Systems-systems interface matrix diagram | Logical View |
| [State Transitions (OV-6b)](./images/page-25-img-2.png) | State transitions diagram | State Transitions |
| [DIV-1 Conceptual Data Model Diagram](./images/page-26-img-1.png) | Conceptual data viewpoint model diagram | Data View |
| [DIV-2 Logical Data Model](./images/page-27-img-1.png) | Logical data viewpoint model diagram | Data View |
| [OV-6d Business Process Model](./images/page-28-img-1.png) | Main business process model diagram | Process View |
| [OV-6d Business Process (full)](./images/page-28-img-2.png) | Main business process model (full detail) | Process View |
| [SV-10c Event-Trace Description](./images/page-29-img-1.png) | Systems event-trace description diagram | Process View |
| [DIV-3 Physical Data Model](./images/page-29-img-2.png) | Physical data viewpoint model diagram | Data View |
