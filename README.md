# Net-Mapper: Enterprise Operator & Reference Manual

Welcome to the official technical guide and documentation for **Net-Mapper**, the professional-grade static network visualization tool designed for Security Operations Centers (SOC), network engineering teams, and penetration testing specialists.

---

# 📖 Table of Contents

1. System Overview
2. Interface Anatomy
3. Operator Workflow & Controls
4. Routing Diagnostics (Dijkstra Engine)
5. Standard Schema & Database Structures
6. Deployment Procedures
7. Operational Security (OPSEC) Posture

---

# 💻 System Overview

Net-Mapper is a zero-dependency, client-side application designed to render complex, segment-aware network topologies in real time.

By leveraging a hardware-accelerated **HTML5 Canvas 2D Context API** instead of traditional DOM structures or heavy visualization libraries, the application maintains high frames-per-second (FPS) performance even on large-scale enterprise mapping grids.

## Key Architectural Benefits

### Zero Backend Footprint

Fully static rendering logic enables:

- Offline usage
- Air-gapped deployments
- Instant hosting

### Responsive Control Schemas

Supports:

- Keyboard controls
- Mouse interactions
- Touch gestures
- Tablet interfaces

### Theme-Native Vector Scaling

Dynamically modifies:

- Vector coordinates
- Border colors
- Text anchors
- Grid layouts
- Glow effects

Based on the selected operating theme.

---

# 🏛️ Interface Anatomy

```text
+---------------------------------------------------------------------------------+
|                                 TOP BANNER HEADER                              |
| [Logo/Net-Mapper]                                    [Search] [Labs] [Themes]   |
+-----------------------------------+---------------------------------------------+
|                                   |                                             |
|                                   |          INTERACTIVE VISUAL CANVAS          |
|            LEFT PANEL             |                                             |
|                                   |  * Viewport (Pan/Zoom)                      |
|  [Tab: Add Asset]                 |  * Node Elements                            |
|  * Asset Name                     |  * Routing Connections                      |
|  * IP Address                     |                                             |
|  * Role / Class                   |                                             |
|  * Subnet Zone                    |                                             |
|  * Severity State                 |---------------------------------------------|
|                                   |              RIGHT INSPECTOR PANEL          |
|  [Tab: Active Assets]             |                                             |
|  * Directory Inventory List       |  * Context-Sensitive Form Editors           |
|  * Detected Subnet Segments       |  * Connection Link Creator                  |
|                                   |  * Dijkstra Routing Engine Controls         |
|                                   |                                             |
+-----------------------------------+---------------------------------------------+
|               FOOTER: Engine Version & Client Sandbox Metrics Status            |
+---------------------------------------------------------------------------------+
```

---

# 🕹️ Operator Workflow & Controls

## Canvas Navigation Matrix

| Action | Mouse Controls | Touch / Tablet Controls |
|----------|----------------|--------------------------|
| Pan Camera | Right-Click + Drag Canvas | Drag on empty canvas area |
| Zoom Viewport | Mouse Scroll Wheel | Pinch-to-Zoom |
| Reposition Node | Left-Click + Drag Node | Single Touch + Drag Node |
| Connect Nodes | Shift + Left-Click Drag from Source to Target | Select Node → Use Right Panel Link Form |
| Quick-Spawn Node | Double-Click Empty Canvas | Double-Tap Empty Canvas |
| Deselect / Clear | Click Empty Grid Background | Tap Empty Grid Background |

---

## Step-by-Step Asset Deployment

### 1. Configure Asset Criteria

Use the **Add Asset** form located within the left configuration panel.

### 2. Assign Subnets

Allocate an accurate subnet designation, such as:

- DMZ
- PCI-DSS
- Active Directory Domain
- Guest Network
- Production

The system automatically extracts subnet identifiers and generates segmentation statistics.

### 3. Establish Connectivity

1. Hold the **Shift** key.
2. Click inside the source node.
3. Drag directly to the target node.
4. Release to create the routing path.

---

# 🧠 Routing Diagnostics (Dijkstra Engine)

Net-Mapper integrates a matrix-based implementation of **Dijkstra's Algorithm** to compute the shortest-hop route through complex network environments.

