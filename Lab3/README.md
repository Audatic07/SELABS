# Lab 3 Component Modelling and Architectural Pattern Selection

This submission models a self-service coffee kiosk using a layered architecture. The design separates the touchscreen, ordering logic, payment processing, menu data, and receipt-printer integration so each responsibility is replaceable behind a clear interface.

## Deliverables

- `Component_Diagram.png` - submission-ready UML component diagram
- `Component_Diagram.pdf` - vector-style PDF export of the diagram
- `Architecture_Justification.docx` - one-page editable Word justification
- `Architecture_Justification.pdf` - one-page submission PDF
- `Component_Diagram.html` - editable diagram source
- `source/build_justification.py` - reproducible Word-document source

## Architecture decision

Layered architecture is the best fit for the stated single-kiosk scope. It preserves clear presentation, business, and data/device boundaries while avoiding the operational complexity and network latency of microservices. A client-server design could become useful for a future multi-kiosk fleet, but it would introduce a server dependency that the supplied scenario does not require.

## Components

1. Touchscreen UI
2. Order Manager
3. Payment Service
4. Menu Repository
5. Receipt Printer Adapter

## Interfaces

| Consumer | Provider | Interface | Technology and data |
|---|---|---|---|
| Touchscreen UI | Order Manager | Order API | In-process calls carrying selections and order state |
| Order Manager | Payment Service | Payment API | Service call carrying payment request and authorization result |
| Order Manager | Menu Repository | Menu Query Interface | SQL query returning coffee types, sizes, and prices |
| Order Manager | Receipt Printer Adapter | Printer Port | USB or driver call carrying formatted receipt data |

The diagram uses a socket at the consuming component for each required interface and a circle at the providing component for each provided interface. Arrowheads show request-data direction; responses return over the same assembly connector.
