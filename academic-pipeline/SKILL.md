---
name: academic-pipeline
description: "Orchestrator for the full academic research pipeline with automated project state tracking. Manages the transition between RESEARCH, WRITE, REVIEW, and PUBLISH states. Features: shell-based file management, automatic YAML frontmatter synchronization (status: active/archived), and wiki-link [[ ]] reference management."
metadata:
  version: "3.1-Pro"
  last_updated: "2026-04-08"
  status: active
  capabilities:
    - state_tracking: [Drafts, Revisions, Publications]
    - file_ops: run_shell_command
    - metadata_sync: YAML frontmatter update
    - link_management: Wiki-links [[ ]]
---

# Academic Pipeline — Project State & Lifecycle Orchestrator

This skill manages the physical and logical lifecycle of an academic project. It ensures that files are in the right place, metadata is accurate, and internal references are synchronized across the Obsidian vault.

## Project States

1.  **Drafts**: Initial research and first-pass writing.
    *   *Frontmatter*: `status: active`, `phase: draft`
    *   *Directory*: `Projects/[Name]/01_Drafts/`
2.  **Revisions**: Post-review or internal editing phase.
    *   *Frontmatter*: `status: active`, `phase: revision`
    *   *Directory*: `Projects/[Name]/02_Revisions/`
3.  **Publications**: Finalized, verified, and formatted versions.
    *   *Frontmatter*: `status: archived`, `phase: final`
    *   *Directory*: `Projects/[Name]/03_Publications/`

---

## Core Operations

### 1. State Tracking & File Migration
When a project moves from one stage to another, the pipeline MUST:
*   Use `run_shell_command` to move files between directories.
*   Rename files to include versioning (e.g., `Paper_v1_Draft.md` -> `Paper_v2_Revision.md`).
*   Example Shell Logic:
    ```bash
    mkdir -p "Projects/MyPaper/02_Revisions"
    mv "Projects/MyPaper/01_Drafts/Draft.md" "Projects/MyPaper/02_Revisions/Revision_v1.md"
    ```

### 2. YAML Frontmatter Synchronization
Every time a file is moved or modified, the pipeline verifies and updates the frontmatter:
*   **Verification**: Ensure `title`, `date`, `status`, and `phase` exist.
*   **Update**: Change `status: active` to `status: archived` once a paper reaches the `Publications` state.
*   **Tool**: Use `replace` or `write_file` to update the top block of the Markdown file.

### 3. Wiki-Link [[ ]] Management
The pipeline ensures that the `index.md` (Project Hub) and the individual sections are linked:
*   **Hub Sync**: The `index.md` must contain a dynamic list of `[[Links]]` to all files in the project folders.
*   **Reference Sync**: If a section (e.g., `Methodology.md`) is renamed, the pipeline must update all `[[Methodology]]` links in other files to the new name using `replace` with `allow_multiple: true`.

---

## Workflow (Transition Gates)

### Gate A: Research -> Draft (WRITE)
*   **Action**: Create project directory structure.
*   **Metadata**: Set `status: active`, `phase: draft`.

### Gate B: Draft -> Review (REVIEW)
*   **Action**: Move files to `02_Revisions/`.
*   **Metadata**: Set `phase: revision`.
*   **Link**: Update `index.md` to point to the revision folder.

### Gate C: Review -> Final (PUBLISH)
*   **Action**: Move final validated files to `03_Publications/`.
*   **Metadata**: Set `status: archived`, `phase: final`.
*   **Action**: Generate the "Scientific Integrity Report" (via `academic-paper-reviewer`) and link it in the `index.md`.

---

## Technical Instructions for the Agent

1.  **Use Shell for Structure**: Always use `run_shell_command` to maintain the directory hierarchy.
2.  **Atomic Updates**: When updating a file's state, update both its physical location and its internal YAML metadata in a single turn if possible.
3.  **Link Integrity**: Before finishing a transition, grep for the old filename to ensure no broken `[[ ]]` links remain in the project.
4.  **Reporting**: Always provide a "Pipeline Status Update" after a transition:
    ```markdown
    ### Pipeline Update: [Project Name]
    - **Current State**: [[Revisions]]
    - **Files Moved**: 3
    - **Links Updated**: 12
    - **Frontmatter**: Synchronized (status: active)
    ```

---

## Trigger Conditions

*   **English**: academic pipeline, move to revision, finalize project, archive paper, sync links, update project state.
*   **繁體中文**: 啟動流程, 進入修訂階段, 存檔論文, 同步連結, 更新專案狀態.

---

## Anti-Patterns (Prohibited)

1.  **Dangling Files**: Leaving old versions in the `Drafts` folder when the project is in `Revisions`.
2.  **Manual Link Updates**: Forgetting to update `[[ ]]` links when a file is renamed.
3.  **Inconsistent Metadata**: Having a file in the `Publications` folder with `status: active`.
4.  **Flat Structures**: Putting all files in the root without the `01_Drafts`, `02_Revisions`, `03_Publications` hierarchy.