---

## Mathematical Weight Allocation

The routing engine evaluates routes using weighted connection values.

| Connection Type | Weight |
|---------------|----------|
| High-Speed Uplink (10 Gbps) | 2.5 |
| Standard LAN (1 Gbps) | 1.0 |
| Slow / Saturated WAN (100 Mbps) | 0.5 |

---

## Running Route Diagnostics

### Step 1

Select:

**Compute Shortest Path (Dijkstra)**

from the diagnostics toolbox.

### Step 2

Choose:

- Source Node (Origin)
- Destination Node (Target)

### Step 3

Click:

**Calculate Path**

### Output

The platform will:

- Highlight the optimal route in Emerald Green
- Apply visual glow effects
- Display sequential hop information
- Log routing details within the operator terminal panel

---

# 🗄️ Standard Schema & Database Structures

Topology templates are serialized as lightweight JSON documents.

## Data Model

```typescript
interface Topology {
  nodes: Node[];
  edges: Edge[];
  theme: 'dark' | 'light' | 'corporate' | 'terminal' | 'contrast';
}

interface Node {
  id: string;          // Cryptographically secure UUIDv4
  name: string;        // Hostname label
  ip: string;          // IPv4 standard string representation
  type: NodeType;      // firewall | router | server | workstation | cloud | database | iot
  subnet: string;      // Logical segment tag
  severity: Severity;  // normal | warning | critical
  desc: string;        // Operational description
  x: number;           // Absolute canvas X coordinate
  y: number;           // Absolute canvas Y coordinate
  radius: number;      // Vector collision bounds (default: 26)
}

interface Edge {
  id: string;          // Cryptographically secure UUIDv4
  from: string;        // Source Node UUIDv4 matching node.id
  to: string;          // Target Node UUIDv4 matching node.id
  type: EdgeType;      // bidirectional | directional | tunnel
  weight: number;      // Routing weight coefficient (1, 2.5, 0.5)
  label?: string;      // Connection segment description
}
```

---

# 🚢 Deployment Procedures

## Instant Static Hosting (GitHub Pages)

Since Net-Mapper has zero compiled module dependencies, deployment is immediate.

### Steps

1. Create a GitHub repository.
2. Upload `index.html` to the root of the repository.
3. Navigate to:

```text
Settings → Pages
```

4. Configure:

```text
Source: Deploy from branch
Branch: main
Folder: /
```

5. Save changes.

### Live URL Format

```text
https://[your-github-username].github.io/[repository-name]/
```

---

## Localized Enterprise Air-Gap Deployment

For organizations operating isolated environments:

### Steps

1. Copy the `index.html` file to the target workstation.
2. Open using:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox
- Safari

### Offline Capability

The following features continue functioning without internet access:

- Local Storage
- Saved Topologies
- Theme Selection
- Import / Export Operations
- Routing Diagnostics

---

# 🔒 Operational Security (OPSEC) Posture

Net-Mapper operates under a **Strict Client-Side Sandboxing Directive**.

---

## Zero Telemetry Transmissions

The application contains:

- No API endpoints
- No analytics packages
- No tracking services
- No external databases

No network topology information, IP addresses, or infrastructure metadata is transmitted externally.

---

## Localized Persistence

State information is stored exclusively within:

```text
HTML5 LocalStorage
```

All data remains sandboxed to the browser origin.

### Data Removal

Clearing browser site data immediately removes:

- Stored topologies
- User preferences
- Session state

---

## Offline Import / Export Safety

Imported files are processed entirely within the browser through:

```text
FileReader API
```

### Security Guarantees

- No server-side processing
- No upload queues
- No cloud synchronization
- No external inspection

All imported and exported topology files remain local to the operator's device.

---

# Summary

Net-Mapper is a high-performance, fully client-side network visualization platform built for:

- Security Operations Centers (SOC)
- Network Engineering Teams
- Penetration Testers
- Red Teams
- Enterprise Infrastructure Architects

Its architecture emphasizes:

- Performance
- Offline functionality
- Air-gapped operation
- Routing intelligence
- OPSEC-first design

while maintaining zero external dependencies and complete operator control over network topology data.

