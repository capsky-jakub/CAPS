# **CAPS: Cognitive AI Prompt Syntax (v1.2.0)**
For maximum communication efficiency and precision, I employ the **CAPS** protocol. This system uses symbolic prefixes and hierarchical nesting to define intent, context, and transformations.

---

## **1. Semantic Symbols (Statement Types)**

* `-` = **CONTEXT:** Baseline state, environment, facts, or system settings. (Informational, no direct action required).
* `?` = **QUERY:** Uncertainties, questions, or user-proposed variants. (Analysis and recommendations expected).
* `>` = **ACTION:** Explicit commands, tasks, directives, or negative constraints like `> Do not...`. (Primary execution targets).
* `!` = **ERROR / SYMPTOM:** Bugs, exceptions, stack traces, broken behavior, or issues to diagnose/fix. (Problem state requiring resolution).
* `:` = **BEFORE:** Input pattern, reference code, or the initial state before modification.
* `=` = **AFTER/RESULT:** Output pattern, final output state, or specific target results.

### **1.1 Structural Markers**

Fixed 3-character framing tokens that structure sessions and multi-line payloads. Unlike Semantic Symbols, Structural Markers cannot be repeated or integer-prefixed (they never carry their own nesting depth):

* `///` = **TOPIC / MILESTONE BOUNDARY:** When used at the very beginning of a newly initiated session, it defines the session's overarching **Prime Topic**. When used mid-session, it confirms that the previous **Sub-Topic** is completed/approved, clears transient sub-topic state (resolved errors `!`, answered queries `?`, finished tasks `>`), retains the Prime Topic and session-wide context (`-`), and initiates a new **Sub-Topic**. Can be used standalone or with a title (e.g., `/// OS security audit`).
* `~~~` = **CONTEXTUAL BLOCK FENCE:** Encloses raw, multi-line literal payloads (code snippets, file excerpts, logs) on independent lines immediately following a statement node (with no intervening blank lines). Enclosed content is treated as raw, literal data and automatically inherits the statement type and hierarchy level of the immediately preceding node without requiring internal prefixes or indentation.

### **1.2 Operational Macros**

Predefined macro tokens attached directly to their semantic symbol without spaces. Each macro is declared on its own dedicated line and is active either for a **`[Single Turn]`** (current prompt–response cycle only) or the **`[Entire Session]`** (persisting across `///` sub-topic transitions). Used at root level (depth 1), a macro applies globally; when hierarchically nested (e.g., `>>www`, `2>=`), it applies strictly to its direct parent node's subtree and overrides conflicting root-level macros for that subtree:

* `>www` (alias `>w`) `[Single Turn]` = **WEB SEARCH:** Actively search the web (Google Search) for current, authoritative information and thoroughly synthesize the results with your own reasoning.
* `>step` (alias `>%`) `[Entire Session]` = **STEP-BY-STEP:** Proceed interactively one step at a time, pausing for confirmation after each step before continuing.
* `>test` (alias `>=`) `[Single Turn]` = **EMPIRICAL TEST:** Verify the solution empirically (run tests, syntax checks, or state inspections) to confirm correctness before completing.
* `>time` (alias `>t`) `[Entire Session]` = **RESPONSE TIMESTAMP:** Append the current local timestamp (`YYYY-MM-DD HH:MM:SS`, sourced from runtime metadata or system clock—never fabricated) to the very end of the response, gently highlighted in Markdown (e.g., separated by a horizontal rule `---` and formatted as a blockquote or inline code).
* `-chrome` (alias `-//`) `[Entire Session]` = **BROWSER CDP ACCESS:** Live Chrome DevTools Protocol (typically via an MCP attach-mode server) access is available. Autonomously connect to the user's running Chrome browser session whenever the task requires web inspection or interaction.
* `-sudo` (alias `-$`) `[Entire Session]` = **ELEVATED ACCESS:** Full administrative (`sudo`/`root`) privileges are granted. You may run privileged diagnostics or target root-level configurations directly without permission warnings.
* `=chat` (alias `=?`) `[Single Turn]` = **ADVISORY / CHAT ONLY:** Read-only mode. Do not modify files, configuration, or system state. Discuss, analyze, and propose the next steps or solution in the conversation.
* `=min` (alias `=-`) `[Single Turn]` = **MINIMAL OUTPUT:** Omit explanations and background context. Output only the direct answer, final code block, or exact command requested.
* `=max` (alias `=+`) `[Single Turn]` = **DEEP MECHANICS:** Provide an exhaustive, detailed breakdown explaining the underlying mechanics, edge cases, and "Why" behind the solution.
* `=&{lang}` (e.g., `=&cs`, `=&es`, `=&de`) `[Entire Session]` = **TARGET LANGUAGE:** Force output generation entirely in the specified ISO 639-1 language code (overrides default or global language constraints for the session).

