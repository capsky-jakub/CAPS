<p align="center">
  <img src="assets/caps-logo.svg" alt="CAPS: Cognitive AI Prompt Syntax Logo" width="460">
</p>

# **CAPS: Cognitive AI Prompt Syntax**
### *A Symbolic, Hierarchical DSL & Cognitive Framework for Deterministic Human-to-AI Interaction*

[![Specification Version](https://img.shields.io/badge/specification-v1.1.0-blue?style=flat-square)](CAPS-DSL.md)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/license-CC_BY--SA_4.0-lightgrey?style=flat-square)](LICENSE)

**CAPS (Cognitive AI Prompt Syntax)** is a declarative, asymmetric, write-only Domain-Specific Language (DSL) and human-to-agent communication protocol engineered to maximize intent precision, token efficiency, and deterministic parsing in Large Language Model (LLM) workflows.

---

## Why CAPS?

1. **Transformer Attention Anchoring ($Q \times K^T$):** Standard conversational prose produces diffuse self-attention distributions where models must infer intent from surrounding filler. CAPS prefixes (`-`, `?`, `>`, `!`, `:`, `=`, `///`) serve as high-contrast semantic anchors that explicitly separate static context from executable directives, uncertainties, diagnostic symptoms, and state transitions.
2. **Hierarchical Scope Isolation:** Indentation via symbol repetition (`--`, `>>>`, `::::`) or integer compression (`2-`, `3>`) constructs a strict Abstract Syntax Tree (AST). Child nodes elaborate exclusively on their direct parent node, preventing local constraints or sub-queries from leaking globally across the prompt.
3. **Cognitive Pre-Processing (QIQO):** Beyond machine parsing, CAPS acts as a *"Quality In, Quality Out"* mental framework—guiding the author to decouple baseline facts (`-`), desired actions (`>`), target results (`=`), uncertainties or queries (`?`), and diagnostic errors (`!`) before prompting.
4. **Asymmetric Design Contract:** CAPS is strictly a **Human-to-AI** input syntax. The user prompts in CAPS; the AI agent parses the hierarchy deterministically and responds in standard natural language and Markdown.

---

## Quick Syntax Overview

| Symbol | Type | Purpose |
| :--- | :--- | :--- |
| `-` | **CONTEXT** | Baseline state, environment, facts, or system settings (informational). |
| `?` | **QUERY** | Uncertainties, questions, or proposed variants requiring analysis. |
| `>` | **ACTION** | Explicit commands, tasks, directives, or negative constraints (`> Do not...`). |
| `!` | **ERROR / SYMPTOM** | Bugs, exceptions, stack traces, or broken behavior to diagnose/resolve. |
| `:` | **BEFORE** | Input pattern, reference code, or initial state prior to modification. |
| `=` | **AFTER / RESULT** | Output pattern, target state, or desired transformation result. |
| `///` | **TOPIC / MILESTONE BOUNDARY** | Establishes the overarching **Prime Topic** at session start, or confirms the previous **Sub-Topic** is completed and initiates a new **Sub-Topic** mid-session. |
| `~~~` | **BLOCK FENCE** | Encloses multi-line literal payloads (code, logs, configs) inheriting the parent node's scope. |

### Operational Shorthand Macros

| Macro | Alias | Scope | Meaning |
| :--- | :--- | :--- | :--- |
| `>www` | `>w` | `Turn` | **Web Search:** Search the web for current, authoritative info. |
| `>test` | `>=` | `Turn` | **Empirical Test:** Run tests, syntax checks, or state inspection. |
| `=chat` | `=?` | `Turn` | **Advisory Only:** Read-only analysis; do not modify files or state. |
| `=min` | `=-` | `Turn` | **Minimal Output:** Direct answer or code only, no explanations. |
| `=max` | `=+` | `Turn` | **Deep Mechanics:** Exhaustive breakdown of edge cases and *"Why"*. |
| `>step` | `>%` | **`Session`** | **Step-by-Step:** Pause for confirmation after each step. |
| `>time` | `>t` | **`Session`** | **Timestamp:** Append local `YYYY-MM-DD HH:MM:SS` to responses. |
| `-chrome` | `-//` | **`Session`** | **Browser CDP:** Connect to live Chrome CDP session when needed. |
| `-sudo` | `-$` | **`Session`** | **Elevated Access:** Full `sudo`/`root` privileges granted. |
| `=&{lang}` | — | **`Session`** | **Target Language:** Force output in ISO 639-1 code (e.g., `=&cs`, `=&de`). |

> **Scope & Negation (`.` Suffix):**
> * `Turn` applies to the current prompt (or nested subtree); `Session` persists across `///` boundaries.
> * Append `.` to turn **OFF** or **suppress** a macro:
>   * On `Session` macros: deactivates or locks out (`>t.`, `>%.`, `-$.` = *no `sudo`*, `-//.` = *no Chrome*).
>   * On `Turn` macros: suppresses for this turn/subtree (`>w.` = *no web search*, `>=.` = *skip tests*).

---

## Repository Structure

* **[CAPS-DSL.md](CAPS-DSL.md)** — The full canonical RFC-style specification, including architectural foundations, lexical rules, multi-domain integration examples, and the formal ISO/IEC 14977 EBNF grammar.
* **[CAPS-DSL.txt](CAPS-DSL.txt)** — Exact plaintext mirror of `CAPS-DSL.md` for universal ingestion across tools and web interfaces.
* **[CAPS.md](CAPS.md)** — Condensed, token-optimized specification (~5.7 KB) designed for direct embedding into AI system prompts, custom instructions, and agent rule files (e.g., `GEMINI.md`, `CLAUDE.md`, `.cursorrules`).
* **[LICENSE](LICENSE)** — Creative Commons Attribution-ShareAlike 4.0 International (`CC-BY-SA-4.0`) license text.
* **[assets/](assets/)** — Official CAPS brand assets in vector ([caps-logo.svg](assets/caps-logo.svg)) and full-resolution raster ([caps-logo.png](assets/caps-logo.png)) formats.
* **[examples/](examples/)** — Curated, anonymized real-world CAPS prompts across 7 domains:
  * [AI Engineering & Agent Orchestration](examples/CAPS_log-bestof_AI.md?plain=1)
  * [Linux & OS Administration](examples/CAPS_log-bestof_Linux.md?plain=1)
  * [Personal & Legal Workflows](examples/CAPS_log-bestof_Personal.md?plain=1)
  * [Python Automation & Document Processing](examples/CAPS_log-bestof_Python.md?plain=1)
  * [SAP Enterprise Development (ABAP Cloud, RAP, CDS, Fiori, BTP)](examples/CAPS_log-bestof_SAP.md?plain=1)
  * [Shell Scripting & CLI Utilities](examples/CAPS_log-bestof_Shell.md?plain=1)
  * [Web, Cloud & Firebase Development](examples/CAPS_log-bestof_Web.md?plain=1)

---

## Quick Start: Adding CAPS to Your AI Agent

To enable native CAPS comprehension in your AI coding assistant or LLM environment, add the contents of **[CAPS.md](CAPS.md)** to your global system prompt or agent rules file alongside these two directives:

```markdown
* The user can prompt you using the **CAPS (Cognitive AI Prompt Syntax)**.
* When CAPS is used, parse the user's inputs according to the semantic symbols (`-`, `?`, `>`, `!`, `:`, `=`, `///`) and hierarchical nesting defined by the CAPS protocol.
* CAPS is strictly for user inputs. Do not use CAPS syntax in your responses.
```

---

## License & Attribution

**CAPS: Cognitive AI Prompt Syntax** is © 2026 **Jakub Capsky** and is licensed under the **[Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0)](LICENSE)** ([summary](https://creativecommons.org/licenses/by-sa/4.0/)).

---

## Support

If CAPS saves you time or provides extra value in your workflows, you are welcome to support its continued evolution:

[![GitHub Sponsors](https://img.shields.io/badge/GitHub_Sponsors-ea4aaa?style=flat-square&logo=github-sponsors&logoColor=white)](https://github.com/sponsors/capsky-jakub)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-F16061?style=flat-square&logo=ko-fi&logoColor=white)](https://ko-fi.com/capskyjakub)
