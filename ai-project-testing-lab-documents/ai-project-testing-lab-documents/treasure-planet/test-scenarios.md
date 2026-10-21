# Treasure Planet Navigation System — Test Scenarios

**Prefix:** TPNS  
**Purpose:** Evaluate dependency handling, readiness validation, boundaries, concurrency, and critical-path scenarios.

## Spacecraft and mission setup

### TPNS-TS-01 — Register a spacecraft with valid values
**Type:** Positive  
**Preconditions:** Authorized user is signed in.  
**Steps:** Create a spacecraft with a unique ID, valid capacity, fuel capacity, fuel level within range, and operational status.  
**Expected result:** The spacecraft is saved.

### TPNS-TS-02 — Reject fuel above capacity
**Type:** Negative / boundary  
**Preconditions:** A spacecraft has a defined fuel capacity.  
**Steps:** Attempt to set its fuel level above that capacity.  
**Expected result:** The system rejects the update and preserves the prior fuel value.

### TPNS-TS-03 — Reject an invalid mission time range
**Type:** Negative / boundary  
**Preconditions:** Coordinator can create missions.  
**Steps:** Enter a planned return time equal to or earlier than departure time.  
**Expected result:** The system rejects the mission dates.

### TPNS-TS-04 — Prevent duplicate mission IDs
**Type:** Negative / validation  
**Preconditions:** A mission with the submitted ID already exists.  
**Steps:** Attempt to create another mission using that ID.  
**Expected result:** The system rejects the duplicate.

## Route and readiness

### TPNS-TS-05 — Save a connected multi-segment route
**Type:** Positive  
**Preconditions:** A mission exists in Draft or Planned status.  
**Steps:** Add two route segments where the first destination equals the second origin.  
**Expected result:** The route is saved in the specified order.

### TPNS-TS-06 — Reject a disconnected route
**Type:** Negative / business rule  
**Preconditions:** Coordinator is editing a mission route.  
**Steps:** Add consecutive route segments whose connecting locations do not match.  
**Expected result:** The system rejects the route or identifies the discontinuity.

### TPNS-TS-07 — Block departure when fuel is insufficient
**Type:** Negative / business rule  
**Preconditions:** Mission route has known estimated fuel consumption greater than available fuel.  
**Steps:** Run readiness validation and attempt to mark the mission Ready for Departure.  
**Expected result:** Departure readiness is denied and the fuel shortfall is identified.

### TPNS-TS-08 — Block departure for a Critical maintenance issue
**Type:** Negative / safety rule  
**Preconditions:** Assigned spacecraft has an unresolved Critical issue.  
**Steps:** Run readiness validation.  
**Expected result:** Mission cannot become Ready for Departure and the issue is shown.

### TPNS-TS-09 — Block departure when crew is below minimum
**Type:** Negative / boundary  
**Preconditions:** Mission requires a specified minimum crew count.  
**Steps:** Assign fewer active crew members than required and run readiness validation.  
**Expected result:** Readiness is denied and the missing crew count is identified.

### TPNS-TS-10 — Confirm readiness when all checks pass
**Type:** Positive / integration  
**Preconditions:** Spacecraft is operational; fuel is sufficient; crew minimum is met; no blocking maintenance issue exists; route is valid.  
**Steps:** Run readiness validation and attempt to mark the mission Ready for Departure.  
**Expected result:** The mission can be marked Ready for Departure.

## Mission operations and access

### TPNS-TS-11 — Prevent unauthorized mission status change
**Type:** Security / authorization  
**Preconditions:** Sign in as a user without mission status permissions.  
**Steps:** Attempt to change a mission from Planned to In Progress.  
**Expected result:** The request is denied and the status remains unchanged.

### TPNS-TS-12 — Verify status-change audit history
**Type:** Positive / audit  
**Preconditions:** Authorized user can change mission status.  
**Steps:** Change a mission status and inspect its history.  
**Expected result:** The history contains the actor, previous/new status if supported, and timestamp.

### TPNS-TS-13 — Handle concurrent fuel updates
**Type:** Concurrency / data integrity  
**Preconditions:** Two users update the same spacecraft fuel level concurrently.  
**Steps:** Submit updates that could cause the resulting value to exceed capacity or fall below zero.  
**Expected result:** The system preserves valid fuel limits and handles conflicting updates consistently.

### TPNS-TS-14 — Dashboard reflects a new Critical issue
**Type:** Integration / reporting  
**Preconditions:** A mission dashboard is open and its spacecraft receives a Critical maintenance issue.  
**Steps:** Refresh or reload the dashboard.  
**Expected result:** The issue appears and the readiness state reflects the blocking condition.

## Questions for the AI to identify

- Is a fuel reserve required?
- Which roles can abort or resume missions?
- Can in-progress missions be edited?
- How are overlapping crew assignments handled?
- What happens when telemetry or communication is lost?
