# Software Requirements Specification (SRS)

**Group Travel Expense Simplification Engine** | Problem Statement #37
Name: M Stuthi | SRN: PES1UG24CS283 | Section: E | PES University, Dept. of CSE

## 1. Introduction

### 1.1 Purpose
Specifies the functional and non-functional requirements of the Group Travel Expense Simplification Engine (GTES), a multi-currency expense-sharing utility.

### 1.2 Scope
GTES records uneven group expenses, converts foreign-currency amounts to the trip base currency using daily exchange rates, and computes a minimum-transaction debt settlement graph. Out of scope: bank account management, receipt OCR, tax/VAT handling, and card issuing.

### 1.3 Definitions

| Term | Meaning |
|---|---|
| Group Traveler | Participant who logs expenses, views balances, settles payments |
| Trip Admin | Creates trip, manages members, maintains exchange rates |
| Base currency | Currency in which all balances and settlements are expressed |
| Net balance | Total paid minus total owed by a traveler |
| Settlement graph | Set of payer-to-payee edges that brings all net balances to zero |
| RTM | Requirements Traceability Matrix (`1-RE/RTM.md`) |

### 1.4 References
Problem statement #37; `1-RE/Requirements_Table.pdf`; `1-RE/UseCase_Diagram.pdf`; `1-RE/UseCase_Flow.docx`; `1-RE/Alternate_Flow.docx`; IEEE Std 830 / ISO/IEC/IEEE 29148 structure.

## 2. Overall Description

### 2.1 Product Perspective
Standalone web/mobile application with a layered architecture (`2-Architectural-Diagram/architecture.png`). Two external systems: Exchange Rate Service (daily rates) and Payment Gateway (payment confirmation).

### 2.2 User Classes

| Actor | Responsibilities |
|---|---|
| Group Traveler | Log expenses (UC-02, UC-08), view balances (UC-04), compute settlement (UC-05), record payments (UC-06) |
| Trip Admin | Create trip and manage members (UC-01, UC-09); supply or correct exchange rates; may do everything a Group Traveler can |
| Exchange Rate Service (external) | Provides daily exchange rates (UC-03) |
| Payment Gateway (external) | Confirms payments (UC-07) |

### 2.3 Operating Environment
Modern web browser and mobile client; server-side REST API; relational database; HTTPS to external services.

### 2.4 Constraints
- All monetary values stored as fixed-point decimals, never binary floating point.
- Conversion rate is the rate for the expense date.
- Settlement computed in the trip base currency only.

### 2.5 Assumptions and Dependencies
- A trip has at least two travelers before a settlement is meaningful.
- Exchange Rate Service availability is not guaranteed; the missing-rate alternate flow handles gaps.
- Payment Gateway is optional: a payment can be marked Paid manually (UC-06) or confirmed via the gateway (UC-07).

## 3. Specific Requirements

### 3.1 Functional Requirements

| ID | Description | Priority | Acceptance Criteria |
|---|---|---|---|
| FR-001 | The system shall allow a Trip Admin to create a trip and add Group Travelers as participants, each identified by name and a base currency. | High | Pass: new trip lists all added travelers with correct base currencies. Fail: any traveler missing or with wrong currency. |
| FR-002 | The system shall allow a Group Traveler to log an expense with amount, payer, currency, date, category, and a list of participants sharing the cost (equal or custom split). | High | Pass: logged expense shows exact amount, payer, split. Fail: split total differs from amount. |
| FR-003 | The system shall convert any foreign-currency expense into the trip base currency using the daily exchange rate in effect on the expense date. | High | Pass: converted amount = amount x that day's rate, within 0.01 currency units. Fail: stale or incorrect rate used. |
| FR-004 | The system shall compute the optimal debt settlement graph using min-flow algorithms to minimize total peer-to-peer payment transactions. | High | Pass: balances settle to net zero with minimal payment edges. Fail: sum of debts differs from sum of credits. |
| FR-005 | The system shall allow a Group Traveler to mark a suggested settlement payment as Paid, updating outstanding balances in real time for all trip members. | Medium | Pass: payer and payee balances update within 2 s and are visible to all members. Fail: balance unchanged or not reflected for others. |

### 3.2 Non-Functional Requirements

| ID | Type | Description | Priority | Acceptance Criteria |
|---|---|---|---|---|
| NFR-001 | Performance & Security | The settlement engine shall execute in under 100 ms for up to 50 travelers and 500 expenses. | High | Pass: benchmarks confirm target latency and security standards under simulated peak load. |
| NFR-002 | Reliability & Data Integrity | Currency conversion and settlement computations shall be deterministic and auditable: recomputing from the same expense data always yields an identical result. | Medium | Pass: byte-identical debt/credit values in 100/100 reruns on unchanged data. |

### 3.3 Use Cases

| ID | Name | Actor | Relationship |
|---|---|---|---|
| UC-01 | Create Trip & Add Travelers | Trip Admin | Primary |
| UC-02 | Log Expense | Group Traveler | Primary |
| UC-03 | Convert Currency | Exchange Rate Service | Included by UC-05 |
| UC-04 | View Trip Balances | Group Traveler | Primary |
| UC-05 | Compute Settlement Graph | Group Traveler, Trip Admin | Primary, includes UC-03 |
| UC-06 | Record Settlement Payment | Group Traveler | Primary |
| UC-07 | Process Payment via Gateway | Payment Gateway | Extends UC-06 |
| UC-08 | Apply Custom Split | Group Traveler | Extends UC-02 |
| UC-09 | Manage Trip Members | Trip Admin | Primary |

Detailed flow for UC-05 (main success scenario, preconditions, postconditions) is in `1-RE/UseCase_Flow.docx`; alternate flow 4a (missing exchange rate) is in `1-RE/Alternate_Flow.docx`.

### 3.4 Business Rules

| ID | Rule |
|---|---|
| BR-01 | Sum of all net balances in a trip equals zero. |
| BR-02 | An expense's split amounts sum exactly to its total. |
| BR-03 | An expense with no available rate is excluded from settlement and flagged until a rate is entered. |
| BR-04 | Only a Trip Admin can create a trip, manage members, or edit exchange rates. |

### 3.5 External Interfaces

| Interface | Direction | Data |
|---|---|---|
| Exchange Rate Service | Inbound | Currency pair, date, rate |
| Payment Gateway | Outbound/Inbound | Payment request, confirmation status |
| User interface | Bidirectional | Forms for trip, expense, payment; views for balances and settlement |

### 3.6 Data Entities (logical)

Trip(id, name, base_currency); Member(id, trip_id, name, currency, role); Expense(id, trip_id, payer_id, amount, currency, date, category); Split(expense_id, member_id, share); ExchangeRate(currency, date, rate); Settlement(id, trip_id, payer_id, payee_id, amount, status).

## 4. Requirement Traceability
See `1-RE/RTM.md`.
