# IBM_BOB_PROJECT
Concept Linker — Students study topics in isolation. Auto-link related concepts across subjects to build a personal knowledge graph.
# 🎓 Concept Linker — Comprehensive Project Report & Documentation

> **An Intelligent Personal Knowledge Graph & Mind Map System for Students and Educators**  
> *Transforming fragmented study notes into an interactive, visual, cross-linked knowledge tree.*

---


### Project Contributors:

* Ayesha Luqman:[https://github.com/Ayesha-Luqman/](https://github.com/Ayesha-Luqman/)
* Khubaib Muhammad:[https://github.com/Khubaib-Muhammad](https://github.com/Khubaib-Muhammad)

*Project Link: [https://drive.google.com/file/d/1Mlc7wUFEC25qgaRqTIJYomBwot1RC_Cz/view](https://drive.google.com/file/d/1Mlc7wUFEC25qgaRqTIJYomBwot1RC_Cz/view)*

## 📑 Table of Contents
1. [Executive Summary](#1-executive-summary)
2. [The Problem: Why Traditional Study Notes Fail](#2-the-problem-why-traditional-study-notes-fail)
3. [The Solution & Educational Value](#3-the-solution--educational-value)
4. [Complete Feature Guide & Visual Walkthroughs](#4-complete-feature-guide--visual-walkthroughs)
   - [4.1 Note Ingestion & Multi-Format Parsing](#41-note-ingestion--multi-format-parsing)
   - [4.2 Smart NLP Concept Extraction & Acronym Normalization](#42-smart-nlp-concept-extraction--acronym-normalization)
   - [4.3 Hierarchical Knowledge Tree & Mind Map Visualization](#43-hierarchical-knowledge-tree--mind-map-visualization)
   - [4.4 Navigation, Filtering & Viewport Ergonomics](#44-navigation-filtering--viewport-ergonomics)
   - [4.5 Semantic Cross-Linking & Interactive Relationship Cards](#45-semantic-cross-linking--interactive-relationship-cards)
   - [4.6 Note Maintenance, Re-Extraction & Database Sanitization](#46-note-maintenance-re-extraction--database-sanitization)
   - [4.7 Knowledge Portability, Obsidian Export & Backups](#47-knowledge-portability-obsidian-export--backups)
5. [System Architecture & Technology Stack](#5-system-architecture--technology-stack)
6. [Database Schema & Data Model](#6-database-schema--data-model)
7. [Installation, Setup & User Quickstart](#7-installation-setup--user-quickstart)
8. [API Reference Documentation](#8-api-reference-documentation)
9. [Verification, Benchmarks & Testing Results](#9-verification-benchmarks--testing-results)

---

## 1. Executive Summary

**Concept Linker** is a private, subject-agnostic web application designed to help students, researchers, and educators build interconnected personal knowledge graphs from raw study materials. 

Instead of leaving notes trapped in isolated documents or disorganized folders, Concept Linker automatically processes notes (typed text, pasted articles, uploaded PDF/DOCX files), identifies key conceptual entities, canonicalizes acronyms into semantic tags, and constructs a structured **Knowledge Tree and Mind Map**.

### Key Highlights:
- **100% Local & Zero-Docker**: Runs natively on Node.js using embedded `node:sqlite` and in-process NLP similarity, eliminating container overhead and network latency.
- **Hierarchical Tree & Mind Map Layouts**: Switch effortlessly between Horizontal Mind Map (`LR`) and Vertical Tree Hierarchy (`TB`) powered by Dagre.
- **Obsidian-Compatible Export**: 1-click export of notes enriched with `[[WikiLinks]]` for seamless integration into Obsidian, Logseq, and Notion.
- **Privacy First**: Zero external third-party AI APIs required; all text processing and data persistence stay on the user's machine.

---

## 2. The Problem: Why Traditional Study Notes Fail

Students and educators face several structural challenges with modern digital note-taking:

```
Traditional Note Taking (The Silo Problem):
[ Biology Notes ]       [ Chemistry Notes ]       [ Physics Notes ]
       │                         │                        │
  (Isolated)                (Isolated)               (Isolated)
       ▼                         ▼                        ▼
  Forgotten                 Forgotten                Forgotten
```

1. **The Silo Effect**: Students study topics in departmental isolation (e.g. studying "Energy Transfer" in Physics without linking it to "Metabolism" in Biology or "Thermodynamics" in Chemistry). Knowledge remains compartmentalized and easily forgotten.
2. **Linear Note Traps**: Standard linear documents (Word docs, Google Docs, Apple Notes) do not reflect how the human brain retains information—which is associative and non-linear.
3. **High Maintenance Friction**: Manually cross-referencing and tagging hundreds of concepts in personal wikis is tedious, causing students to abandon knowledge management systems within weeks.
4. **Acronym & Synonym Confusion**: Raw notes often mix abbreviations (e.g. *CNNs*, *ML*, *NLP*) with full terms, creating fragmented duplicates and conceptual clutter.

---

## 3. The Solution & Educational Value

```
Concept Linker (Associative Knowledge Tree):
                    ┌─────────────────────────┐
                    │ 🌐 Knowledge Base (Root) │
                    └────────────┬────────────┘
            ┌────────────────────┴────────────────────┐
            ▼                                         ▼
   ┌─────────────────┐                       ┌─────────────────┐
   │ 📝 Deep Learning │                       │  📝 NLP & LLMs  │
   └────────┬────────┘                       └────────┬────────┘
     ┌──────┴──────┐                           ┌──────┴──────┐
     ▼             ▼                           ▼             ▼
┌─────────┐   ┌─────────┐                 ┌─────────┐   ┌─────────┐
│💡 CNNs  │   │💡 Vision│◄- - - - - - - -►│💡 Attn  │   │💡 Trans │
└─────────┘   └─────────┘  (Semantic Link)└─────────┘   └─────────┘
```

### For Students:
- **Spontaneous Conceptual Discovery**: Auto-detects similarities across different classes and semesters, reinforcing active recall.
- **Cognitive Clarity**: Visualizing notes as a hierarchical tree with collapsible branches prevents information overwhelm.
- **Accelerated Revision**: Clicking any concept immediately highlights every note where that idea was introduced.

### For Teachers & Educators:
- **Curriculum Mapping**: Easily map course syllabi to verify that prerequisite topics connect logically to advanced modules.
- **Visual Lecture Aids**: Present interactive mind maps during lectures to illustrate complex topic hierarchies.

---

## 4. Complete Feature Guide & Visual Walkthroughs

### 4.1 Note Ingestion & Multi-Format Parsing
Concept Linker supports three flexible input pathways:
1. **Direct Composition**: Rich title and text editor for quick lecture notes.
2. **Pasted Content**: Paste articles, summaries, or transcripts directly.
3. **File Uploads**: Drag-and-drop parsing for **PDF (`.pdf`)** and **Word (`.docx`)** documents using native buffer extraction (`pdf-parse` & `mammoth`).

---

### 4.2 Smart NLP Concept Extraction & Acronym Normalization
When a note is created or updated, our local NLP pipeline performs:
- **Acronym Detection**: Scans text for `Full Concept (Acronym)` patterns (e.g. `"Convolutional Neural Networks (CNNs)"`).
- **Canonical Labeling**: Converts concepts to clean base forms (`"convolutional neural networks"`) and binds acronyms as tags (`#cnn`).
- **Compound Conjunction Splitting**: Splits multi-topic phrases (`"Computer Vision and Robotics"` ➔ `"computer vision"`, `"robotics"`).
- **Subsumption Filtering**: Drops redundant sub-strings and generic filler words (`method`, `approach`, `system`).

---

### 4.3 Hierarchical Knowledge Tree & Mind Map Visualization

The application organizes knowledge into a clear 3-tiered tree structure:
1. **🌐 Knowledge Base (Root)**: The central anchor of your knowledge base.
2. **📝 Notes (Branches)**: Each note acts as a topic branch with child count badges and collapse handles.
3. **💡 Concepts (Leaves)**: Extracted key concepts equipped with color-coded tag pills and quick action buttons.

```
Layout Modes:
1. ➔ Mind Map (LR): Spreads branches horizontally from left to right (ideal for wide screens).
2. ⬇ Hierarchy (TB): Spreads branches vertically from top to bottom (ideal for structured outlines).
```

---

### 4.4 Navigation, Filtering & Viewport Ergonomics

- **🧭 Left Sidebar Tree Navigator**: Search through all notes and concepts with instant fuzzy filtering and Expand/Collapse All buttons.
- **🏷️ Interactive Tag Filter Chips**: Filter the entire canvas by tag (e.g. `#ml`, `#nlp`, `#vision`) to isolate related branches while dimming unrelated nodes.
- **⛶ Focus Mode (Fullscreen)**: 1-click fullscreen mode that collapses sidebars and auto-centers the viewport for deep-work mind mapping.
- **📋 Concept Copying**: Hovering on any concept reveals a copy button (`📋` ➔ `✓`) for quick pasting into research papers or flashcards.

---

### 4.5 Semantic Cross-Linking & Interactive Relationship Cards

- **Automated Similarity Scoring**: Computes token Jaccard and character 3-gram cosine similarity across concepts. Concepts exceeding similarity thresholds receive animated dashed connection lines.
- **Manual Connections (`＋ Connect Concepts`)**: Connect any two concepts directly by clicking them in the canvas or the sidebar navigator.
- **Interactive Relationship Card**: Clicking any connection line displays a floating card showing:
  - Connected concepts (`Concept A ⇄ Concept B`)
  - Match strength badge (e.g. `95% Match` / `Manual Connection`)
  - 1-click `🗑️ Delete Link` button.

---

### 4.6 Note Maintenance, Re-Extraction & Database Sanitization

- **⚡ Note Concept Re-Extraction**: Click the `⚡` button on any note card to synchronously re-run the latest NLP extraction rules on demand.
- **🧹 Concept Database Cleanup**: Click the `🧹 Clean Concepts` button in the Notes header to sanitize legacy trailing punctuation, merge duplicate concept records, and re-link note associations automatically.
- **🗑️ Granular Deletion**: Delete individual notes, concepts, or links securely.

---

### 4.7 Knowledge Portability, Obsidian Export & Backups

Concept Linker ensures your knowledge is never locked into a proprietary silo:
1. **📄 Markdown (`.md` / Obsidian-Ready)**:
   - Exports all notes with extracted concepts automatically formatted into **`[[WikiLinks]]`**.
   - Includes a formatted **Knowledge Tree Index** at the top.
2. **🌳 Mind Map (`.json`)**:
   - Exports the full tree hierarchy, coordinates, and link relations.
3. **💾 Full Database Backup (`.json`)**:
   - Generates a complete single-file JSON backup of your user profile, notes, concepts, tags, and link mappings.

---

## 5. System Architecture & Technology Stack

```
┌─────────────────────────────────────────────────────────────┐
│                    REACT CLIENT (:5173)                     │
│  - React 19 + Vite (Fast HMR & Optimized Production Build) │
│  - React Flow (@xyflow/react) Visual Canvas                 │
│  - Dagre Hierarchical Layout Engine                         │
│  - Sonner Toast Notifications                               │
└──────────────────────────────┬──────────────────────────────┘
                               │ REST API (Bearer JWT)
┌──────────────────────────────▼──────────────────────────────┐
│                    EXPRESS SERVER (:5000)                   │
│  - Node.js v26 Native Runtime                               │
│  - Embedded SQLite Database (node:sqlite DatabaseSync)      │
│  - In-Process Smart NLP Tokenizer & 3-Gram Similarity       │
│  - PDF/DOCX Binary File Parsers                             │
└─────────────────────────────────────────────────────────────┘
```

### Core Technologies:
- **Frontend**: React 19, Vite, React Flow, Dagre, Vanilla CSS design system.
- **Backend**: Node.js Express, embedded `node:sqlite`, JWT authentication, Bcrypt password hashing.
- **Parsing**: `pdf-parse` (PDF documents), `mammoth` (Word `.docx` documents).
- **Zero External AI Dependencies**: Complete privacy with local NLP processing.

---

## 6. Database Schema & Data Model

The SQLite database (`server/concept_linker.db`) utilizes relational tables with foreign keys and cascading integrity:

```sql
-- 1. Users Table
CREATE TABLE IF NOT EXISTS users (
  id TEXT PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 2. Notes Table
CREATE TABLE IF NOT EXISTS notes (
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  title TEXT NOT NULL,
  content TEXT NOT NULL,
  source_type TEXT NOT NULL, -- 'typed' | 'uploaded'
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 3. Concepts Table
CREATE TABLE IF NOT EXISTS concepts (
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  label TEXT NOT NULL,
  tags TEXT DEFAULT '[]', -- JSON array of strings
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(user_id, label)
);

-- 4. Note-to-Concept Junction Table
CREATE TABLE IF NOT EXISTS note_concepts (
  note_id TEXT NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
  concept_id TEXT NOT NULL REFERENCES concepts(id) ON DELETE CASCADE,
  PRIMARY KEY (note_id, concept_id)
);

-- 5. Concept Links Table
CREATE TABLE IF NOT EXISTS concept_links (
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  concept_a_id TEXT NOT NULL REFERENCES concepts(id) ON DELETE CASCADE,
  concept_b_id TEXT NOT NULL REFERENCES concepts(id) ON DELETE CASCADE,
  strength REAL NOT NULL DEFAULT 1.0,
  is_manual BOOLEAN NOT NULL DEFAULT 0,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(concept_a_id, concept_b_id)
);
```

---

## 7. Installation, Setup & User Quickstart

### Prerequisites:
- **Node.js**: v18 or higher (Node v20+ or v26 recommended).

### 1-Click Launch (Windows):
Double-click [`run.bat`](file:///d:/tests/bob_project/concept-linker/run.bat) in the project root. This automatically installs missing packages and boots both the backend API and frontend dev server.

### Manual Terminal Launch:
```bash
# 1. Install root, server, and client dependencies
npm run install:all

# 2. Start both server and frontend concurrently
npm run dev
```

### Access Points:
- **Web App UI**: [http://localhost:5173](http://localhost:5173)
- **Backend API**: [http://localhost:5000](http://localhost:5000)
- **Health Check**: [http://localhost:5000/health](http://localhost:5000/health)

---

## 8. API Reference Documentation

| Method | Endpoint | Auth | Description |
|---|---|:---:|---|
| `POST` | `/api/auth/register` | No | Register a new user (`email`, `password`) |
| `POST` | `/api/auth/login` | No | Authenticate user and receive JWT token |
| `GET` | `/api/user/me` | Bearer | Fetch authenticated user profile |
| `GET` | `/api/user/export` | Bearer | Download full database JSON backup |
| `GET` | `/api/notes` | Bearer | List all notes belonging to the user |
| `POST` | `/api/notes` | Bearer | Create a note (text or multipart PDF/DOCX) |
| `GET` | `/api/notes/:id` | Bearer | Fetch a single note by ID |
| `PUT` | `/api/notes/:id` | Bearer | Update note title or content (re-runs NLP) |
| `DELETE` | `/api/notes/:id` | Bearer | Delete note and cascade relationships |
| `POST` | `/api/notes/:id/re-extract` | Bearer | Synchronously re-extract concepts for a note |
| `GET` | `/api/graph` | Bearer | Fetch tree hierarchy, notes, concepts, and links |
| `PATCH`| `/api/concepts/:id/tags` | Bearer | Replace custom tags on a concept |
| `POST` | `/api/concepts/cleanup` | Bearer | Sanitize and deduplicate legacy concepts |
| `DELETE`| `/api/concepts/:id` | Bearer | Delete a specific concept node |
| `POST` | `/api/links` | Bearer | Manually create a link between two concepts |
| `DELETE`| `/api/links/:id` | Bearer | Remove a semantic or manual link |

---

## 9. Verification, Benchmarks & Testing Results

### Automated Test Suite (13 / 13 Passed — 100%):
```
=== COMPREHENSIVE END-TO-END VERIFICATION ===

✓ 1. Server Health Check: OK
✓ 2. User Registration & JWT Issuance: OK
✓ 3. User Profile API: OK
✓ 4. Note 1 Creation & Storage: OK
✓ 5. Smart NLP Extraction & Acronym Canonicalization: OK
✓ 6. Note Re-Extraction Endpoint: OK
✓ 7. Concept Tagging (PATCH /api/concepts/:id/tags): OK
✓ 8. Concept DB Cleanup & Deduplication: OK
✓ 9. Manual Concept Linking (POST /api/links): OK
✓ 10. Link Deletion (DELETE /api/links/:id): OK
✓ 11. Full Database Backup Export (GET /api/user/export): OK
✓ 12. Concept Deletion (DELETE /api/concepts/:id): OK
✓ 13. Note Deletion (DELETE /api/notes/:id): OK

🎉 ALL 13 END-TO-END FINAL TESTS PASSED 100%!
```

### Performance & Build Metrics:
- **Client Build Time**: `530ms` (`vite build`, 562 modules).
- **Backend API Response Time**: `< 15ms` for full tree graph query.
- **NLP In-Process Extraction**: `< 40ms` per typical study note.
- **Container Footprint**: 0 MB (Zero Docker containers required).

---


