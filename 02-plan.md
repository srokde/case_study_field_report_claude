# Field Service Report Summarizer – Implementation Plan

## Approach

### 1. Specification-Driven Development (Completed)

The specification serves as the authoritative source for implementation and defines:

- Required customer-facing content
- Strict non-publication requirements
- Rules for handling conflicting report data
- Procedures for processing reports with insufficient information
- Prompt-injection prevention requirements

All implementation decisions will align with the documented specification.

---

### 2. Solution Architecture

The solution will be implemented as a lightweight Python CLI and batch-processing utility.

**Input**

- Path to a JSONL file containing service reports

**Output**

- A directory containing Markdown summaries (one file per report)
- A consolidated JSON results index

#### Processing Workflow

1. Load and parse report data.
2. Perform safety screening to identify:
   - Technician identifiers
   - Phone numbers
   - Email addresses
   - Access codes
   - Alarm codes
   - Physical access instructions
   - Personal information
   - Prompt-injection attempts
3. Calculate visit duration and perform consistency checks.
4. Execute report quality validation:
   - Duration mismatch detection
   - Parts-versus-resolution conflict detection
   - Resolution-versus-notes conflict detection
   - Insufficient-information detection
5. Sanitize technician notes by removing or redacting prohibited content.
6. Generate customer-facing summaries using deterministic templates.
7. Perform post-generation validation:
   - Re-scan generated summaries
   - Confirm removal of sensitive information
   - Prevent publication if restricted information remains
8. Generate and store outputs.

---

### 3. Summary Generation Strategy

#### Primary Approach

Customer-facing summaries will be generated using deterministic templates.

The summarizer will construct summaries using structured report fields and sanitized technician notes.

#### Benefits

- Predictable and consistent output
- Simplified testing and validation
- Greater control over published content
- Reduced risk of exposing sensitive or private information

#### Required Summary Sections

- Asset Reference
- Visit Date
- Findings
- Actions Performed
- Parts Installed
- Outstanding or Recommended Actions
- Time on Site

No assumptions or fabricated information will be introduced.

---

### 4. Publication Status Framework

Each report will be assigned one of the following publication statuses.

#### Published

Assigned when sufficient information is available and no blocking issues are identified.

#### Published with Caveat

Assigned when the report contains:

- Duration discrepancies
- Contradictory data fields
- Data quality concerns

In these cases, the summary will clearly explain the identified issue.

#### Insufficient Information

Assigned when the report lacks adequate detail to determine:

- What was found
- What work was completed

The approved follow-up message defined in the specification will be published instead.

#### Blocked for Security Review

Assigned when sensitive information remains after processing and cannot be safely removed.

Publication will be withheld and the report flagged for manual review.

---

### 5. Safety Strategy (Defense-in-Depth)

Protection of customer-facing content takes precedence over completeness.

#### Pre-Generation Filtering

Before summary creation, the system will detect and remove:

- Personal information
- Contact details
- Physical security information
- Prompt-injection attempts

Sensitive content will be removed or replaced with redaction placeholders.

#### Prompt-Injection Protection

Technician notes will be treated exclusively as descriptions of maintenance activities.

Any content attempting to:

- Alter system behavior
- Influence generated output
- Override defined rules
- Conceal information

will be ignored.

#### Post-Generation Validation

Generated summaries will undergo a second safety review.

Validation checks include:

- Personal information
- Contact information
- Access instructions
- Alarm or access codes
- Residual security-sensitive content

Any summary containing prohibited information will be blocked from publication.

---

### 6. Data Quality Management

The implementation will apply all decision rules defined in the specification.

#### Duration Discrepancies

- Calculate time on site using arrival and departure timestamps
- Compare calculated duration with the reported duration
- Use the calculated value as the authoritative duration
- Add a caveat when discrepancies exceed the defined threshold

#### Parts Usage Conflicts

- Prioritize the structured `parts_used` field
- Include a caveat describing the inconsistency

#### Resolution and Notes Conflicts

- Identify conflicting outcomes
- Avoid automatic resolution
- Publish with an explanatory caveat

#### Insufficient Detail

- Never infer missing information
- Publish the approved follow-up message instead

#### Mixed Safe and Sensitive Content

- Remove sensitive information
- Preserve non-sensitive maintenance details whenever possible

---

### 7. Error Handling Strategy

Processing must continue even if individual reports fail validation.

#### Approach

- Validate reports independently
- Record processing errors and warnings
- Continue processing all remaining reports
- Generate outputs for all recoverable records

Malformed timestamps and missing data fields will be handled gracefully and logged appropriately.

---

### 8. Testing Strategy

#### Automated Testing

Test coverage will include:

- Valid reports
- Duration discrepancies
- Parts conflicts
- Resolution conflicts
- Insufficient-information scenarios
- Personal name detection
- Email detection
- Phone number detection
- Access code detection
- Prompt-injection attempts
- Post-generation safety verification

#### Dataset Validation

The solution will be validated against all 20 supplied reports.

**Validation Checklist**

- No names exposed
- No technician identifiers exposed
- No phone numbers exposed
- No email addresses exposed
- No access instructions exposed
- No access codes exposed
- No alarm codes exposed
- Duration caveats applied correctly
- Contradictions reported appropriately
- Insufficient-information reports handled correctly

---

### 9. Git Workflow Discipline

The following commit sequence will be maintained:

1. `01-spec`
2. `02-plan`
3. `03-tasks`
4. `04-implement`

No implementation files will be introduced prior to the implementation phase.

The commit history will serve as evidence of adherence to a Spec-Driven Development process.

---

### 10. AI Usage and Code Review

AI-assisted development may be utilized; however, all generated code will undergo thorough human review before submission.

#### Review Focus Areas

- Alignment with requirements
- Test coverage
- Security considerations
- Performance
- Maintainability

At least one legitimate issue will be identified, corrected, and documented as part of the review process.

The review will evaluate both:

- Technical implementation quality
- Safety and suitability of customer-facing outputs

---

### 11. Final Deliverables

The completed solution will include:

- A working report summarization tool
- Markdown summaries for all reports
- Consolidated JSON output
- Analysis of problematic or exceptional reports
- AI review findings and documented corrections
- Documentation of specification-driven decisions
- Git history demonstrating the required commit sequence
- Demonstration notes
- A declared-effort statement
