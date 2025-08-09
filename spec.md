# Mind Map Application – Extended Technical Specification (v2.0)

## 1. Overview
This is a **web-based, minimalist yet powerful mind mapping application** designed as a "thinking canvas" for organizing ideas.  
It is optimized for **compact visual representation**, **keyboard-driven editing**, **rich export formats**, and **AI-assisted ideation**.  

It features two layout modes:
- **Elastic Mode**: Fully freeform — users can drag nodes anywhere, overlap allowed.
- **Strict Mode**: Automatic non-overlapping arrangement for readability.

---

## 2. Target Users
- Knowledge workers and researchers  
- Writers and journalists  
- Product designers and strategists  
- Students and educators  
- Anyone who needs **a compact, visual thinking tool** that can export to documents.

---

## 3. Primary Goals
1. Keep the **visual footprint compact** while maximizing clarity.  
2. Maintain **feature richness** without overloading UI.  
3. Allow **manual and automatic layouts** to coexist seamlessly.  
4. Make the app **keyboard-first** with full mouse/touch support.  
5. Enable **persistent user settings** for a personalized workspace.  
6. Support **AI-driven assistance** for node expansion, summarization, and brainstorming.  
7. Allow **local-first data storage** with easy export/import.  
8. Provide **easy self-hosting and deployment**.

---

## 4. Core Features

### 4.1 Layout Modes
- **Elastic Mode**
  - Drag nodes anywhere on an infinite canvas.
  - Overlaps allowed (nodes and edges).
  - Position stored in save file.

- **Strict Mode**
  - Tree-like or radial arrangement.
  - No overlaps — uses **collision resolution algorithm** (see Section 10).
  - Auto-adjusts neighboring branches when adding/deleting nodes.
  - Compact placement to maximize visible nodes on screen.

### 4.2 Node Creation & Editing
- Add **child node** → `Tab` (indent) or context menu.
- Add **sibling node** → `Enter` (same level).
- Inline rich text editor per node (supports bold, italic, underline, highlight, hyperlinks, bullet lists).
- Node background color, border style, and icon support.
- Drag to reorder siblings in elastic mode.
- Merge/split nodes.
- Collapse/expand branches.
- Link to external documents or URLs.

### 4.3 Infinite Canvas
- Zoom in/out with `Ctrl+Scroll` or trackpad pinch.
- Pan with middle mouse drag or spacebar+drag.
- Mini-map view toggle for navigation.

### 4.4 Data Persistence
- **LocalStorage auto-save** every X seconds.
- Manual **Save As** to JSON or Markdown file.
- Auto-load last opened map from localStorage.

### 4.5 Import & Export
- Import JSON or Markdown (drag-and-drop or file dialog).
- Export:
  - JSON (editable, full metadata)
  - Markdown (outline format)
  - SVG (vector graphic)
  - PNG (bitmap image)
  - PDF (print-ready)
  - DOCX (with headings reflecting hierarchy)
- Copy-paste to Excel/Google Sheets:
  - Maintains hierarchy as indented text.
  - Rich text formatting preserved where possible.

### 4.6 Keyboard Shortcuts
| Action | Shortcut |
|--------|----------|
| Add sibling node | `Enter` |
| Add child node | `Tab` |
| Delete node | `Delete` |
| Edit node text | `F2` or double-click |
| Collapse/expand node | `Space` |
| Move selection | Arrow keys |
| Zoom in/out | `Ctrl +` / `Ctrl -` |
| Select all nodes | `Ctrl+A` |
| Copy selection | `Ctrl+C` |
| Paste nodes | `Ctrl+V` |
| Undo | `Ctrl+Z` |
| Redo | `Ctrl+Y` or `Ctrl+Shift+Z` |
| Toggle layout mode | `Ctrl+M` |
| Open AI Assistant | `Ctrl+Shift+A` |

---

## 5. AI Assistant Features

### 5.1 UI Integration
- **Floating button** in bottom-right corner.
- Click → opens chat window overlay.
- **Right-click node → “AI Actions” menu**:
  - Elaborate with prompt.
  - Summarize branch.
  - Suggest related ideas.
  - Convert branch to bullet points for export.

### 5.2 Backend AI Proxy
- Node.js/Express API as proxy.
- Reads keys from `.env` (`AI_PROVIDER`, `AI_API_KEY`).
- Supports:
  - OpenAI
  - Anthropic
  - Local models (via REST API)
- No direct API key exposure to frontend.

---

## 6. Persistent User Settings
Stored in browser localStorage:
- Theme (light/dark/system)
- Last used layout mode
- Zoom level
- Node font size/style
- Default node color
- Keyboard shortcut customizations
- AI model choice (if multiple available)

---

## 7. Deployment

### 7.1 Dockerized Setup
- **Frontend service** (React/TypeScript)
- **Backend service** (Node.js AI proxy)
- `docker-compose.yml` for easy multi-container start.
- `.env` file for:
  - API keys
  - Backend port
  - Allowed origins
- **Management script** (`manage.sh`):
  ```bash
  ./manage.sh start
  ./manage.sh stop
  ./manage.sh rebuild
  ./manage.sh logs
  ./manage.sh status
  ```

---

## 8. Additional Advanced Features
- **Search & Filter**: real-time node search with highlights.
- **Tagging System**: tag nodes and filter by tags.
- **Version History**: local revision history with restore points.
- **Branch Locking**: lock a branch to prevent accidental edits.
- **Quick Templates**: prebuilt mind map templates (SWOT, project planning, essay structure).
- **Collaboration-ready architecture** (future): WebSockets/CRDTs for multi-user editing.
- **Custom Themes**: user-defined color palettes.

---

## 9. Export to Documents
- DOCX and PDF include:
- Mind map image (snapshot)
- Outline text hierarchy
- AI-generated executive summary (optional toggle)

---

## 10. Strict Mode Collision Resolution Algorithm
1. Represent nodes as bounding boxes.
2. Start from root node, place children radially or hierarchically.
3. For each sibling group:
 - Calculate required space from leaf count.
 - Assign angular/radial positions or horizontal positions.
4. Detect overlaps → push overlapping nodes apart with force-directed separation until no intersections.
5. Recalculate edge paths to avoid crossing where possible.
6. Apply "compactness bias" — minimize space without overlap.

---

## 11. Security
- API keys only in backend.
- HTML sanitization for rich text.
- CSRF and CORS protection on backend.
- Optional password-protected maps.

---

## 12. Roadmap
**Phase 1 (MVP)**: Core mind map editing, layouts, import/export JSON+MD, localStorage persistence.  
**Phase 2**: Image/PDF export, mini-map, search.  
**Phase 3**: AI integration, DOCX export, tagging.  
**Phase 4**: Collaboration mode, templates, revision history.

---

*End of Extended Specification*
