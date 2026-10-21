# El Dorado Exploration System — Test Scenarios

**Prefix:** EES  
**Purpose:** Evaluate validation, workflow, inventory integrity, search, and role-based access scenarios.

## Explorer and expedition management

### EES-TS-01 — Create an explorer with a unique ID
**Type:** Positive  
**Preconditions:** Administrator is signed in.  
**Steps:** Enter a unique explorer ID and valid required fields; save.  
**Expected result:** The explorer is created and appears in the registry.

### EES-TS-02 — Reject a duplicate expedition code
**Type:** Negative / validation  
**Preconditions:** An expedition with the submitted code exists.  
**Steps:** Create another expedition using the same code.  
**Expected result:** The system rejects the duplicate code.

### EES-TS-03 — Reject invalid expedition dates
**Type:** Negative / boundary  
**Preconditions:** User can create an expedition.  
**Steps:** Set the planned end date earlier than the planned start date and save.  
**Expected result:** The system rejects the date range and explains the validation error.

### EES-TS-04 — Prevent assignment of a deactivated explorer
**Type:** Negative / business rule  
**Preconditions:** An explorer is deactivated.  
**Steps:** Attempt to assign that explorer to a new expedition.  
**Expected result:** The system rejects the assignment.

## Artifact catalog and review

### EES-TS-05 — Submit an artifact linked to an existing expedition
**Type:** Positive  
**Preconditions:** An active expedition exists and the user is assigned to it.  
**Steps:** Submit a complete artifact record with a unique ID and valid expedition reference.  
**Expected result:** The artifact is saved with review status Pending.

### EES-TS-06 — Reject an artifact linked to a nonexistent expedition
**Type:** Negative / data integrity  
**Preconditions:** User can submit artifact records.  
**Steps:** Submit an artifact with an invalid expedition reference.  
**Expected result:** The system rejects the record.

### EES-TS-07 — Reject an artifact without a rejection reason
**Type:** Negative / workflow  
**Preconditions:** An artifact is Pending and the user is a Curator.  
**Steps:** Select Reject without entering a reason.  
**Expected result:** The system requires a reason and does not save the rejected status.

### EES-TS-08 — Prevent an Explorer from approving an artifact
**Type:** Security / authorization  
**Preconditions:** Sign in as an Explorer.  
**Steps:** Attempt to approve a Pending artifact, including through a direct request if applicable.  
**Expected result:** The action is denied and the artifact remains Pending.

## Inventory and supply requests

### EES-TS-09 — Reject a stock adjustment that would make inventory negative
**Type:** Negative / boundary  
**Preconditions:** An item has a known available quantity.  
**Steps:** Attempt to remove more units than are available.  
**Expected result:** The adjustment is rejected and the quantity remains unchanged.

### EES-TS-10 — Process two stock reductions at the same time
**Type:** Concurrency / data integrity  
**Preconditions:** An item has enough stock for only one of two simultaneous reductions.  
**Steps:** Submit both reductions concurrently.  
**Expected result:** The system accepts only valid transactions and never records negative stock.

### EES-TS-11 — Reject a supply request without a rejection reason
**Type:** Negative / workflow  
**Preconditions:** A supply request is Submitted and the user can review it.  
**Steps:** Reject the request without a reason.  
**Expected result:** The system requires a reason and does not save the rejection.

### EES-TS-12 — Search discovery logs by expedition and date
**Type:** Positive / search  
**Preconditions:** Discovery entries exist across multiple expeditions and dates.  
**Steps:** Filter by one expedition and a date range.  
**Expected result:** Only matching entries are returned. Date-boundary behavior should follow a clarified requirement.

### EES-TS-13 — Attempt to add a discovery to a completed expedition
**Type:** Negative / business rule  
**Preconditions:** An expedition is Completed.  
**Steps:** Attempt to add a new discovery log entry.  
**Expected result:** The system blocks the action unless an explicit authorized exception has been defined.

## Questions for the AI to identify

- Can explorers be assigned to multiple active expeditions?
- Can approved artifacts be edited?
- Are inventory quantities integers or decimals?
- Who approves supply requests?
- What happens to outstanding requests when an expedition is cancelled?
