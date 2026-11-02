# Connected Digital Universe — Architecture

This document explains what the public demo actually does and how the surrounding system is designed.

## Product idea

A small interactive map for understanding a digital product as connected layers instead of isolated tools. The globe is the visual anchor; selecting a node reveals its role in the system.

## What is implemented

The live demo is a self-contained static site:

- index.html contains the page structure, styles, and interaction logic.
- CSS draws the visual globe, continent shapes, orbit lines, responsive cards, and the information panel.
- Canvas renders the star field, latitude/longitude curves, surface points, network links, and orbiting connection cues.
- JavaScript handles drag rotation, tilt, zoom, node selection, and accessible selected-state updates.
- GitHub Pages serves the site from the repository root.

There is no server, live API, or production database behind this demo. The database, API, file, cloud, and security nodes describe a system architecture concept; they do not claim those services are already running.

## Conceptual system map

~~~mermaid
flowchart LR
  User[Visitor] --> UI[Web interface]
  UI --> Files[Application modules and assets]
  UI --> API[Service API]
  API --> Auth[Authentication and validation]
  Auth --> Data[Data access layer]
  Data --> DB[(Database)]
  UI --> CDN[Static assets and CDN]
  API --> Deploy[Application deployment]
  DB --> Backup[Backup and recovery]
  Monitor[Logs and monitoring] --- UI
  Monitor --- API
  Monitor --- DB
~~~

## Suggested application layout

This is a proposed structure for growing the concept into a full application. It is not the file tree of the current static demo.

~~~text
app/
  page.tsx
  api/
    architecture/route.ts
    health/route.ts
components/
  GlobeCanvas.tsx
  ArchitectureNode.tsx
  DetailPanel.tsx
lib/
  architecture-data.ts
  validation.ts
  db.ts
  auth.ts
  logger.ts
public/
  icons/
  images/
~~~

## Data model proposal

A relational database could represent the map with three core tables:

- systems: identity, name, description, and visibility.
- nodes: system ID, stable key, label, node type, position, and metadata.
- connections: source node, destination node, relationship type, and direction.

Important constraints: node keys are unique within a system; both ends of a connection must belong to that system; deleting a node must not leave orphaned connections.

## API contract proposal

- GET /api/architecture — return the published nodes and connections.
- GET /api/architecture/:id — return one system with its nodes and links.
- GET /api/health — report service readiness without exposing secrets or internal configuration.

Write endpoints should be added only with authentication, input validation, rate limiting, and audit logging.

## Security and operations

- Keep credentials and environment variables out of source control.
- Validate and normalize all user-controlled input at the API boundary.
- Use least-privilege database credentials and parameterized queries.
- Add request limits, security headers, dependency updates, and backup/restore procedures before production use.
- Monitor errors and latency while avoiding personal data in logs.
- Keep static assets cacheable and versioned; deploy changes through a reviewed commit.

## Interaction and accessibility

- Each architecture node is a real button with keyboard support and an announced selected state.
- The canvas is decorative; essential labels and details stay in regular HTML.
- The layout adapts for narrow screens and honors reduced-motion preferences.
- Dragging rotates the scene, the wheel adjusts scale, and selecting a node updates its detail panel.
