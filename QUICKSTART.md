# Quick Start Guide (Cybernetic-Academic Research)

Configure your research environment powered by **Gemini 3.1 Pro** and **Obsidian.md** in less than 5 minutes.

---

## 🛠️ Step 1: Environment Configuration

Ensure you have `gemini-cli` installed and the **MCP Filesystem Server** configured for your Obsidian vault.

1.  **Configure MCP**: Add your vault to the `mcp.json` file:
    ```json
    {
      "mcpServers": {
        "filesystem": {
          "command": "npx",
          "args": ["-y", "@modelcontextprotocol/server-filesystem", "/your/obsidian/vault/path"]
        }
      }
    }
    ```
2.  **Trust**: Launch Gemini CLI and press `tab` to trust the project folder when prompted.

---

## 🚀 Step 2: Rapid Skill Launch

Gemini will automatically select the correct skill based on your request. Here are the primary triggers:

### 🔍 Phase 1: Deep Research (`deep-research`)
*   **What it does:** Scans the web with Google Search, validates sources, and creates a literature matrix in Obsidian.
*   **Example:** *"Start a systematic research on the impact of generative AI on student assessment."*

### ✍️ Phase 2: Manuscript Writing (`academic-paper`)
*   **What it does:** Writes the paper section by section using LaTeX for formulas and wiki-links for references.
*   **Example:** *"Help me write a paper based on the previous research. Use folder_mode for Obsidian."*

### 🛡️ Phase 3: Integrity Audit (`academic-paper-reviewer`)
*   **What it does:** Acts as an implacable reviewer. Verifies if citations are real or hallucinations and saves an integrity report in the vault.
*   **Example:** *"Perform a peer review of this draft and verify all cited sources."*

### 🔄 Phase 4: Lifecycle Management (`academic-pipeline`)
*   **What it does:** Moves files between folders (Drafts -> Revisions -> Publications) and updates YAML metadata.
*   **Example:** *"The paper is ready. Move everything to the Revisions folder and update the project status."*

---

## 📊 Quick Choice Table

| Objective | Skill to Invoke | What happens in Obsidian |
| :--- | :--- | :--- |
| Explore a vague idea | `deep-research` | A `Sources.md` note is created with validated links. |
| Write a chapter | `academic-paper` | A `.md` file is generated with frontmatter and LaTeX. |
| Verify sources | `academic-paper-reviewer` | An `Integrity_Report.md` is created via Shell. |
| Archive a project | `academic-pipeline` | Files are renamed and moved to `03_Publications/`. |

---

## 💡 Tips for Success

1.  **Context Window:** Don't be afraid to load many documents. Gemini 3.1 Pro handles up to 2M tokens; it can "read" hundreds of papers simultaneously.
2.  **Wiki-Links:** Always use double brackets `[[ ]]` when asking Gemini to refer to other notes.
3.  **Shell Trust:** When you see a shell command (`mkdir`, `mv`), confirm its execution. This is how the system maintains order in your vault.

---

## Next Steps
- Read the [README.md](README.md) for advanced technical details.
- See the [examples](examples/showcase/) to see a complete pipeline in action.
