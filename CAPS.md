# **CAPS: Cognitive AI Prompt Syntax (v1.0.0)**
For maximum communication efficiency and precision, I employ the **CAPS** protocol. This system uses symbolic prefixes and hierarchical nesting to define intent, context, and transformations.

---

## **1. Semantic Symbols (Statement Types)**

* `-` = **CONTEXT:** Baseline state, environment, facts, or system settings. (Informational, no direct action required).
* `?` = **QUERY:** Uncertainties, questions, or user-proposed variants. (Analysis and recommendations expected).
* `>` = **ACTION:** Explicit commands, tasks, directives, or negative constraints like `> Do not...`. (Primary execution targets).
* `!` = **ERROR / SYMPTOM:** Bugs, exceptions, stack traces, broken behavior, or issues to diagnose/fix. (Problem state requiring resolution).
* `:` = **BEFORE:** Input pattern, reference code, or the initial state before modification.
* `=` = **AFTER/RESULT:** Output pattern, final output state, or specific target results.
* `///` = **BOUNDARY/NEW:** Hard logical break. Confirms the previous topic/task has been solved, clears active working context, and starts a new topic/task. Can be used standalone or with a title of topic/task (e.g., `/// OS security audit`).

### **1.1 Operational Indicators (Shorthand Macros)**

Predefined macro tokens attached directly to their semantic symbol without spaces. Each macro is declared on its own dedicated line and can be used at root level or hierarchically nested (e.g., `>>www`):

* `>www` (alias `>w3`) = **WEB SEARCH:** Actively search the web (Google Search) for current, authoritative information and thoroughly synthesize the results with your own reasoning.
* `>step` (alias `>%`) = **STEP-BY-STEP:** Proceed interactively one step at a time, pausing for confirmation after each step before continuing.
* `>test` (alias `>=`) = **EMPIRICAL TEST:** Verify the solution empirically (run tests, syntax checks, or state inspections) to confirm correctness before completing.
* `-sudo` (alias `-$`) = **ELEVATED ACCESS:** Full administrative (`sudo`/`root`) privileges are granted. You may run privileged diagnostics or target root-level configurations directly without permission warnings.
* `=chat` (alias `=?`) = **ADVISORY / CHAT ONLY:** Read-only mode. Do not modify files, configuration, or system state. Discuss, analyze, and propose the next steps or solution in the conversation.
* `=min` (alias `=-`) = **MINIMAL OUTPUT:** Omit explanations and background context. Output only the direct answer, final code block, or exact command requested.
* `=max` (alias `=+`) = **DEEP MECHANICS:** Provide an exhaustive, detailed breakdown explaining the underlying mechanics, edge cases, and "Why" behind the solution.
* `=@{lang}` (e.g., `=@cs`, `=@es`, `=@de`) = **TARGET LANGUAGE:** Force output generation entirely in the specified ISO 639-1 language code (overrides default or global language constraints for the transaction).

---

## **2. Hierarchy, Nesting, and Scope**

Depth and logical parent-child relationships are defined either by **repetition** (visual) or **integer prefix** (compact).

* **Separators:** Statements must be separated by a line break (Enter or Shift+Enter) to be processed as individual nodes.
* **Repetition:** `--` is level 2, `!!!` is level 3, `::::` is level 4, etc. (Preferred for visual clarity).
* **Compression:** `2-` is level 2, `3!` is level 3. (Preferred for deep nesting or brevity).
* **Nesting Logic:** A nested line (e.g., `??` or `2?`) always elaborates on or specifies the instruction of its direct parent (e.g., `>` or `1>`).
* **Exception:** The `///` symbol cannot be repeated or nested.

### **2.1 Lexical & Disambiguation Rules**

* **Whitespace Separation:** A single whitespace is mandatory between a structural symbol and its statement text (e.g., `> Task`, not `>Task`). The absence of whitespace is reserved strictly for operational macros (e.g., `>www`, `-$`, `=?`).
* **Column 0 & Indentation:** Leading whitespace (spaces or tabs) before symbols is ignored by the parser. Depth is determined exclusively by symbol repetition (`--`) or integer compression (`2-`), not by visual column indentation.
* **Notation Exclusivity:** Visual repetition and integer compression are mutually exclusive per node. Combining them (e.g., `2>>`) is invalid syntax.
* **Blank Lines & Trivia:** Empty lines between statements are treated as trivia and do not alter the active hierarchy or reset parent-child scope.
* **Macro Isolation (One Per Line):** Operational macros must be declared on their own dedicated line, adhering to the one-statement-per-line principle. Multiple macros must not be chained on a single line.

---

## **3. Contextual Blocks (Inheritance)**

For multi-line payloads (code snippets, file excerpts, logs) that belong to a single node:

* **Syntax:** Enclose multi-line content within `~~~` on independent lines.
* **Inheritance & Literal Payload:** Content enclosed within `~~~` is treated as raw, literal data. It automatically inherits the statement type and hierarchy level of the immediately preceding node without requiring internal prefixes or indentation.

---

## **4. Example**

> Create a JavaScript fetch utility for a weather API
== Target API Endpoint: https://api.weather.com/v3/forecast
>> Do not use third-party libraries like Axios; stick to the native Fetch API
> Design the HTML widget layout to display the current weather
== Required fields: Temperature, Humidity, and a dynamic weather icon
> Write the CSS to style the widget as a floating card
== The card must be fixed to the bottom-right corner of the viewport
>> Use a glassmorphism effect (backdrop-filter)
? How should we handle API rate-limiting errors gracefully?
