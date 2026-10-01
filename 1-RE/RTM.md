# Requirements Traceability Matrix (RTM)

**Problem Statement #37: Group Travel Expense Simplification Engine**
Name: M Stuthi | SRN: PES1UG24CS283 | Section: E

## 1. Forward Traceability: Requirement to Use Case, Component, Test

| Req ID | Requirement (short) | Type | Priority | Actor(s) | Use Case(s) | Architecture Component | Test Case(s) | Status |
|---|---|---|---|---|---|---|---|---|
| FR-001 | Create trip, add travelers with name and base currency | Functional | High | Trip Admin | UC-01, UC-09 | Trip & Member Service | TC-001, TC-002 | Specified |
| FR-002 | Log expense (amount, payer, currency, date, category, equal/custom split) | Functional | High | Group Traveler | UC-02, UC-08 | Expense Service | TC-003, TC-004 | Specified |
| FR-003 | Convert foreign-currency expense using daily rate on expense date | Functional | High | Group Traveler, Exchange Rate Service | UC-03 | Currency Conversion | TC-005, TC-006 | Specified |
| FR-004 | Compute min-transaction debt settlement graph (min-flow) | Functional | High | Group Traveler, Trip Admin | UC-04, UC-05 | Settlement Engine | TC-007, TC-008 | Specified |
| FR-005 | Mark suggested payment as Paid; balances update for all members | Functional | Medium | Group Traveler, Payment Gateway | UC-06, UC-07 | Payment Service | TC-009, TC-010 | Specified |
| NFR-001 | Settlement computed in under 100 ms for 50 travelers / 500 expenses | Performance & Security | High | n/a (system-wide) | UC-05 | Settlement Engine | TC-011 | Specified |
| NFR-002 | Deterministic, auditable conversion and settlement (identical output on rerun) | Reliability & Data Integrity | Medium | n/a (system-wide) | UC-03, UC-05 | Currency Conversion, Settlement Engine | TC-012 | Specified |

## 2. Backward Traceability: Use Case to Requirement

| Use Case | Name | Relationship | Requirement(s) |
|---|---|---|---|
| UC-01 | Create Trip & Add Travelers | Primary | FR-001 |
| UC-02 | Log Expense | Primary | FR-002 |
| UC-03 | Convert Currency | Included by UC-05 | FR-003, NFR-002 |
| UC-04 | View Trip Balances | Primary | FR-004 |
| UC-05 | Compute Settlement Graph | Primary (includes UC-03) | FR-004, NFR-001, NFR-002 |
| UC-06 | Record Settlement Payment | Primary | FR-005 |
| UC-07 | Process Payment via Gateway | Extends UC-06 | FR-005 |
| UC-08 | Apply Custom Split | Extends UC-02 | FR-002 |
| UC-09 | Manage Trip Members | Primary | FR-001 |

## 3. Test Case Reference

| TC ID | Verifies | Pass Condition |
|---|---|---|
| TC-001 | FR-001 | New trip lists all added travelers with correct base currencies |
| TC-002 | FR-001 | Missing or wrong currency on a traveler is rejected or flagged |
| TC-003 | FR-002 | Logged expense shows exact amount, payer, split |
| TC-004 | FR-002 | Expense whose split total differs from amount is rejected |
| TC-005 | FR-003 | Converted amount = amount x rate for expense date, within 0.01 |
| TC-006 | FR-003 | Stale or wrong-date rate is never applied |
| TC-007 | FR-004 | Net balances sum to zero; payment edge count is minimal |
| TC-008 | FR-004 | Sum of debts equals sum of credits |
| TC-009 | FR-005 | Payer and payee balances update within 2 s |
| TC-010 | FR-005 | Update is visible to all trip members |
| TC-011 | NFR-001 | Under 100 ms at 50 travelers / 500 expenses under simulated peak load |
| TC-012 | NFR-002 | 100/100 reruns on unchanged data give byte-identical output |

## 4. Coverage Check

- 7/7 requirements map to at least one use case, one component, and one test case.
- 9/9 use cases map to at least one requirement.
- Alternate flow (missing exchange rate, UC-05 step 4a) traces to FR-003.
