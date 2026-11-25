# Automation Rules - MarketFlow AI

## Goal

Define what can be automated and what must always go through human review.

## Main MVP rule

In the MVP, no answer is published automatically.

The AI only suggests answers.

## Cases that always require human review

- Questions about payments.
- Payment confirmation.
- Price changes.
- Discounts.
- Complaints.
- Returns.
- Warranties.
- Changes to commercial terms.
- Unavailable information.
- Upset customers.
- Delivery promises.
- Legal or sensitive cases.

## Candidates for future automation

These could only be automated later if the information is available and the risk is low:

- Simple availability questions.
- Simple feature questions.
- Simple payment-method questions.
- Simple questions about already-configured delivery times.
- Simple product-condition questions.

## Minimum conditions to automate in the future

An answer could be published automatically only if:

- The question type is low risk.
- The information exists in the database.
- The confidence level is high.
- There are no contradictions.
- It is not about payments, complaints, warranties or returns.
- The automation rule is active.
- History shows that similar answers were approved.

## Risk levels

### Low

Simple, objective questions.

Examples:

- Is it available?
- Is it new?
- What color is it?
- Do you ship to Bogotá?

### Medium

Questions that depend on up-to-date data or commercial terms.

Examples:

- How much is shipping?
- How long does it take?
- Does it have a warranty?
- Do you accept a certain payment method?

### High

Sensitive questions or ones with commercial/legal impact.

Examples:

- Complaints.
- Returns.
- Payments.
- Price changes.
- Discounts.
- Delivery problems.

## Recommended evolution

### Phase 1

100% human review.

### Phase 2

Simulated automation, without publishing.

### Phase 3

Partial automation for low risk.

### Phase 4

Automation with high confidence and monitoring.

### Phase 5

Almost full automation with alerts and auditing.
