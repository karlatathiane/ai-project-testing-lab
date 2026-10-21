# Treasure Planet Navigation System — Requirements

**Organization:** Treasure Planet  
**Project prefix:** TPNS  
**Project type:** Web application  
**Suggested methodology:** Scrum  
**Suggested sprint length:** 3 weeks  
**Document status:** Initial requirements for AI-platform evaluation

## 1. Purpose

Build a mission-management application for planning interplanetary expeditions, managing spacecraft and crews, tracking supplies, monitoring mission status, and recording discoveries. The system must help mission coordinators identify readiness problems before a spacecraft departs.

## 2. User roles

- **Mission Coordinator:** creates missions, plans routes, and assigns crews.
- **Pilot:** views assigned missions and updates flight status.
- **Engineer:** records spacecraft readiness and maintenance issues.
- **Mission Analyst:** reviews mission telemetry summaries and discovery records.
- **Administrator:** manages users and configuration.

## 3. Functional requirements

### TPNS-FR-01 — Spacecraft registry
Administrators and authorized engineers shall register spacecraft with a unique craft ID, name, capacity, fuel capacity, current fuel level, operational status, and maintenance status.

### TPNS-FR-02 — Crew management
Authorized users shall register crew members and assign them to missions. Each crew member shall have a unique ID, name, role, and active status.

### TPNS-FR-03 — Mission creation
A Mission Coordinator shall create a mission with a unique mission ID, destination, planned departure and return times, assigned spacecraft, required crew count, and mission priority. Priority values shall be Low, Normal, High, or Critical.

### TPNS-FR-04 — Route planning
A Mission Coordinator shall add ordered route segments to a mission. Each segment shall include origin, destination, estimated duration, and estimated fuel consumption. Duration and fuel consumption must be positive values.

### TPNS-FR-05 — Mission readiness validation
Before a mission can be marked Ready for Departure, the system shall validate that the spacecraft is operational, fuel is sufficient for the planned route, the assigned crew meets the required minimum, and required maintenance is not outstanding.

### TPNS-FR-06 — Mission status
The system shall support the statuses Draft, Planned, Ready for Departure, In Progress, Paused, Completed, and Aborted. Status changes shall be recorded with actor and timestamp.

### TPNS-FR-07 — Fuel and supply tracking
Authorized engineers shall record fuel and supply updates. The system shall reject updates that result in fuel below zero or above the spacecraft's defined fuel capacity.

### TPNS-FR-08 — Maintenance issues
Engineers shall report maintenance issues with description, severity, spacecraft reference, creation time, and status. A spacecraft with an unresolved issue marked Critical shall not be cleared for departure.

### TPNS-FR-09 — Mission event log
Pilots and authorized crew members shall record mission events with timestamp, event type, description, and mission reference. Events shall be searchable by mission and time range.

### TPNS-FR-10 — Discovery records
Mission Analysts shall record discoveries linked to a mission, including name, description, location, discovery time, and review status.

### TPNS-FR-11 — Mission dashboard
The system shall display mission status, spacecraft readiness, crew assignments, planned route, fuel status, open critical maintenance issues, and recorded discoveries.

### TPNS-FR-12 — Access control
The system shall enforce role-based access to spacecraft configuration, route planning, mission status changes, fuel updates, maintenance records, and discovery records.

## 4. Business rules

- **TPNS-BR-01:** Spacecraft IDs, crew IDs, and mission IDs must be unique within their respective record types.
- **TPNS-BR-02:** Planned return time must be later than planned departure time.
- **TPNS-BR-03:** A mission cannot become Ready for Departure unless all readiness checks pass.
- **TPNS-BR-04:** A spacecraft with a Critical unresolved maintenance issue cannot depart.
- **TPNS-BR-05:** Fuel must remain between zero and the spacecraft's defined fuel capacity, inclusive.
- **TPNS-BR-06:** The assigned crew count must meet or exceed the mission's required minimum.
- **TPNS-BR-07:** Route segments must be ordered and connected: the destination of one segment must match the origin of the next segment.
- **TPNS-BR-08:** Only authorized roles may change mission status or spacecraft readiness data.

## 5. Non-functional requirements

- **TPNS-NFR-01 — Performance:** The mission dashboard should load within 3 seconds under normal conditions.
- **TPNS-NFR-02 — Reliability:** Mission readiness checks must use the latest saved spacecraft, fuel, crew, and maintenance data.
- **TPNS-NFR-03 — Data integrity:** Simultaneous updates must not allow fuel values outside the permitted range.
- **TPNS-NFR-04 — Security:** Role-based permissions must be enforced server-side.
- **TPNS-NFR-05 — Auditability:** Mission status and readiness changes must include actor and timestamp.
- **TPNS-NFR-06 — Usability:** Readiness failures should identify which checks failed and what needs attention.

## 6. Acceptance criteria

- **TPNS-AC-01:** Given a mission with valid dates and a unique ID, when a coordinator saves it, then the mission is created.
- **TPNS-AC-02:** Given a route whose total estimated fuel consumption exceeds available fuel, when readiness is checked, then the mission cannot be marked Ready for Departure.
- **TPNS-AC-03:** Given a spacecraft has an unresolved Critical maintenance issue, when readiness is checked, then departure is blocked.
- **TPNS-AC-04:** Given the assigned crew count is below the required minimum, when readiness is checked, then the system identifies the crew shortfall.
- **TPNS-AC-05:** Given connected route segments, when the route is saved, then the route order is retained.
- **TPNS-AC-06:** Given an unauthorized user attempts to change mission status, when the request is submitted, then the system rejects it and preserves the current status.
- **TPNS-AC-07:** Given fuel is updated with a value above spacecraft capacity or below zero, then the system rejects the update.

## 7. Open questions and assumptions to flag

1. Is a safety reserve required in addition to the route's estimated fuel consumption?
2. How is total route fuel consumption calculated when conditions change during a mission?
3. Can a mission be edited after it has entered In Progress?
4. Which roles may pause, abort, or complete a mission?
5. What are the exact rules for resuming a Paused mission?
6. How should the system handle loss of telemetry or communication?
7. Can one crew member be assigned to overlapping missions?
8. What criteria determine when a discovery is approved?

The AI should identify these unresolved decisions and avoid presenting assumptions as confirmed policy.
