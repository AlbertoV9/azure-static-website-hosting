# Architecture Decisions

---

# Version 1 - Azure Storage Account Static Website Hosting

## Context

The first implementation of the project uses Azure Storage Account Static Website Hosting.

The goal of this phase is not to build the final production architecture but to gain practical experience with Azure Storage, deployment processes, storage security settings, networking options, data protection capabilities and cost evaluation.

Future versions may replace this architecture with Azure Static Web Apps or Azure App Service.

---

## ADR-001: Initial Website Hosting Platform

### Decision

Host the first version of the website using Azure Storage Account Static Website Hosting.

### Reasoning

The Storage Account based approach provides:

- very low operating cost
- minimal infrastructure requirements
- practical exposure to Azure Storage services
- understanding of hosting fundamentals

The objective of this phase is educational and architectural exploration rather than implementation of the final hosting platform.

### Alternatives Considered

- Azure Static Web Apps
- Azure App Service

### Status

Implemented

## ADR-002: Azure Region Selection

### Decision

Deploy resources in Poland Central.

### Reasoning

Poland Central provided lower estimated costs when compared against East US during initial calculations.

Additional factors:

- target audience is expected to be primarily located in Europe
- all required Azure services are available in the region
- data remains within the European Union

### Trade-offs

Pros

- Lower projected cost
- Regional proximity to expected users

Cons

- Smaller Azure region compared with West Europe

### Status

Implemented

## ADR-003: Storage Workload Type

### Decision

Select Primary Workload - General Purpose.

### Reasoning

The Storage Account will operate as the primary hosting location for static website assets.

Specialized workload types such as analytics-oriented configurations would not align with the project requirements.

### Status

Implemented

## ADR-004: Storage Performance Tier

### Decision

Select Standard performance.

### Reasoning

The website consists of:

- HTML
- CSS
- JavaScript
- Images

Premium storage performance would increase costs without providing meaningful benefits for a static website workload.

### Alternatives Considered

- Premium

### Status

Implemented

## ADR-005: Storage Replication Strategy

### Decision

Select Zone Redundant Storage (ZRS).

### Reasoning

Locally Redundant Storage (LRS) would have been sufficient for a personal website.

However, ZRS was selected to increase resilience against datacenter-level failures and to gain practical exposure to higher availability configurations.

### Alternatives Considered

- LRS

### Trade-offs

Pros

- Higher availability
- Zone-level resiliency

Cons

- Increased cost

### Status

Implemented

## ADR-006: Advanced Storage Features

### Decision

Disable:

- Hierarchical Namespace
- SFTP
- NFS v3
- Cross-tenant Replication

### Reasoning

The project hosts a simple static website and does not require:

- Data Lake functionality
- file transfer workloads
- analytics workloads
- cross-tenant replication

These services would introduce unnecessary complexity.

### Status

Implemented


## ADR-007: Data Access Tier

### Decision

Select Hot tier.

### Reasoning

Website assets are expected to be accessed regularly by visitors.

Hot tier is optimized for frequently accessed content.

### Alternatives Considered

- Cool
- Archive

### Status

Implemented

## ADR-008: Data Protection Strategy

### Decision

Enable:

- Blob Soft Delete
- Container Soft Delete

Disable:

- Point-in-Time Restore
- Blob Change Feed
- Version-Level Immutability
- File Share Soft Delete

### Reasoning

The selected features provide protection against accidental deletion while avoiding unnecessary complexity and cost.

Additional source code protection is already provided through Git and GitHub.

### Status

Implemented

## ADR-009: Storage Security Baseline

### Decision

Enable:

- Secure Transfer Required
- TLS 1.2

Disable:

- Blob Anonymous Access
- Microsoft Defender for Storage

### Reasoning

The project adopts a basic security baseline while maintaining low operating costs.

TLS 1.2 is enforced for all supported communications.

Microsoft Defender for Storage was not enabled because the workload contains only static public website assets.

### Status

Implemented

## ADR-010: Encryption Strategy

### Decision

Use Microsoft Managed Keys (MMK).

Do not enable:

- Customer Managed Keys
- Infrastructure Encryption

### Reasoning

The workload hosts public website assets and does not currently require advanced compliance or key management controls.

Microsoft Managed Keys provide encryption at rest while minimizing operational overhead.

### Status

Implemented