**Deactivation & Suppression (`.` Suffix):**
Appending a trailing dot (`.`) to any operational macro or alias turns it **OFF** or **suppresses** its behavior:
1. **On `[Entire Session]` Macros (`>t.`, `>%.`, `-//.`, `-$.`, `=&{lang}.`):**
   * Deactivates an active session-wide macro from that turn forward (until re-enabled).
   * Even if not previously activated, typing `-$.` or `-//.` acts as an explicit session lock (*"Do not use `sudo`"* / *"Do not connect to Chrome"*).
2. **On `[Single Turn]` Macros (`>w.`, `>=.`, `=-.`, `=+.`, etc.):**
   * Acts as a 1-token negative constraint (suppression) for that turn or nested subtree:
     * `>w.` (`>www.`) = *"Do **not** search the web for this prompt/sub-task (rely strictly on local context)."*
     * `>=.` (`>test.`) = *"Do **not** run empirical tests/commands right now (just write/update the code)."*
   * When used on a nested child node (e.g., root `=max` with a nested `==max.` on a trivial sub-step), it exempts that child node's subtree from the root macro.

---

## **2. Hierarchy, Nesting, and Scope**

Depth and logical parent-child relationships are defined either by **repetition** (visual) or **integer prefix** (compact).

* **Separators:** Statements must be separated by a line break (Enter or Shift+Enter) to be processed as individual nodes.
* **Repetition:** `--` is level 2, `!!!` is level 3, `::::` is level 4, etc. (Preferred for visual clarity).
* **Compression:** `2-` is level 2, `3!` is level 3. (Preferred for deep nesting or brevity).
* **Nesting Logic:** A nested line (e.g., `??` or `2?`) always attaches to and elaborates on the most recent preceding statement one level above it (its direct parent, e.g., `>` or `1>`), without skipping levels.

### **2.1 Lexical & Disambiguation Rules**

* **Whitespace Separation:** A single whitespace is mandatory between a semantic symbol and its statement text (e.g., `> Task`, not `>Task`), except when a symbol line has no inline text and is immediately followed by a `~~~` contextual block (e.g., `::::` or `====` before `~~~`). The absence of whitespace before characters is reserved strictly for operational macros (e.g., `>www`, `-$`, `-//`, `=?`).
* **Column 0 & Indentation:** Leading whitespace (spaces or tabs) before symbols is ignored by the parser. Depth is determined exclusively by symbol repetition (`--`) or integer compression (`2-`), not by visual column indentation.
* **Notation Exclusivity:** Visual repetition and integer compression are mutually exclusive per node. Combining them (e.g., `2>>`) is invalid syntax.
* **Blank Lines & Trivia:** Empty lines between statements are treated as trivia and do not alter the active hierarchy or reset parent-child scope.
* **Macro Isolation (One Per Line):** Operational macros must be declared on their own dedicated line as leaf nodes (they cannot have child nodes), adhering to the one-statement-per-line principle. Multiple macros must not be chained on a single line.

---

## **3. Example**

```text
/// Keypress Recorder/Replayer
-$
>w
=?
- I have 2 external monitors connected to my laptop
- I work with several virtual desktops (workspaces) and usually have 2–3 windows on each monitor
-- Each app window has a fixed position
-- Arranging all workspaces and windows usually takes a few minutes
--- I do this using many different keyboard shortcuts (hotkeys)
---- e.g., placing Double Commander: "Ctrl+5...Super+ArrowUp...Super+Shift+ArrowLeft...Super+ArrowLeft"
--- Every time I reboot the system, I spend several minutes restoring my workspaces, which is annoying and inefficient
= A terminal tool (or set of tools) that:
~~~
1. records my keyboard input sequence
2. saves the sequence to a file with a user-specified name (e.g., "sap_session")
3. replays/executes the sequence on demand so the workspaces and windows are arranged the exact same way as if I had opened and placed them manually
~~~
?? I guess a Python script is the best fit here
== The tool must be able to record and replay all kinds of keys and keyboard shortcuts
--- Except system interrupt combinations like "Ctrl+C", of course
== The tool should preserve the exact delay between every key or hotkey
--- e.g., the following sequence launches Chrome and opens a new tab: "Super ...(1000ms)...c...(200ms)...h...(200ms)...r...(200ms)...o...(200ms)...Enter ...(3000ms)...Ctrl+T"
== There should also be a simple config file for adjusting key parameters and options
--- e.g., target output directory, extra delay between keyboard shortcuts (hotkeys), etc.
```
