# Atlantis Research & Preservation — Requirements

**Organization:** Atlantis  
**Project prefix:** ARP  
**Project type:** Web application  
**Suggested methodology:** Scrum  
**Suggested sprint length:** 2 weeks  
**Document status:** Initial requirements for AI-platform evaluation

## 1. Purpose

Build a web application for managing underwater research expeditions, cataloging artifacts, monitoring environmental conditions, managing equipment, and supporting the preservation of a submerged ancient civilization. The system should help researchers track findings and respond to unsafe environmental conditions.

## 2. User roles

- **Researcher:** views assigned expeditions and records observations and findings.
- **Expedition Lead:** manages expedition plans, team assignments, and operational status.
- **Conservator:** reviews artifacts and records preservation actions.
- **Safety Officer:** reviews environmental readings and manages safety alerts.
- **Administrator:** manages users and system configuration.

## 3. Functional requirements

### ARP-FR-01 — Expedition management
Authorized users shall create and update expeditions with a unique expedition ID, name, site, planned dates, assigned lead, team members, and status. Status values shall be Planned, Active, Paused, Completed, or Cancelled.

### ARP-FR-02 — Researcher and team management
Administrators shall manage researcher profiles. Expedition Leads shall assign active researchers to expeditions.

### ARP-FR-03 — Artifact catalog
Researchers shall register artifacts with a unique artifact ID, name, description, discovery site, discovery date, condition, and associated expedition. Condition values shall be Unknown, Stable, At Risk, or Damaged.

### ARP-FR-04 — Artifact preservation review
Conservators shall review artifacts and record a preservation status of Pending Review, In Assessment, Preservation Required, or Stable. Preservation actions shall include action description, responsible conservator, and date.

### ARP-FR-05 — Environmental readings
Authorized users or connected instruments shall record environmental readings for a research site, including timestamp, water temperature, pressure, and water-quality indicator. The system shall reject missing required values and values outside configured sensor limits.

### ARP-FR-06 — Safety thresholds and alerts
The Safety Officer shall be able to configure minimum and maximum acceptable values for supported environmental measurements. When a new reading exceeds a configured threshold, the system shall create a safety alert that includes the site, measurement, observed value, threshold, timestamp, and status.

### ARP-FR-07 — Alert lifecycle
Safety Officers shall be able to acknowledge and resolve alerts. An alert cannot be resolved without resolution notes. The system shall retain alert history.

### ARP-FR-08 — Equipment management
Authorized users shall register equipment with a unique equipment ID, name, type, condition, and status. Equipment status values shall be Available, Assigned, Maintenance, or Retired.

### ARP-FR-09 — Equipment assignment
Expedition Leads shall assign available equipment to an expedition. Equipment in Maintenance or Retired status cannot be assigned to a new expedition.

### ARP-FR-10 — Research log
Researchers shall record observations linked to an expedition, including date, site, description, author, and optional artifact reference. Logs shall be searchable by expedition and date.

### ARP-FR-11 — Research dashboard
The dashboard shall show active expeditions, artifacts requiring preservation review, recent environmental readings, open safety alerts, and equipment under maintenance.

### ARP-FR-12 — Access control and audit
The system shall enforce role-based access for artifact reviews, threshold configuration, alert resolution, equipment management, and user administration. Important changes shall be auditable.

## 4. Business rules

- **ARP-BR-01:** Expedition, artifact, researcher, and equipment IDs must be unique within their respective record types.
- **ARP-BR-02:** An expedition's end date must not precede its start date.
- **ARP-BR-03:** Artifacts and research logs must reference an existing expedition.
- **ARP-BR-04:** Equipment in Maintenance or Retired status cannot be assigned.
- **ARP-BR-05:** A reading that breaches a configured threshold must generate an alert.
- **ARP-BR-06:** A safety alert cannot be marked Resolved without resolution notes.
- **ARP-BR-07:** Only authorized Safety Officers may configure thresholds or resolve safety alerts.
- **ARP-BR-08:** Researchers may not approve their own artifact preservation review unless an explicit exception is approved.

## 5. Non-functional requirements

- **ARP-NFR-01 — Performance:** The dashboard should load within 3 seconds under normal conditions.
- **ARP-NFR-02 — Data integrity:** Environmental readings must retain their timestamps and site references.
- **ARP-NFR-03 — Security:** Sensitive research records and administrative actions must be protected by server-side authorization.
- **ARP-NFR-04 — Reliability:** A failed reading submission must not create a misleading partial record or duplicate alert.
- **ARP-NFR-05 — Auditability:** Threshold changes, alert status changes, and preservation actions must record actor and timestamp.
- **ARP-NFR-06 — Usability:** Safety alerts must clearly identify the breached measurement and configured threshold.

## 6. Acceptance criteria

- **ARP-AC-01:** Given a valid expedition with a unique ID and valid dates, when an authorized user saves it, then the expedition is created.
- **ARP-AC-02:** Given an artifact references an existing expedition, when a researcher saves the artifact, then it is stored and appears in the catalog.
- **ARP-AC-03:** Given an environmental reading exceeds a configured threshold, when the reading is accepted, then the system creates an alert with the relevant measurement and threshold details.
- **ARP-AC-04:** Given an alert has no resolution notes, when a Safety Officer attempts to resolve it, then the system rejects the status change.
- **ARP-AC-05:** Given equipment is in Maintenance status, when a lead attempts to assign it to a new expedition, then the system rejects the assignment.
- **ARP-AC-06:** Given a user lacks permission to configure thresholds, when they attempt the operation, then the system denies the action and preserves the current threshold.
- **ARP-AC-07:** Given a researcher submits a reading outside configured sensor limits, then the system rejects it with a clear validation message.

## 7. Open questions and assumptions to flag

1. What units are used for temperature and pressure?
2. What are the default threshold values for each environmental measurement?
3. Should alerts be created for values equal to a threshold, or only values beyond it?
4. What should happen if a sensor sends duplicate or out-of-order readings?
5. Can an artifact be reviewed by the same researcher who registered it?
6. What is the required response time for Critical safety alerts?
7. Can equipment be assigned to multiple expeditions at the same time?
8. What retention and export rules apply to research data?

The AI should flag these questions rather than inventing values, units, or operational policies.
