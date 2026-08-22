# Field Service Report Summarizer – Implementation Tasks

## Objective

Implement the solution outlined in `spec.md` by following the architecture and design guidance defined in `plan.md`.

---

## Task 1: Report Ingestion and Parsing

### Purpose

Load and validate all field service reports from the JSONL source file.

### Activities

- Read the input file sequentially, one record at a time.
- Parse each JSON object.
- Verify the presence of all mandatory fields.
- Handle malformed or invalid records gracefully.
- Continue processing even if an individual record fails validation.

### Completion Criteria

- All valid reports are successfully loaded.
- Invalid records are identified and reported without interrupting batch processing.

---

## Task 2: Sensitive Information Detection

### Purpose

Identify content that must never be exposed in customer-facing summaries.

### Activities

- Detect phone numbers.
- Detect email addresses.
- Detect technician identifiers.
- Detect personal names and contact references.
- Detect access codes.
- Detect alarm codes.
- Detect key storage or key location information.
- Detect physical access instructions.
- Detect prompt injection attempts and manipulation phrases.

### Completion Criteria

- Sensitive information is reliably detected before summary generation.

---

## Task 3: Note Sanitization

### Purpose

Ensure restricted or confidential information cannot reach published outputs.

### Activities

- Replace phone numbers with redaction placeholders.
- Replace email addresses with redaction placeholders.
- Remove or anonymize names when necessary.
- Eliminate access codes and security-related details.
- Remove prompt injection content.
- Retain useful maintenance and service information wherever possible.

### Completion Criteria

- Sanitized notes contain only information that is safe for customer consumption.

---

## Task 4: Visit Duration Calculation

### Purpose

Provide accurate customer-facing information regarding time spent on-site.

### Activities

- Calculate visit duration using arrival and departure timestamps.
- Cross-check the calculated duration against the reported duration.
- Identify discrepancies that exceed the defined specification threshold.
- Generate explanatory caveats when required.

### Completion Criteria

- Duration details are available for every valid report.

---

## Task 5: Report Consistency Validation

### Purpose

Highlight data-quality issues rather than silently resolving them.

### Activities

#### Duration Variance

- Compare reported and calculated durations.

#### Parts Usage Conflicts

- Detect cases where parts are recorded while the resolution states that no parts were used.

#### Resolution and Notes Conflicts

- Identify situations where repair outcomes described in technician notes contradict the resolution status.

### Completion Criteria

- All detected contradictions are flagged and reflected in the output status.

---

## Task 6: Insufficient Information Detection

### Purpose

Prevent the generation of inaccurate or misleading customer summaries.

### Activities

- Assess the available report information.
- Determine whether the following can be clearly described:
  - What was identified
  - What actions were taken
- Identify reports that lack the information required for a reliable summary.

### Completion Criteria

- Reports with insufficient detail use the approved follow-up messaging.

---

## Task 7: Customer-Facing Summary Generation

### Purpose

Create clear and understandable summaries for facilities and customer contacts.

### Activities

Generate summaries containing:

- Asset reference
- Visit date
- Findings
- Work completed
- Parts installed
- Outstanding recommendations
- Time spent on-site

### Guidelines

- Use clear, non-technical language where practical.
- Do not introduce assumptions or unsupported facts.
- Prioritize structured data when conflicts exist.
- Include caveats whenever required.

### Completion Criteria

- Each report produces either a valid summary or an approved fallback message.

---

## Task 8: Publication Status Assignment

### Purpose

Assign the correct publication outcome to every report.

### Activities

Support the following statuses:

- Published
- Published With Caveat
- Insufficient Information
- Blocked for Security Review

### Completion Criteria

- Every processed report receives a publication status.

---

## Task 9: Post-Generation Validation

### Purpose

Confirm that generated summaries remain compliant and secure after creation.

### Activities

- Re-scan generated summaries.
- Verify the absence of:
  - Personal names
  - Phone numbers
  - Email addresses
  - Technician identifiers
  - Access-related information
  - Security-sensitive content

### Completion Criteria

- Any unsafe summary is blocked before publication.

---

## Task 10: Output Generation

### Purpose

Produce all required deliverables.

### Activities

- Generate a separate Markdown file for each report.
- Generate a consolidated JSON output.
- Include the following fields:
  - `report_id`
  - `publication_status`
  - `summary`
  - `flags`

### Completion Criteria

- Output files are generated for every processed report.

---

## Task 11: Automated Testing

### Purpose

Validate compliance with the project specification.

### Test Scenarios

- Standard report processing
- Duration mismatch detection
- Parts contradiction detection
- Resolution conflict detection
- Insufficient information handling
- Prompt injection detection
- Phone number redaction
- Email address redaction
- Access code redaction
- Physical security information removal
- Post-generation validation checks

### Completion Criteria

- All critical and high-risk scenarios are adequately tested.

---

## Task 12: Dataset Validation

### Purpose

Verify behaviour using the supplied report dataset.

### Activities

- Process all 20 reports.
- Perform a manual review of every generated output.
- Confirm that:
  - No sensitive information has been exposed.
  - Caveats are applied correctly.
  - Contradictions are properly highlighted.
  - Reports with insufficient detail use the approved wording.

### Completion Criteria

- All outputs meet the specification requirements.

---

## Task 13: AI-Assisted Output Review

### Purpose

Review the AI-assisted implementation before final submission.

### Review Focus Areas

#### Requirements Alignment

- Does the implementation satisfy the specification?

#### Test Coverage

- Are all significant scenarios covered?

#### Security

- Is there any path through which restricted information could be published?

#### Performance

- Can the solution efficiently process all reports?

#### Maintainability

- Is the codebase clear, maintainable, and easy to extend?

### Deliverables

- Review report.
- Documentation of at least one genuine issue.
- Evidence that corrective action was implemented.

### Completion Criteria

- Findings are documented and all required fixes have been applied.

---

# Final Submission Checklist

- [ ] Specification committed as `01-spec`
- [ ] Architecture plan committed as `02-plan`
- [ ] Task breakdown committed as `03-tasks`
- [ ] Implementation committed as `04-implement`
- [ ] All reports processed successfully
- [ ] No restricted information published
- [ ] Inconsistencies correctly identified and surfaced
- [ ] Insufficient-information scenarios handled appropriately
- [ ] AI review completed
- [ ] Delivery package prepared for submission
