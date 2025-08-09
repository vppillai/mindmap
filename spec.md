# Mind Map Application – Extended Technical Specification (v3.0)

## 1. Overview
A **web-based, modern, minimal-yet-powerful mind mapping application** designed as a **thinking canvas** for organizing ideas.  
It emphasizes **clarity**, **compact representation**, **keyboard-first editing**, **local-first persistence**, and **AI-assisted ideation**.  

The app supports two **complementary layout modes**:
- **Elastic Mode**: Fully freeform placement, complete manual control.
- **Strict Mode**: Balanced, automatic arrangement for clean tree/radial layouts, avoiding overlaps.

The UI and interactions must be **polished, smooth, and responsive**, with animations and transitions that make rearranging, zooming, and editing feel natural.

---

## 2. Target Users
- Researchers, engineers, and knowledge workers
- Writers, journalists, and content creators
- Product designers and strategists
- Students and educators
- Teams planning projects or brainstorming ideas

---

## 3. Core Design Principles
1. **Compact Clarity** – Maximize idea density without clutter.
2. **Dual Control** – Allow both manual and automatic node arrangement.
3. **Keyboard-First, Mouse-Ready** – Power users get fast shortcuts, casual users get intuitive clicks/taps.
4. **No Vendor Lock-In** – Fully portable JSON/Markdown formats.
5. **Offline-First** – Works entirely offline, with optional online sync.
6. **Extendable** – Designed with an API for future plugins/extensions.
7. **Visual Delight** – Clean, modern aesthetic with subtle shadows, rounded corners, and soft animations.

---

## 4. Core Features

### 4.1 Layout Modes
**Elastic Mode**  
- Infinite drag-and-drop placement, overlaps allowed.
- Node coordinates saved exactly.
- Grid-snapping toggle (optional).
- Multi-node selection + group move.

**Strict Mode**  
- Tree, radial, or layered hierarchical layouts.
- **Balanced Layout Algorithm**:
  - Evenly distributes sibling nodes.
  - Prevents overlaps with collision detection.
  - Dynamically reflows only affected branches, not the whole map.
- Manual override:
  - User can nudge individual nodes.
  - Overrides stored until “Recalculate Layout” is triggered.

---

### 4.2 Node Creation & Editing

#### Primary Data Model
- **Nodes are primarily text-based**:
  - Multi-line content is supported.
  - Rich text formatting is allowed using **Markdown** syntax stored in the JSON.
  - Markdown is rendered to HTML in the UI with sanitization.
- The text rendering engine supports:
  - Bold, italic, underline, strikethrough
  - Headings (H1–H3)
  - Bullet and numbered lists
  - Checklists
  - Hyperlinks
  - Inline code and code blocks
  - Block quotes
  - Inline images (optional)
- Styling per node:
  - Text color (any CSS color, with accessible contrast check)
  - Background color
  - Border style (solid/dashed/none)
  - Border radius
  - Drop shadow toggle

#### Editing Behavior
- **Inline WYSIWYG Markdown editor**:
  - Toolbar with basic formatting options
  - Toggle to “raw Markdown” mode for power users
  - Live preview in-place
- **Keyboard controls**:
  - `Enter` → New paragraph
  - `Shift+Enter` → Soft line break
  - `Tab` → Create child node
  - `Enter` at end of line → Create sibling node
  - `Ctrl+Enter` → Finish editing
- Node auto-resizes to fit content, up to a max height (scroll within node if exceeded).
- Dragging node in **Elastic Mode** moves its position without affecting siblings.
- In **Strict Mode**, node position is layout-driven but can be nudged.

#### Visual Enhancements
- **Path Highlighting**:
  - When a node is selected, all nodes and edges from it up to the root are visually highlighted.
  - Highlight style:
    - Nodes in path: brighter border + background tint
    - Edges in path: accent color
  - Fades out when deselected.
- **Selection State**:
  - Selected node has stronger outline and subtle shadow.
  - Keyboard navigation moves selection and updates path highlight in real-time.

