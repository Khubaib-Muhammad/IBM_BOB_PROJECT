# IBM_BOB_PROJECT

# 🐞 TraceLink — Comprehensive Project Report & Technical Documentation

> **Developer Knowledge Graph & Root-Cause Correlation System for Debugging and Testing**  
> *Transforming stack traces, test failures, and error logs into an associative diagnostic knowledge tree.*

### Project Contributors:

* Ayesha Luqman:[https://github.com/Ayesha-Luqman/](https://github.com/Ayesha-Luqman/)
* Khubaib Muhammad:[https://github.com/Khubaib-Muhammad](https://github.com/Khubaib-Muhammad)

*Project Link: [https://drive.google.com/file/d/1Mlc7wUFEC25qgaRqTIJYomBwot1RC_Cz/view](https://drive.google.com/file/d/1Mlc7wUFEC25qgaRqTIJYomBwot1RC_Cz/view)*

---

## 📑 Table of Contents
1. [Executive Summary](#1-executive-summary)
2. [The Problem: Why Traditional Debugging & Testing Fail](#2-the-problem-why-traditional-debugging--testing-fail)
3. [The Solution: Associative Root-Cause Knowledge Graphs](#3-the-solution-associative-root-cause-knowledge-graphs)
4. [Complete Feature Guide & Workflows](#4-complete-feature-guide--workflows)
   - [4.1 Stack Trace, Test Failure & Log Ingestion](#41-stack-trace-test-failure--log-ingestion)
   - [4.2 Diagnostic Concept & Root-Cause Extraction](#42-diagnostic-concept--root-cause-extraction)
   - [4.3 Interactive Root Cause Tree & Mind Map Visualization](#43-interactive-root-cause-tree--mind-map-visualization)
   - [4.4 Navigation, Filtering & Viewport Ergonomics](#44-navigation-filtering--viewport-ergonomics)
   - [4.5 Automated Failure Correlation & Manual Linkage](#45-automated-failure-correlation--manual-linkage)
   - [4.6 Entity Normalization & Database Sanitization](#46-entity-normalization--database-sanitization)
   - [4.7 Postmortem Reports, Graph Export & Backups](#47-postmortem-reports-graph-export--backups)
5. [System Architecture & Technology Stack](#5-system-architecture--technology-stack)
6. [Database Schema & Data Model](#6-database-schema--data-model)
7. [Installation, Setup & User Quickstart](#7-installation-setup--user-quickstart)
8. [API Reference Documentation](#8-api-reference-documentation)
9. [Verification, Benchmarks & Testing Results](#9-verification-benchmarks--testing-results)

---

## 1. Executive Summary

**TraceLink** is a specialized, privacy-focused web application designed to help software developers, QA engineers, and site reliability engineers (SREs) ingest, classify, correlate, and diagnose test failures, stack traces, and unhandled errors into an interactive **Root-Cause Knowledge Graph and Mind Map**.

Software teams frequently face fragmented, repetitive debugging loops where the same underlying fault (e.g. database pool exhaustion, authentication signature mismatch, or null pointer exception) manifests in multiple test runs, microservices, and error logs. TraceLink ingests raw outputs from test frameworks (Jest, PyTest, JUnit) and runtime exceptions, automatically extracts diagnostic entities (exception classes, HTTP codes, socket errors, stack frames), and computes similarity links to expose common failure points across services.

### Key Highlights:
- **100% Local & Zero-Docker**: Operates natively on Node.js using embedded `node:sqlite` (`DatabaseSync`) and local in-process token Jaccard + 3-gram cosine similarity, eliminating container overhead.
- **Zero-Friction Local Workspace**: Instant-on workspace mode without mandatory registration or login blockers.
- **Hierarchical Diagnostic Tree & Mind Map**: Dual-mode visualization powered by React Flow and Dagre (`➔ Diagnostic Mind Map` and `⬇ Root Cause Tree Hierarchy`).
- **Automated Root-Cause Correlation**: Highlights shared fault domains across disparate test suites and services.
- **Structured Postmortem Generator**: Exports Markdown postmortem summaries with `[[WikiLinks]]` ready for incident reviews in Obsidian, GitHub Discussions, or Notion.

---

## 2. The Problem: Why Traditional Debugging & Testing Fail

```
Traditional Incident Management & Debugging (The Siloed Crash):
[ Service A Logs ]         [ CI Test Failures ]         [ Service B Logs ]
 (500 Pool Timeout)         (Auth Token Mismatch)        (ETIMEDOUT Socket)
        │                            │                           │
   (Isolated)                   (Isolated)                  (Isolated)
        ▼                            ▼                           ▼
Re-diagnosed from scratch    Flaky test ignored          Repeated Outage
```

1. **Repetitive Root-Cause Diagnosis**: Engineering teams spend up to 40% of debugging time re-investigating errors that have already occurred and been solved in adjacent services.
2. **Siloed Error Reporting**: Test runner failures, CI/CD pipeline crashes, and production application logs live in separate systems without unified cross-correlation.
3. **Flaky Test Blindness**: Intermittent test failures are often ignored rather than correlated with underlying shared resource contentions (e.g. socket exhaustion or database locks).
4. **Unstructured Knowledge Loss**: Postmortem insights rarely connect back to the raw stack traces and test suites that originally failed.

---

## 3. The Solution: Associative Root-Cause Knowledge Graphs

```
TraceLink (Associative Root-Cause Knowledge Tree):
                    ┌────────────────────────────────────────┐
                    │ 🛡️ Debug & Test Diagnostics (Root)    │
                    └───────────────────┬────────────────────┘
            ┌───────────────────────────┴───────────────────────────┐
            ▼                                                       ▼
   ┌────────────────────────┐                              ┌────────────────────────┐
   │ 🧪 Jest: auth.test.js  │                              │ ⚡ Node: Payment Pool  │
   │ (🔴 High • Open)       │                              │ (🔴 Critical • Open)   │
   └───────────┬────────────┘                              └───────────┬────────────┘
     ┌─────────┴─────────┐                                   ┌─────────┴─────────┐
     ▼                   ▼                                   ▼                   ▼
┌──────────────┐   ┌──────────────┐                     ┌──────────────┐   ┌──────────────┐
│🛑 AssertErr  │   │📦 auth.js:45 │◄- - - - - - - - - -►│🌐 ETIMEDOUT  │   │📦 pg.Pool    │
└──────────────┘   └──────────────┘ (Failure Correlation)└──────────────┘   └──────────────┘
```

TraceLink establishes an associative graph where:
1. **Incidents & Test Runs** branch out from the diagnostic base.
2. **Diagnostic Entities (Leaves)** represent extracted exceptions, stack frames, error codes, and assertions.
3. **Correlation Lines** dynamically connect related failures that share the same fault signature across different systems.

---

## 4. Complete Feature Guide & Workflows

### 4.1 Stack Trace, Test Failure & Log Ingestion
- **Monospaced Code Editor**: Optimized for pasting multi-line stack traces with preservation of indentation and formatting.
- **Preset Reproduction Templates**:
  - `🔴 Jest Assertion Failure`: Token mismatch assertion error.
  - `🟠 Node.js Database Connection Timeout`: `ETIMEDOUT` in connection pool.
  - `🟡 Python TypeError / NullPointer`: `TypeError` in sentiment analyzer.
- **Classification Selectors**:
  - **Severity**: `🔴 Critical`, `🟠 High`, `🟡 Medium`, `🔵 Low`.
  - **Status**: `Open`, `Investigating`, `Resolved`, `Flaky Test`.
  - **Incident Type**: `⚡ Stack Trace`, `🧪 Test Failure`, `📜 Log Snippet`, `🐞 Bug Report`.
- **File Uploads**: Drag-and-drop support for `.log`, `.txt`, `.json`, `.docx`, and `.pdf` files.

![Trace Ingestion Interface](docs/images/ingest_form.png)

### 4.2 Incident Management & Triage List
The incidents view organizes all ingested stack traces and test logs with real-time severity badges, filter chips, and extracted diagnostic entities:

![Incidents & Test Traces List View](docs/images/incidents_list.png)

### 4.3 Diagnostic Concept & Root-Cause Extraction
The heuristic extraction pipeline in `server/src/utils/extractConcepts.js` uses compromise NLP and targeted regex patterns to extract:
- **Exception Classes**: `AssertionError`, `TypeError`, `NullPointerException`, `IndexOutOfBoundsException`, `DeadlockDetected`.
- **System & Network Errors**: `ECONNREFUSED`, `ETIMEDOUT`, `ENOTFOUND`, `EADDRINUSE`, `ECONNRESET`.
- **HTTP & SQL Codes**: `HTTP 500`, `HTTP 502`, `HTTP 404`, `SQLSTATE 23505`, `SQLITE_ERROR`.
- **Source Paths**: `authService.js:45`, `payment/pool.js`, `main.py:45`.

### 4.4 Interactive Root Cause Tree & Mind Map Visualization
- **Dual Layout Modes**: Toggle between **➔ Diagnostic Mind Map (`LR`)** and **⬇ Root Cause Hierarchy (`TB`)** using Dagre hierarchical positioning.
- **Diagnostic Icon Coding**:
  - `🔴` / `🟠` — Critical / High severity incidents.
  - `🧪` — Test runner failure.
  - `⚡` — Runtime stack trace.
  - `🛑` — Unhandled exception.
  - `🌐` — Socket / Network error.
  - `⚠️` — Failed assertion.
  - `📦` — Affected module / source file.
- **Branch Collapsing**: Collapse or expand entire test suites with child-count badges (`+` / `−`).

![Interactive Root Cause Tree & Mind Map](docs/images/root_cause_tree.png)

### 4.5 Navigation, Filtering & Viewport Ergonomics
- **Real-Time Filter Bar**: Filter incidents by severity (`Critical`, `High`, `Medium`, `Low`) and status (`Open`, `Investigating`, `Resolved`, `Flaky`).
- **Diagnostic Navigator Drawer**: Left-side drawer with instant keyword search and tag filtering.
- **⛶ Focus Mode**: Fullscreen canvas mode that hides surrounding toolbars for distraction-free triage.
- **📋 1-Click Entity Copying**: Quick copy button on entity chips for fast pasting into terminal or code editors.

### 4.6 Automated Failure Correlation & Manual Linkage
- **Smart Similarity Auto-Linker**: Pairs similar traces and exceptions above threshold ($\ge 0.55$) using token Jaccard and 3-gram cosine similarity.
- **Manual Link Creation**: Connect test failures to suspected bugs directly on the canvas or via the sidebar.
- **Interactive Relationship Card**: Clicking any correlation edge displays match confidence and allows 1-click removal.

### 4.7 Postmortem Reports, Graph Export & Backups
- **📄 Postmortem Summary (`.md`)**: Exports comprehensive Markdown postmortem documents with `[[WikiLinks]]` linking root causes and failing tests.
- **🌳 Diagnostic Graph (`.json`)**: Exports graph structure and layout coordinates.
- **💾 Full Incident Backup (`.json`)**: 1-click complete database export.

![Export Postmortem & Diagnostic Graph Menu](docs/images/export_menu.png)

---

## 5. System Architecture & Technology Stack

| Layer | Technologies | Role |
|---|---|---|
| **Frontend** | React 19, Vite 8.2, @xyflow/react, Dagre, Sonner | Interactive UI, Mind Map canvas, Trace Editor, Filter Bars |
| **Backend API** | Node.js v20+, Express 4.19, Multer | REST API endpoints, multipart file ingestion, async workers |
| **Database** | `node:sqlite` (`DatabaseSync`) | Embedded SQL engine, zero external DBMS setup |
| **NLP Engine** | `compromise`, In-Process N-Gram & Jaccard Matcher | Diagnostic entity parser, regex heuristics, similarity scorer |
| **Document Parsers** | `pdf-parse`, `mammoth` | Ingestion of `.pdf`, `.docx`, `.txt`, `.log` files |

---

## 6. Database Schema & Data Model

```sql
CREATE TABLE IF NOT EXISTS notes (
  id TEXT PRIMARY KEY DEFAULT (lower(hex(randomblob(16)))),
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  title TEXT NOT NULL,
  content TEXT NOT NULL DEFAULT '',
  source_type TEXT NOT NULL DEFAULT 'typed',
  status TEXT NOT NULL DEFAULT 'open',
  severity TEXT NOT NULL DEFAULT 'medium',
  incident_type TEXT NOT NULL DEFAULT 'stack_trace',
  created_at TEXT NOT NULL DEFAULT (datetime('now')),
  updated_at TEXT NOT NULL DEFAULT (datetime('now'))
);

CREATE TABLE IF NOT EXISTS concepts (
  id TEXT PRIMARY KEY DEFAULT (lower(hex(randomblob(16)))),
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  label TEXT NOT NULL,
  tags TEXT NOT NULL DEFAULT '[]',
  created_at TEXT NOT NULL DEFAULT (datetime('now')),
  UNIQUE(user_id, label)
);

CREATE TABLE IF NOT EXISTS note_concepts (
  note_id TEXT NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
  concept_id TEXT NOT NULL REFERENCES concepts(id) ON DELETE CASCADE,
  PRIMARY KEY (note_id, concept_id)
);

CREATE TABLE IF NOT EXISTS concept_links (
  id TEXT PRIMARY KEY DEFAULT (lower(hex(randomblob(16)))),
  user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  concept_a_id TEXT NOT NULL REFERENCES concepts(id) ON DELETE CASCADE,
  concept_b_id TEXT NOT NULL REFERENCES concepts(id) ON DELETE CASCADE,
  strength REAL NOT NULL DEFAULT 0,
  is_manual INTEGER NOT NULL DEFAULT 0,
  created_at TEXT NOT NULL DEFAULT (datetime('now')),
  UNIQUE(concept_a_id, concept_b_id)
);
```

---

## 7. Installation, Setup & User Quickstart

### Prerequisites
- **Node.js**: v18+ (Node v20+ recommended).
- **Zero Docker**: No external database or Docker container required.

### 1-Click Launch (Windows)
Double-click [`run.bat`](file:///d:/tests/bob_project/concept-linker/run.bat).

### Terminal Commands
```bash
# Clone or open directory
cd concept-linker

# Install all packages across root, server, and client
npm run install:all

# Start both backend and frontend dev servers
npm run dev
```

---

## 8. API Reference Documentation

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/notes` | List all incidents & traces with severity and status |
| `POST` | `/api/notes` | Ingest new trace / test failure (accepts JSON or file upload) |
| `GET` | `/api/notes/:id` | Get incident details and raw trace content |
| `PUT` | `/api/notes/:id` | Update incident title, trace, severity, or status |
| `DELETE` | `/api/notes/:id` | Delete incident |
| `POST` | `/api/notes/:id/re-extract` | Re-run diagnostic entity extraction on incident |
| `GET` | `/api/notes/:id/concepts` | Get extracted entities for an incident |
| `GET` | `/api/graph` | Fetch hierarchical tree nodes, edges, and failure correlations |
| `POST` | `/api/links` | Create manual correlation link between entities |
| `DELETE` | `/api/links/:id` | Remove correlation link |
| `POST` | `/api/concepts/cleanup` | Normalize and deduplicate diagnostic entities |
| `GET` | `/api/user/export` | Export full workspace backup JSON |

---

## 9. Verification, Benchmarks & Testing Results

### Automated Test Suite (100/100 Tests Passing)
```bash
cd server
npm test
```

```
PASS unit __tests__/notes.unit.test.js
PASS integration __tests__/auth.test.js
PASS unit __tests__/graphLinks.unit.test.js
PASS unit __tests__/concepts.tags.unit.test.js
PASS unit __tests__/concepts.unit.test.js
PASS unit __tests__/extractConcepts.unit.test.js
PASS unit __tests__/auth.unit.test.js
PASS unit __tests__/extractText.unit.test.js
PASS unit __tests__/nlpClient.unit.test.js
PASS unit __tests__/autoLinker.unit.test.js

Test Suites: 10 passed, 10 total
Tests:       100 passed, 100 total
Snapshots:   0 total
Time:        11.004 s
```

### Performance Benchmarks
- **Trace Ingestion & Parsing**: $< 50\text{ ms}$ per trace.
- **Tree Layout Computation**: $< 35\text{ ms}$ for 100+ nodes using Dagre.
- **Cold Boot Time**: $< 1.5\text{ seconds}$ from terminal command to interactive UI.
