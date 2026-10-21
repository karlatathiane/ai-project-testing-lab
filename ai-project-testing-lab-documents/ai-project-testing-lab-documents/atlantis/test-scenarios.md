# Atlantis Research & Preservation — Test Scenarios

**Prefix:** ARP  
**Purpose:** Evaluate environmental threshold logic, alert lifecycle, artifact workflows, equipment constraints, and authorization.

## Expeditions and artifacts

### ARP-TS-01 — Create an expedition with valid data
**Type:** Positive  
**Preconditions:** Authorized user is signed in.  
**Steps:** Enter a unique expedition ID, name, site, valid dates, and lead; save.  
**Expected result:** The expedition is created.

### ARP-TS-02 — Reject an invalid expedition date range
**Type:** Negative / boundary  
**Preconditions:** User can create expeditions.  
**Steps:** Set the end date earlier than the start date and save.  
**Expected result:** The system rejects the date range.

### ARP-TS-03 — Register an artifact linked to an existing expedition
**Type:** Positive  
**Preconditions:** An expedition exists.  
**Steps:** Submit an artifact with a unique ID, required details, and valid expedition reference.  
**Expected result:** The artifact is saved and linked to the expedition.

### ARP-TS-04 — Reject an artifact with a nonexistent expedition reference
**Type:** Negative / data integrity  
**Preconditions:** User can register artifacts.  
**Steps:** Submit an artifact using an unknown expedition ID.  
**Expected result:** The system rejects the record.

### ARP-TS-05 — Prevent self-approval of an artifact
**Type:** Security / workflow  
**Preconditions:** A researcher has registered an artifact and also has access to review functions, if the interface exposes them.  
**Steps:** Attempt to approve the same artifact as its registering researcher.  
**Expected result:** The system blocks self-approval unless an explicitly approved exception exists.

## Environmental readings and alerts

### ARP-TS-06 — Accept an environmental reading within configured limits
**Type:** Positive  
**Preconditions:** Site and threshold configuration exist.  
**Steps:** Submit a reading within all configured sensor and alert limits.  
**Expected result:** The reading is stored with its timestamp and site; no threshold-breach alert is created.

### ARP-TS-07 — Reject a reading outside sensor limits
**Type:** Negative / boundary  
**Preconditions:** Sensor input limits are configured.  
**Steps:** Submit a reading outside the permitted sensor range.  
**Expected result:** The system rejects the reading with a validation message.

### ARP-TS-08 — Create an alert when a reading breaches a threshold
**Type:** Positive / integration  
**Preconditions:** A site has a configured threshold.  
**Steps:** Submit a valid sensor reading beyond that threshold.  
**Expected result:** A safety alert is created with site, measurement, observed value, threshold, timestamp, and status.

### ARP-TS-09 — Check reading exactly equal to a threshold
**Type:** Boundary / ambiguity  
**Preconditions:** A threshold is configured.  
**Steps:** Submit a reading exactly equal to the threshold.  
**Expected result:** The system behavior should match the agreed threshold rule. If no rule exists, the AI should flag the requirement as ambiguous rather than inventing expected behavior.

### ARP-TS-10 — Reject alert resolution without notes
**Type:** Negative / workflow  
**Preconditions:** An open safety alert exists and the user is an authorized Safety Officer.  
**Steps:** Attempt to resolve the alert without resolution notes.  
**Expected result:** The system requires notes and leaves the alert unresolved.

### ARP-TS-11 — Prevent unauthorized threshold changes
**Type:** Security / authorization  
**Preconditions:** Sign in as a Researcher without threshold configuration permission.  
**Steps:** Attempt to change a configured threshold, including by direct request if applicable.  
**Expected result:** The action is denied and the threshold remains unchanged.

### ARP-TS-12 — Handle duplicate environmental readings
**Type:** Data integrity / ambiguity  
**Preconditions:** A reading has already been received from a site or sensor.  
**Steps:** Submit the same reading again with the same timestamp and source, where source identity is available.  
**Expected result:** The system follows a documented duplicate-handling rule. If no such rule exists, the AI should identify the missing requirement.

## Equipment and research logs

### ARP-TS-13 — Prevent assignment of equipment under maintenance
**Type:** Negative / business rule  
**Preconditions:** Equipment status is Maintenance.  
**Steps:** Attempt to assign it to a new expedition.  
**Expected result:** The system rejects the assignment.

### ARP-TS-14 — Search research logs by expedition and date
**Type:** Positive / search  
**Preconditions:** Research logs exist across multiple expeditions and dates.  
**Steps:** Filter logs by expedition and date range.  
**Expected result:** Only matching logs are returned.

### ARP-TS-15 — Verify audit history for a threshold change
**Type:** Positive / audit  
**Preconditions:** Safety Officer has permission to change thresholds.  
**Steps:** Change a threshold and inspect the audit history.  
**Expected result:** The change records the actor and timestamp, along with the changed value if supported.

## Questions for the AI to identify

- What are the measurement units and default threshold values?
- Does a reading equal to a threshold trigger an alert?
- How should duplicate or out-of-order readings be handled?
- What is the required response time for Critical alerts?
- Can equipment be assigned to multiple expeditions simultaneously?
