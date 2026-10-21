# Monster Energy Management — Requirements

**Organization:** Monstros SA  
**Project prefix:** MEM  
**Project type:** Web application  
**Suggested methodology:** Scrum  
**Suggested sprint length:** 2 weeks  
**Document status:** Initial requirements for AI-platform evaluation

## 1. Purpose

Build a web application for monitoring energy production in a monster-powered factory, managing employees and dimensional doors, and recording operational incidents. The system should help supervisors understand production performance and respond to equipment or process problems.

## 2. User roles

- **Operator:** views assigned production information and reports incidents.
- **Supervisor:** monitors production, reviews incidents, and views operational dashboards.
- **Administrator:** manages employee accounts, door records, and system configuration.

Users must only access actions and data permitted for their role.

## 3. Functional requirements

### MEM-FR-01 — Employee management
The system shall allow an administrator to create, view, edit, and deactivate employee records. Each record shall include employee ID, full name, role, and status. Employee IDs must be unique.

### MEM-FR-02 — Dimensional door registry
The system shall allow an administrator to register a dimensional door with a unique door ID, location, operational status, and assigned production area. Door status shall be one of: Available, In Use, Maintenance, or Out of Service.

### MEM-FR-03 — Production recording
The system shall allow an authorized operator to record energy production for a door and a time period. Each record shall include door ID, start time, end time, energy units produced, and the responsible operator. End time must be later than start time, and energy units must not be negative.

### MEM-FR-04 — Production dashboard
The system shall provide supervisors with a dashboard showing total energy produced over a selected period, production by door, and the number of operational incidents. Filters shall include date range and door.

### MEM-FR-05 — Incident reporting
Operators and supervisors shall be able to create an incident with a title, description, related door (when applicable), severity, creation time, and status. Severity shall be Low, Medium, High, or Critical. Status shall be Open, In Progress, or Resolved.

### MEM-FR-06 — Incident lifecycle
Supervisors shall be able to assign an incident, update its status, and add resolution notes. Resolved incidents must include resolution notes. The system shall preserve the incident history.

### MEM-FR-07 — Door status changes
Authorized users shall be able to change a door's status. A door marked Maintenance or Out of Service must not be selectable for a new production record. The system shall record who changed the status and when.

### MEM-FR-08 — Access control
The system shall enforce role-based access for employee administration, door configuration, production recording, dashboards, and incident management. Unauthorized actions shall be rejected.

### MEM-FR-09 — Audit history
The system shall record important changes, including employee deactivation, door status changes, production record creation, and incident status changes. Audit entries shall include actor, action, target record, and timestamp.

## 4. Business rules

- **MEM-BR-01:** Employee IDs and door IDs must be unique.
- **MEM-BR-02:** Production records must reference an existing door and an active operator.
- **MEM-BR-03:** Production cannot be recorded for a door in Maintenance or Out of Service status.
- **MEM-BR-04:** Production units cannot be negative; the end time must be later than the start time.
- **MEM-BR-05:** An incident can be marked Resolved only when resolution notes are provided.
- **MEM-BR-06:** Deactivated employees cannot sign in or create new production records.
- **MEM-BR-07:** Changes to door status and incident status must be auditable.

## 5. Non-functional requirements

- **MEM-NFR-01 — Performance:** The dashboard should load within 3 seconds under normal operating conditions.
- **MEM-NFR-02 — Security:** Passwords must be stored using an appropriate secure password-hashing mechanism.
- **MEM-NFR-03 — Authorization:** Server-side authorization must protect restricted operations; hiding a button alone is insufficient.
- **MEM-NFR-04 — Usability:** The interface should work on desktop and tablet screen sizes.
- **MEM-NFR-05 — Data integrity:** Invalid production values and references to nonexistent records must be rejected.
- **MEM-NFR-06 — Auditability:** Audit records must identify who performed a tracked action and when.

## 6. Acceptance criteria

- **MEM-AC-01:** Given an administrator creates an employee with a new ID and valid required fields, when the record is saved, then the employee is created.
- **MEM-AC-02:** Given a door is in Maintenance status, when an operator attempts to record production for it, then the system rejects the record.
- **MEM-AC-03:** Given valid production data for an eligible door, when an authorized operator submits it, then the record is saved and included in dashboard totals.
- **MEM-AC-04:** Given an incident has no resolution notes, when a supervisor attempts to mark it Resolved, then the system rejects the status change.
- **MEM-AC-05:** Given a user lacks permission for an administrative action, when they attempt that action, then access is denied and the data remains unchanged.
- **MEM-AC-06:** Given a door status changes, when the update succeeds, then the system records the actor and timestamp in the audit history.

## 7. Open questions and assumptions to flag

1. What unit of measurement is used for energy production?
2. Can production records be edited or deleted after submission?
3. What are the retention requirements for audit history?
4. Should Critical incidents trigger notifications, and through which channel?
5. Can a door have overlapping production periods, or must the system prevent them?
6. What exact dashboard date boundaries and time zone should be used?
7. Should supervisors be allowed to deactivate employees, or only administrators?

The AI should identify these as unresolved questions rather than silently inventing business rules.
