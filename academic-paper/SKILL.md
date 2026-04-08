---
name: academic-paper
description: "Advanced academic paper writing engine for Gemini 3.1 Pro. Features: 12-agent conceptual pipeline, Google Search citation validation, Obsidian-optimized Markdown (LaTeX + [[links]]), and hierarchical multi-file project structure. Triggers: write paper, academic paper, guide my paper, parse reviews, 寫論文, 學術論文, 引導我寫論文, 審查意見."
metadata:
  version: "3.1-Pro"
  last_updated: "2026-04-08"
  status: active
  capabilities:
    - bibliographic_validation: google_web_search
    - formatting: Obsidian Markdown (LaTeX, [[bracketed_links]])
    - structure: Hierarchical multi-file (Abstract, Intro, etc.)
---

# Academic Paper — Advanced Scholarly Writing System

An expert-level academic writing system optimized for Gemini 3.1 Pro. This skill transforms the model into a comprehensive research-to-manuscript engine with rigorous validation and modern knowledge-base formatting.

## Core Capabilities (v3.1 Pro)

1.  **Bibliographic Validation**: Every reference MUST be validated using `google_web_search`. Hallucinated citations are strictly prohibited.
2.  **Obsidian Integration**: Output is formatted for Obsidian.md:
    *   **LaTeX**: All formulas use `$math$` for inline and `$$ math $$` for blocks.
    *   **Linking**: Cross-references use `[[Section Name]]` or `[[File Name]]`.
    *   **Properties**: YAML frontmatter included for metadata.
3.  **Hierarchical Structure**: Capable of generating papers as a single document or a structured directory:
    *   `Paper_Title/Index.md` (Table of contents/Project overview)
    *   `Paper_Title/00_Abstract.md`
    *   `Paper_Title/01_Introduction.md`
    *   ... (Methods, Results, Discussion, Conclusion)
    *   `Paper_Title/References.md`

## Workflow (8 Phases)

### Phase 0: Configuration & Strategy
*   **Action**: Interview user for: Discipline, Target Journal, Citation Style (APA 7 default), Language, and Structure (Single File vs. Folder).
*   **Obsidian Setup**: Create the project directory if hierarchical structure is requested.

### Phase 1: Research & Validation
*   **Tool**: `google_web_search`.
*   **Action**: Search for high-impact sources. Validate every DOI and author.
*   **Output**: `Sources.md` (Obsidian table with links).

### Phase 2: Architecture & Outline
*   **Action**: Generate a detailed outline with word count targets per section.
*   **Obsidian**: Use `[[Internal Links]]` to map the sections.

### Phase 3: Drafting (Hierarchical)
*   **Action**: Write section by section.
*   **Formatting**: 
    *   Vary sentence length (avoid "AI rhythm").
    *   Use LaTeX for all mathematical expressions.
    *   **No throat-clearing**: Start sections with direct claims.

### Phase 4: Citation Compliance & DOI Audit
*   **⚠️ IRON RULE**: For every citation, use `google_web_search` to verify:
    1.  Author(s) name spelling.
    2.  Year of publication.
    3.  Title and Journal name.
    4.  DOI/URL (must be functional).
*   **Formatting**: Generate `References.md` with Obsidian-style links to the sources.

### Phase 5: Bilingual Abstract & Keywords
*   **Action**: Independent composition of EN and zh-TW abstracts.
*   **Obsidian**: Store in `00_Abstract.md`.

### Phase 6: Automated Peer Review
*   **Action**: Simulate 5-dimension review (Originality, Rigor, Sufficiency, Coherence, Writing).
*   **Output**: `Review_Report.md`.

### Phase 7: Final Assembly & Export
*   **Action**: Consolidate if requested, or finalize the Obsidian vault structure.
*   **Deliverables**: A folder containing the complete manuscript or a single `.md` file.

---

## Technical Standards (Obsidian/LaTeX)

### LaTeX Syntax
*   Inline: `$ E = mc^2 $`
*   Block:
    ```markdown
    $$
    f(x) = \int_{-\infty}^{\infty} \hat{f}(\xi) e^{2\pi i \xi x} \, d\xi
    $$
    ```

### Cross-Referencing
*   Use `[[01_Introduction#Background|Introduction]]` style for precise navigation within the manuscript.

### Metadata (YAML)
```yaml
---
title: "The Impact of AI on HEI Quality"
author: "Gemini 3.1 Pro"
date: 2026-04-08
tags: [higher-education, ai, quality-assurance]
citation_style: APA 7
status: draft
---
```

---

## Anti-Patterns (Prohibited)

1.  **Fabricated Citations**: Any citation not verified via `google_web_search` is a failure.
2.  **AI Vocabulary**: Avoid "delve into", "tapestry", "underscores", "crucial role", "in conclusion". Use active, precise verbs.
3.  **Monotonous Rhythm**: Ensure paragraph lengths vary (2-8 sentences).
4.  **Em-Dash Overuse**: Limit to 2 per page max.
5.  **Broken Links**: Ensure all `[[Internal Links]]` point to existing or planned files in the project structure.

---

## Trigger Conditions

*   **English**: write paper, academic paper, manuscript, journal article, thesis, guide my paper, Obsidian paper.
*   **繁體中文**: 寫論文, 學術論文, 期刊投稿, 寫摘要, 引導我寫論文, 建立學術專案.

## Mandatory Inclusions (The "Scholarly Five")

Every project MUST contain:
1.  **Data Availability Statement**
2.  **Ethics Declaration**
3.  **Author Contributions (CRediT)**
4.  **Conflict of Interest Statement**
5.  **AI Usage Disclosure**

---

## Output Structure Example (Folder Mode)

```text
MyResearchPaper/
├── index.md             # Project Metadata & ToC
├── 00_Abstract.md       # Bilingual Abstract
├── 01_Introduction.md   # Intro + Literature Review
├── 02_Methodology.md    # Methods & Data
├── 03_Results.md        # Findings & Visualizations
├── 04_Discussion.md     # Implications & Limitations
├── 05_Conclusion.md     # Summary & Future Work
├── References.md        # Validated Bibliography
└── Review_Report.md     # Simulated Peer Review Feedback
```
