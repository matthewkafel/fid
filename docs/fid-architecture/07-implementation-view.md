# Implementation View

This section covers the Implementation View of the VBMS Fiduciary (FID) architecture, including technology components, deployment configuration, monitoring, auditing, logging, and security.

---

## Overview

The VBMS-Fiduciary application is a Spring Boot application that is packaged as a Docker image and deployed on the Kubernetes Container Platform (K8s). It is running and configured in the same manner as all other Booz Allen Hamilton (Booz Allen)/VA Spring Boot executables. All data is persisted and retrieved from CorpDB. Data is accessed and updated directly into Oracle using JDBC (Java Database Connectivity). Web service calls to BIS via HTTPS are also used.

The VBMS-Fiduciary application inherits all BIP data integrity and privacy controls that meet VA standards and takes advantage of all BIP Platform capabilities including a robust DevSecOps pipeline that allows application changes to be rapidly developed and deployed while maintaining high quality.

---

## SV-9: Primary System Technologies (VASI Technology Components)

For standard tools, see the VBMS SV-9 Master Table.

| Software Type | Software Vendor | Software Name | Software Version | TRM Link | TRM ID |
|---|---|---|---|---|---|
| Cloud Services/Server Virtualization | Amazon | Elasticache | N/A | N/A | N/A |
| Cloud Services/Server Virtualization | Amazon | OpenSearch | N/A | N/A | N/A |

---

## Deployment View

The Deployment View for VBMS-FID describes how the tenant operates on the BIP platform. Full details may be found in the Deployment View section of BIP Architecture Documentation (2001AM).

### Environments

| Environment | Description |
|---|---|
| **Dev** | Current development version, dev cluster |
| **DEVTest** | Similar to Dev, but with an alternate cycle |
| **SQA** | Independent validation and verification by the quality assurance team |
| **UAT** | Sprint release version, BGS link test dependency, stage cluster |
| **PDT** | Sprint release version, stage cluster |
| **Demo** | Previous release version, stage cluster |
| **PERF** | Performance testing, latest release ready for production, prod cluster |
| **PREPROD** | Next production release |
| **ProdTest** | Test environment for the next production release |
| **Production** | Current production environment used by VA employees |

---

## System Configuration

All clusters that comprise the BIP Platform follow the Infrastructure as Code (IaC) paradigm. Each cluster, starting with the BIP Platform Development cluster, is installed and configured according to IaC scripts. This approach ensures that the configuration of all clusters is consistent and provides common environments for workloads to execute, which removes the infrastructure and configuration drift commonly found in manually managed systems.

---

## System Monitoring and Metrics

BIP Platform monitoring is concerned with the health of the Kubernetes cluster, its components, and the applications that run it. To monitor the cluster:

- **Prometheus** is connected to the Kubernetes cluster API and ingests the metrics and logs for the entire cluster.
- **Alerts** are piped out to email as well as Slack channels for the Operations teams to provide faster response times.
- **Grafana** is used for dashboarding of cluster metrics captured through Prometheus.
- **AWS S3** stores metrics for historical records.

> **Link:** [MSR Site](https://bffs-int.dev.bip.va.gov/webjars/swagger-ui/index.html)

---

## System Auditing

The BIP Platform Operation teams achieve user auditing through the use of:

- **CloudTrail** — provides AWS account-level operational auditing and risk auditing.
- **CloudWatch** — leveraged for AWS instance monitoring and logging.

In addition to user auditing, the BIP Platform also leverages audit logging at the OS level:

- **Fluentd** aggregates audit logs at the OS level.
- **Elasticsearch** stores and indexes audit logs.
- **Kibana** allows auditors to view and search audit logs.

---

## System Logging

For detailed logging on the clusters and the tenant's applications, the BIP Platform leverages an **EFK Stack** (Elasticsearch, Fluentd, and Kibana):

| Component | Role |
|---|---|
| **Fluentd** | Captures logs and forwards them to AWS S3 and Elasticsearch |
| **Elasticsearch** | Stores and indexes logs for fast retrieval |
| **Kibana** | Used by Development tenant teams and Operations to search and visualize logs |

### Log Retention Policy

- Logs are duplicated into **S3** and retained for a specified period:
  - **Non-PII environments:** 30 days in S3 Standard, then transitioned to S3 Glacier
  - **PII environments:** 90 days in S3 Standard, then transitioned to S3 Glacier
- S3 Glacier is a more economical storage class for objects that are accessed infrequently.

---

## Security Requirements

- **Transport Layer Security (TLS)** is present, leveraging SSL certificates issued from the VA certificate authority.
- A **shared secret** between the client and the VBMS-Fiduciary application is used to verify the **JWT (JSON Web Token)**.
- The VBMS-Fiduciary application stores the secret in **Vault**.

### Authentication
- VBA users authenticate to **IAM SSOi** via **PIV card** (Personal Identification Verification).
- External VA business partner users authenticate to **IAM SSOe** via **CAC** (Common Access Card).
- All applications on BIP Platform utilize **BSS (Benefits Security Services)** for centralized SSO.
- Cross-service communication uses **signed tokens from IAM**.

---

*[← Back to README](./README.md)*
