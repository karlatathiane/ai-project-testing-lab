# El Dorado Exploration System — Requirements

**Organization:** Eldorado  
**Project prefix:** EES  
**Project type:** Web application  
**Suggested methodology:** Kanban  
**Sprint approach:** Continuous flow; do not assume fixed sprints unless the platform requires them  
**Document status:** Initial requirements for AI-platform evaluation

## 1. Purpose

Build a web application for organizing expeditions, registering explorers, cataloging artifacts, tracking supplies, and documenting discoveries. The system should help an exploration team understand expedition status and maintain reliable records of recovered artifacts.

## 2. User roles

- **Explorer:** views assigned expeditions and records field observations.
- **Expedition Lead:** manages assigned expeditions, team members, and supply requests.
- **Curator:** reviews artifact records and controls catalog status.
- **Administrator:** manages user accounts and system-wide configuration.

## 3. Functional requirements

### EES-FR-01 — Explorer registry
Administrators shall create, edit, view, and deactivate explorer profiles. Each profile shall include a unique explorer ID, name, contact details, and status.

### EES-FR-02 — Expedition management
Authorized users shall create and update expeditions with a unique expedition code, name, destination, planned start date, planned end date, lead, status, and assigned explorers. Status values shall be Planned, Active, Paused, Completed, or Cancelled.

### EES-FR-03 — Team assignment
An Expedition Lead shall assign active explorers to an expedition. The system shall prevent assigning deactivated explorers to new expeditions.

### EES-FR-04 — Artifact catalog
Explorers shall be able to submit an artifact record containing a unique artifact ID, name, description, discovery location, discovery date, condition, and expedition reference. Condition values shall be Unknown, Good, Damaged, or Fragile.

### EES-FR-05 — Artifact review
A Curator shall review submitted artifact records and set the review status to Pending, Approved, or Rejected. Rejected records must include a reason. The system shall retain the review decision and reviewer.

### EES-FR-06 — Supply inventory
Authorized users shall register supplies with a unique item code, name, quantity, unit, and storage location. The system shall support increasing and decreasing stock quantities and reject a transaction that would make available stock negative.

### EES-FR-07 — Supply requests
An Expedition Lead shall submit a supply request for an expedition, including requested items and quantities. The request shall have a status of Submitted, Approved, Rejected, or Fulfilled. Rejected requests must include a reason.

### EES-FR-08 — Discovery log
Explorers shall record a discovery log entry linked to an expedition, including date, location, description, and author. Entries shall be searchable by expedition and date.

### EES-FR-09 — Expedition dashboard
Users with appropriate permissions shall view expedition status, assigned team members, pending artifact reviews, and outstanding supply requests.

### EES-FR-10 — Access control and audit
The system shall restrict artifact approval, inventory adjustments, and user administration by role. Important changes to artifact review status, stock quantities, and expedition status shall be auditable.

## 4. Business rules

- **EES-BR-01:** Explorer IDs, expedition codes, artifact IDs, and item codes must be unique within their respective record types.
- **EES-BR-02:** The planned end date of an expedition must not precede its planned start date.
- **EES-BR-03:** New assignments must reference active explorers.
- **EES-BR-04:** An artifact must reference an existing expedition.
- **EES-BR-05:** A rejected artifact or supply request must include a rejection reason.
- **EES-BR-06:** Available inventory must never become negative.
- **EES-BR-07:** A completed or cancelled expedition must not accept new discovery log entries unless an authorized exception is defined.
- **EES-BR-08:** Only Curators may approve or reject artifact records.

## 5. Non-functional requirements

- **EES-NFR-01 — Performance:** Common search and list operations should complete within 3 seconds under normal load.
- **EES-NFR-02 — Data integrity:** Inventory adjustments must be atomic so simultaneous requests cannot produce an invalid negative balance.
- **EES-NFR-03 — Security:** Role restrictions must be enforced by the server.
- **EES-NFR-04 — Usability:** Search and filters should be available for expeditions, artifacts, and discovery logs.
- **EES-NFR-05 — Auditability:** Important changes must include actor and timestamp.
- **EES-NFR-06 — Accessibility:** Forms must provide clear labels and validation messages.

## 6. Acceptance criteria

- **EES-AC-01:** Given a valid expedition with a unique code and valid dates, when an authorized user saves it, then the expedition is created.
- **EES-AC-02:** Given an expedition start date, when an earlier end date is submitted, then the system rejects the dates.
- **EES-AC-03:** Given an artifact is submitted, when a Curator approves it, then the artifact status and reviewer are recorded.
- **EES-AC-04:** Given a Curator rejects an artifact without a reason, then the system prevents the rejection from being saved.
- **EES-AC-05:** Given inventory contains a known quantity, when a stock reduction exceeds available stock, then the system rejects the adjustment.
- **EES-AC-06:** Given an Explorer lacks curator permissions, when they attempt to approve an artifact, then the action is denied.
- **EES-AC-07:** Given a supply request is rejected, when it is saved, then a rejection reason is required.

## 7. Open questions and assumptions to flag

1. Can an explorer belong to multiple active expeditions at once?
2. Who may edit an artifact after it has been approved?
3. Are artifact IDs entered manually or generated by the system?
4. What is the approval workflow for supply requests?
5. Are stock quantities allowed to use decimals, or only whole numbers?
6. Can discovery entries be added after an expedition is completed?
7. What happens to outstanding supply requests when an expedition is cancelled?
8. What defines a “fragile” artifact handling procedure?

The AI should surface these questions rather than inventing policy.
