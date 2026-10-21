# Monster Energy Management — Test Scenarios

**Prefix:** MEM  
**Purpose:** Evaluate generation of positive, negative, boundary, authorization, audit, and reporting tests.

## Employee management

### MEM-TS-01 — Create an employee with valid data
**Type:** Positive  
**Preconditions:** Administrator is signed in.  
**Steps:** Open employee management; enter a unique employee ID, name, role, and active status; save.  
**Expected result:** Employee is created and appears in the employee list.

### MEM-TS-02 — Reject a duplicate employee ID
**Type:** Negative / validation  
**Preconditions:** An employee with the submitted ID already exists.  
**Steps:** Attempt to create another employee using the same ID.  
**Expected result:** The system rejects the duplicate and explains the validation error; the original record remains unchanged.

### MEM-TS-03 — Deactivated employee attempts to sign in
**Type:** Negative / access control  
**Preconditions:** An employee account is deactivated.  
**Steps:** Attempt to sign in using that employee's valid credentials.  
**Expected result:** Access is denied.

## Doors and production

### MEM-TS-04 — Register a door with a duplicate ID
**Type:** Negative / validation  
**Preconditions:** A door with the ID already exists.  
**Steps:** Attempt to register another door with that ID.  
**Expected result:** The system rejects the duplicate.

### MEM-TS-05 — Record production for an available door
**Type:** Positive  
**Preconditions:** Operator is active; door status is Available.  
**Steps:** Submit a valid door, start time, later end time, and non-negative energy value.  
**Expected result:** Production record is saved and attributed to the operator.

### MEM-TS-06 — Reject negative energy units
**Type:** Negative / boundary  
**Preconditions:** Operator is authorized and door is Available.  
**Steps:** Submit a production record with energy units below zero.  
**Expected result:** The system rejects the record and identifies the invalid value.

### MEM-TS-07 — Reject an invalid time interval
**Type:** Negative / boundary  
**Preconditions:** Operator is authorized and door is Available.  
**Steps:** Submit a record where end time equals or precedes start time.  
**Expected result:** The system rejects the record.

### MEM-TS-08 — Prevent production recording for an unavailable door
**Type:** Negative / business rule  
**Preconditions:** Door status is Maintenance or Out of Service.  
**Steps:** Attempt to submit a production record for the door.  
**Expected result:** The system rejects the record and does not include it in totals.

### MEM-TS-09 — Dashboard totals reflect saved production
**Type:** Positive / reporting  
**Preconditions:** Known production records exist for multiple doors within and outside a selected date range.  
**Steps:** Filter the dashboard by date range and door.  
**Expected result:** Totals and per-door figures include only matching records. Exact date-boundary behavior should follow a clarified requirement.

## Incidents, permissions, and audit

### MEM-TS-10 — Resolve an incident without resolution notes
**Type:** Negative / business rule  
**Preconditions:** An incident is Open or In Progress.  
**Steps:** Attempt to change its status to Resolved without entering resolution notes.  
**Expected result:** The status change is rejected.

### MEM-TS-11 — Verify role-based access
**Type:** Security / authorization  
**Preconditions:** Sign in as an Operator.  
**Steps:** Attempt to access an administrative operation, such as deactivating an employee, including by direct URL or request if applicable.  
**Expected result:** The operation is denied on the server and no data changes.

### MEM-TS-12 — Verify audit history for a door status change
**Type:** Positive / audit  
**Preconditions:** An authorized user can change door status.  
**Steps:** Change a door from Available to Maintenance and inspect its audit history.  
**Expected result:** The history contains the actor, action, target door, and timestamp.

### MEM-TS-13 — Attempt to resolve a Critical incident with notes
**Type:** Positive  
**Preconditions:** A Critical incident exists and the user is a supervisor.  
**Steps:** Add resolution notes and mark the incident Resolved.  
**Expected result:** The status changes to Resolved and the resolution details are retained.

## Questions for the AI to identify

- Are overlapping production periods allowed?
- What time zone and date-boundary rules apply to dashboard filters?
- Are notifications required for Critical incidents?
- Which roles may deactivate employees?
- What are the retention rules for audit history?
