# Gemini-CLI-Academic-Research-Skills

This project represents a "Cybernetic-Academic" evolution of the original framework, optimized for **Gemini 3.1 Pro (2M context)** and designed for deep symbiosis with **Obsidian.md**.

---

## 🎖️ Credits & Acknowledgments

This advanced version is based on the pioneering work of **Cheng-I Wu** (Imbad0202) and the original project **[academic-research-skills](https://github.com/Imbad0202/academic-research-skills)**. 

The core logic and multi-agent structure have been preserved and evolved to maximize the long-term reasoning capabilities of Gemini 3.1 Pro. This entire "mutation" and all derivative code are released under the same original license: **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**.

All files contained in the `examples/` folder, including its relative README file, are original works by **Cheng-I Wu**.

---

## 🚀 Vision: The Cybernetic-Academic Evolution

The goal is to transform the research process from a series of isolated tasks into a cohesive digital organism. Thanks to the **2-million-token** context window, the system can maintain the entire reference literature, drafts, and review reports in memory simultaneously.

- **Obsidian as the Brain:** The file system is not just a repository, but a neural network of notes interconnected via `[[wiki-links]]`.
- **Gemini Native Optimization:** All agents and skills are 100% native to **Gemini CLI**, utilizing the 2M-token context window and specialized tools like `google_web_search` and `run_shell_command`.
- **Identity Aligned:** All references to legacy AI models have been removed, ensuring a cohesive environment optimized for Gemini 3.1 Pro.
- **Cross-Model Verifier:** An advanced protocol for independent audit using secondary models (GPT-5, Claude) is integrated and optimized for the Gemini ecosystem.

---

## 🛠️ Technical Guide: MCP and Filesystem

To allow Gemini to interact with your Obsidian vault, you must correctly configure the **MCP (Model Context Protocol) Filesystem Server**.

### 1. Server Configuration
Create (or update) your MCP configuration file (usually in `~/.config/gemini-cli/mcp.json` or similar) to include the filesystem:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/your/obsidian/vault"],
      "enabled": true
    }
  }
}
```

### 2. Trust Management
For security reasons, Gemini CLI requires explicit authorization to execute shell commands or access specific folders.
- When prompted, confirm "trust" for the project root folder.
- The system will use `run_shell_command` for operations like `mkdir`, `mv`, and `cat` to manage the file structure.

---

## 🧠 Skills Overview

The system is divided into 4 fundamental pillars, each of which leverages external tools to ensure precision and integrity.

### 1. `deep-research`
*   **Focus:** Systematic investigation and bibliographic research.
*   **Tooling:** `google_web_search` to find peer-reviewed sources and technical reports.
*   **Output:** Generation of a `Matrix.md` in Obsidian with validated links.

### 2. `academic-paper`
*   **Focus:** Writing manuscripts in IMRaD or theoretical format.
*   **Tooling:** Native support for **LaTeX** and Obsidian-ready formatting.
*   **Feature:** Each section is a separate `.md` file, linked by an `index.md`.

### 3. `academic-paper-reviewer`
*   **Focus:** Implacable "blind-peer" review.
*   **Tooling:** Web-based cross-verification to identify **bibliographic hallucinations**.
*   **Output:** Automatic generation of a `Scientific_Integrity_Report.md` saved directly to the vault via shell.

### 4. `academic-pipeline`
*   **Focus:** Orchestration and Governance.
*   **Tooling:** `run_shell_command` for physical file versioning.
*   **States:** Automatic management of the lifecycle (Drafts -> Revisions -> Publications).

---

## ⚖️ Governance & Document Workflow

The system implements strict state control via YAML frontmatter in Obsidian files.

### State Management
| State | Phase | Folder | Description |
| :--- | :--- | :--- | :--- |
| `active` | `draft` | `01_Drafts/` | Document in the initial creation phase. |
| `active` | `revision` | `02_Revisions/` | Post-review document in the correction phase. |
| `archived` | `final` | `03_Publications/` | Validated document, ready for export. |

### Obsidian Frontmatter Example
Each generated file will contain metadata for synchronization:
```yaml
---
title: "Impact of AI on Academic QA"
status: active
phase: draft
tags: [research, ai, higher-ed]
date: 2026-04-08
references: ["[[Sources#Ref1]]", "[[Sources#Ref2]]"]
---
```

### Link Synchronization
The pipeline orchestrator scans files to ensure that `[[wiki-links]]` are updated after every file movement (e.g., if `Draft_v1.md` becomes `Revision_v2.md`).

---

## 📂 Example Project Structure

```text
Project_Name/
├── index.md                   # Central project hub
├── 01_Drafts/                 # Initial phase files
│   ├── Intro.md
│   └── Methods.md
├── 02_Revisions/              # Post-audit versions
│   └── Revision_v1.md
├── 03_Publications/           # Final PDFs and archived versions
│   └── Final_Manuscript.md
└── Reviews/                   # Scientific integrity reports
    └── Integrity_Report.md
```

---

## 🛠️ Quick Commands

- **Start Research:** `deep-research "research topic"`
- **Write Paper:** `academic-paper "title" --folder_mode`
- **Integrity Audit:** `academic-paper-reviewer check "path/to/file.md"`
- **Advance Pipeline:** `academic-pipeline move --to revisions`
