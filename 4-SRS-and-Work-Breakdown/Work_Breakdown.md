# Work Breakdown Structure (WBS)

**Group Travel Expense Simplification Engine** | M Stuthi | PES1UG24CS283 | Section E

Effort is in hours (individual project). Jira projects: Scrum `SBPS37`, Kanban `KBPS37`, Bug Tracking `BTB`.

| WBS ID | Work Package | Deliverable | Effort (h) | Status |
|---|---|---|---|---|
| **1** | **Project Initiation** | | **2** | |
| 1.1 | Create GitHub repo and folder structure | Repo with 4 folders + README | 1 | Done |
| 1.2 | Create Jira project, epics, backlog | Jira board | 1 | Done |
| **2** | **Requirements Engineering** | | **9** | |
| 2.1 | Identify actors and use cases | Actor/use-case list | 1 | Done |
| 2.2 | Write 5 FRs and 2 NFRs | Requirements table | 3 | Done |
| 2.3 | Draw UML use-case diagram (include/extend) | UseCase_Diagram.pdf | 2 | Done |
| 2.4 | Write UC-05 flow and alternate flow 4a | UseCase_Flow.docx, Alternate_Flow.docx | 2 | Done |
| 2.5 | Build RTM | RTM.md | 1 | Done |
| **3** | **Architecture Design** | | **3** | |
| 3.1 | Define layers and services, map to UCs | Component list | 1 | Done |
| 3.2 | Draw architecture diagram | architecture.svg/png | 2 | Done |
| **4** | **Documentation** | | **4** | |
| 4.1 | Write SRS | SRS.md | 3 | Done |
| 4.2 | Write WBS | Work_Breakdown.md | 1 | Done |
| **5** | **Submission** | | **2** | |
| 5.1 | Capture GitHub and Jira screenshots | Folder 3 files | 1 | Done |
| 5.2 | Final review, consistency check, push | Tagged commit | 1 | To do |
| **6** | **Implementation (planned)** | | **38** | |
| 6.1 | Data model and DB schema | Schema + migrations | 4 | Planned |
| 6.2 | Trip & Member Service (FR-001) | API + tests | 5 | Planned |
| 6.3 | Expense Service with custom split (FR-002) | API + tests | 6 | Planned |
| 6.4 | Currency Conversion + rate ingestion (FR-003) | Service + tests | 6 | Planned |
| 6.5 | Settlement Engine, min-flow (FR-004) | Algorithm + tests | 8 | Planned |
| 6.6 | Payment Service and real-time updates (FR-005) | Service + tests | 6 | Planned |
| 6.7 | Benchmark and determinism tests (NFR-001, NFR-002) | Test reports | 3 | Planned |
| **Total** | | | **58** | |