#### Data Structure (JSON example)
```json
{
  "id": "node-123",
  "content": "**Project Plan**\n- Define scope\n- Assign roles",
  "format": "markdown",
  "style": {
    "textColor": "#222222",
    "backgroundColor": "#fef3c7",
    "border": "1px solid #d97706",
    "borderRadius": 8,
    "shadow": true
  },
  "position": { "x": 200, "y": 150 },
  "children": ["node-124", "node-125"],
  "parent": "node-001"
}


---

### 4.3 Infinite Canvas
- Smooth zoom & pan:
  - Zoom with Ctrl+Scroll / trackpad pinch
  - Pan with middle mouse or Space+drag
- Mini-map with draggable viewport rectangle
- Optional grid background with opacity slider
- Canvas boundaries are *virtual* (infinite in all directions)

---

### 4.4 Data Persistence
- **Local-first** with auto-save (every 3 seconds idle)
- Manual save/load from:
  - JSON (full fidelity)
  - Markdown (outline)
- Optional browser IndexedDB for larger maps
- “Open Recent” list with thumbnails

---

### 4.5 Import & Export
- **Import**:
  - JSON
  - Markdown
  - OPML (mind map/outline interchange format)
- **Export**:
  - JSON
  - Markdown
  - SVG (vector, scalable)
  - PNG (bitmap)
  - PDF (with embedded outline)
  - DOCX (headings reflect hierarchy)
- **Copy-Paste to External Apps**:
  - Maintains indentation + basic rich text
  - Pasting into Excel/Sheets produces an indented outline

---

### 4.6 Keyboard Shortcuts
| Action | Shortcut |
|--------|----------|
| Add sibling node | Enter |
| Add child node | Tab |
| Promote node level | Shift+Tab |
| Delete node | Delete |
| Edit node | F2 or double-click |
| Collapse/expand node | Space |
| Navigate nodes | Arrow keys |
| Zoom in/out | Ctrl + / Ctrl - |
| Select all | Ctrl+A |
| Copy/Paste | Ctrl+C / Ctrl+V |
| Undo/Redo | Ctrl+Z / Ctrl+Y |
| Toggle layout mode | Ctrl+M |
| Search nodes | Ctrl+F |
| AI assistant | Ctrl+Shift+A |

---

## 5. AI Assistant

### 5.1 Integration Points
- **Context menu on node**:
  - Expand idea
  - Summarize branch
  - Suggest related topics
  - Generate examples
- **Floating toolbar**:
  - One-click “brainstorm mode”
  - Suggest N child nodes
- **AI Sidebar**:
  - Persistent chat with context awareness
  - Supports dragging AI-generated nodes into map

### 5.2 Backend AI Proxy
- Node.js/Express backend
- `.env` config for:
  - AI provider
  - API key
  - Model selection
- Supports:
  - OpenAI
  - Anthropic
  - Local inference servers (Ollama, LM Studio)
- Rate limiting + request logging

---

## 6. User Preferences
Stored in LocalStorage/IndexedDB:
- Theme (light/dark/system)
- Node font, size, default color
- Layout mode preference
- Auto-save interval
- Keyboard shortcut remapping
- Default AI provider/model

---

## 7. Deployment

### 7.1 Docker Setup
- **Frontend**: React + TypeScript + Tailwind
- **Backend**: Node.js/Express AI proxy
- `docker-compose.yml` to spin up both
- Reverse proxy with CORS configured
- Environment variables in `.env`:
  - `PORT`
  - `AI_PROVIDER`
  - `AI_API_KEY`
  - `ALLOWED_ORIGINS`

---

## 8. Advanced Features
- **Search & Filter** with live highlighting
- **Node Tagging** and tag-based filtering
- **Branch Locking** to prevent accidental changes
- **Version History** with rollback
- **Templates Library**: prebuilt mind map structures
- **Theme Customization**:
  - User-defined palettes
  - Font choices
- **Print Mode**:
  - Optimized for A4/Letter PDF
  - Auto-collapsed branches beyond a depth

---

## 9. Strict Mode Layout Algorithm
1. Assign bounding boxes to nodes.
2. Calculate sibling angular/horizontal placement based on:
   - Number of leaves
   - Branch depth
3. Apply force-directed separation to prevent overlaps.
4. Anchor root at fixed position unless user drags it.
5. Preserve manual offsets unless “Recalculate” triggered.
6. Animate transitions for smooth rearrangements.

---

## 10. Security
- Sanitize HTML in node content.
- No API keys in frontend.
- CSRF + CORS protection.
- Optional password-protected maps (client-encrypted).

---

## 11. Roadmap
**Phase 1 (MVP)**: Core mind map editing, local persistence, import/export JSON & Markdown.  
**Phase 2**: Mini-map, search, PDF/SVG export, AI context menu.  
**Phase 3**: Tagging, templates, version history, theme editor.  
**Phase 4**: Collaboration (WebSockets + CRDT), cloud sync.

---

## 12. Visual Design Guidelines
- **Nodes**: Rounded corners, subtle shadows, 1–2px border
- **Edges**: Smooth bezier curves, anti-aliased
- **Colors**: Pastel defaults, accessible contrast
- **Animations**:
  - 150–250ms ease-in-out for zoom/pan
  - Fade/scale in when adding nodes
  - Slide-out collapse animations
- **Icons**: Feather icons or Material Icons for minimal look
- **Font**: Inter or Open Sans

---

*End of Spec*
