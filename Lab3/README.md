# Lab 3 Component Modelling and Architectural Pattern Selection

This submission models the **Micro-Lending and Peer Credit Risk Assessor** project documented in Lab 2. The layered design separates the client portal, loan coordination, credit-risk assessment, repayment processing, authorization, and financial persistence behind explicit interfaces.

## Deliverables

- `Component_Diagram.png` - submission-ready UML component diagram
- `Component_Diagram.pdf` - vector-style PDF export of the diagram
- `Architecture_Justification.docx` - one-page editable Word justification
- `Architecture_Justification.pdf` - one-page submission PDF
- `Component_Diagram.drawio` - native editable draw.io source
- `source/build_justification.py` - reproducible Word-document source

## Architecture decision

Layered architecture is the best fit for the current project scope. It separates financial workflows while keeping repayment and loan-state changes within one ACID transaction boundary. Microservices would allow independent scaling, but would add distributed transaction and deployment complexity. Client-server architecture centralizes rules and data, but does not provide the same internal separation of concerns by itself.

## Components

1. Borrower and Lender Portal
2. Loan Management Service
3. Credit Risk Engine
4. Repayment Service
5. Authorization Service
6. Financial and Loan Repository

## Interfaces

| Consumer | Provider | Interface | Technology and data |
|---|---|---|---|
| Borrower and Lender Portal | Loan Management Service | Loan Management API | HTTPS/JSON carrying profiles, loan requests, and repayment views |
| Loan Management Service | Credit Risk Engine | Risk Assessment API | In-process calls carrying validated profiles and risk results |
| Loan Management Service | Repayment Service | Repayment API | Service calls carrying loan terms, schedules, payments, and default status |
| Loan Management Service | Authorization Service | Authorization API | Middleware calls carrying identity, role, and permission decisions |
| Loan Management Service | Financial and Loan Repository | Financial Profile Repository | Parameterized SQL for borrower profiles and loans |
| Repayment Service | Financial and Loan Repository | Repayment Transaction Repository | ACID SQL transactions for repayment records and commit status |

The diagram uses a socket at the consuming component for each required interface and a circle at the providing component for each provided interface. Arrowheads show request-data direction; responses return over the same assembly connector.

The PNG and PDF are exported directly from the native `.drawio` source using draw.io Desktop. They use draw.io's UML component, provided-interface, and required-interface shapes with orthogonal connectors.
