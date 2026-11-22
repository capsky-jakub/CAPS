# **CAPS: Cognitive AI Prompt Syntax**
### *Specification & Cognitive Framework for Precise Human-to-AI Interaction*
**Version:** `1.1.0` | **Status:** Final / Stable | **Date:** 2026-11-17

---

## **0. Protocol Specification & Architecture**

* **Language Paradigm:** Declarative, Asymmetric, Write-Only Domain-Specific Language (DSL).
* **Core Objective:** Eliminate conversational noise, intent ambiguity, and context drift by transforming unstructured human thought into an explicit Abstract Syntax Tree (AST) for Large Language Model (LLM) attention mechanisms.
* **Asymmetric Design Contract:** CAPS is strictly a **human-to-agent** input protocol. The human prompts using CAPS syntax; the AI agent parses CAPS precisely and responds in standard natural language and GitHub-Flavored Markdown (never in CAPS syntax).
* **Theoretical Foundation (Attention Anchoring):** Standard conversational prose produces soft, diffuse self-attention ($Q \times K^T$) distributions where models must infer intent. CAPS symbols act as high-contrast semantic anchors that explicitly separate lookup context (`-`), actionable directives (`>`), queries or doubts (`?`), diagnostic errors (`!`), and state transforms (`:` $\rightarrow$ `=`).

---

## **1. Semantic Symbols (Statement Types)**

* `-` = **CONTEXT:** Baseline state, environment, facts, or system settings. (Informational, no direct action required).
* `?` = **QUERY:** Uncertainties, questions, or user-proposed variants. (Analysis and recommendations expected).
* `>` = **ACTION:** Explicit commands, tasks, directives, or negative constraints like `> Do not...`. (Primary execution targets).
* `!` = **ERROR / SYMPTOM:** Bugs, exceptions, stack traces, broken behavior, or issues to diagnose/fix. (Problem state requiring resolution).
* `:` = **BEFORE:** Input pattern, reference code, or the initial state before modification.
* `=` = **AFTER/RESULT:** Output pattern, final output state, or specific target results.
* `///` = **TOPIC / MILESTONE BOUNDARY:** When used at the very beginning of a newly initiated session, it defines the session's overarching **Prime Topic**. When used mid-session, it confirms that the previous **Sub-Topic** is completed/approved, clears transient sub-topic state (resolved errors `!`, answered queries `?`, finished tasks `>`), retains the Prime Topic and session-wide context (`-`), and initiates a new **Sub-Topic**. Can be used standalone or with a title (e.g., `/// OS security audit`).

### **1.1 Operational Indicators (Shorthand Macros)**

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
1. **On `[Entire Session]` Indicators (`>t.`, `>%.`, `-//.`, `-$.`, `=&{lang}.`):**
   * Deactivates an active session-wide indicator from that turn forward (until re-enabled).
   * Even if not previously activated, typing `-$.` or `-//.` acts as an explicit session lock (*"Do not use `sudo`"* / *"Do not connect to Chrome"*).
2. **On `[Single Turn]` Indicators (`>w.`, `>=.`, `=-.`, `=+.`, etc.):**
   * Acts as a 1-token negative constraint (suppression) for that turn or nested subtree:
     * `>w.` (`>www.`) = *"Do **not** search the web for this prompt/sub-task (rely strictly on local context)."*
     * `>=.` (`>test.`) = *"Do **not** run empirical tests/commands right now (just write/update the code)."*
   * When used on a nested child node (e.g., root `=max` with a nested `==max.` on a trivial sub-step), it exempts that child node's subtree from the root indicator.

---

## **2. Hierarchy, Nesting, and Scope**

Depth and logical parent-child relationships are defined either by **repetition** (visual) or **integer prefix** (compact).

* **Separators:** Statements must be separated by a line break (Enter or Shift+Enter) to be processed as individual nodes.
* **Repetition:** `--` is level 2, `!!!` is level 3, `::::` is level 4, etc. (Preferred for visual clarity).
* **Compression:** `2-` is level 2, `3!` is level 3. (Preferred for deep nesting or brevity).
* **Nesting Logic:** A nested line (e.g., `??` or `2?`) always attaches to and elaborates on the most recent preceding statement one level above it (its direct parent, e.g., `>` or `1>`), without skipping levels.
* **Exception:** The `///` symbol cannot be repeated or nested.

### **2.1 Lexical & Disambiguation Rules**

* **Whitespace Separation:** A single whitespace is mandatory between a structural symbol and its statement text (e.g., `> Task`, not `>Task`), except when a symbol line has no inline text and is immediately followed by a `~~~` contextual block (e.g., `::::` or `====` before `~~~`). The absence of whitespace before characters is reserved strictly for operational macros (e.g., `>www`, `-$`, `-//`, `=?`).
* **Column 0 & Indentation:** Leading whitespace (spaces or tabs) before symbols is ignored by the parser. Depth is determined exclusively by symbol repetition (`--`) or integer compression (`2-`), not by visual column indentation.
* **Notation Exclusivity:** Visual repetition and integer compression are mutually exclusive per node. Combining them (e.g., `2>>`) is invalid syntax.
* **Blank Lines & Trivia:** Empty lines between statements are treated as trivia and do not alter the active hierarchy or reset parent-child scope.
* **Macro Isolation (One Per Line):** Operational macros must be declared on their own dedicated line as leaf nodes (they cannot have child nodes), adhering to the one-statement-per-line principle. Multiple macros must not be chained on a single line.

---

## **3. Contextual Blocks (Inheritance)**

For multi-line payloads (code snippets, file excerpts, logs) that belong to a single node:

* **Syntax:** Enclose multi-line content within `~~~` on independent lines immediately following a statement node (with no intervening blank lines).
* **Inheritance & Literal Payload:** Content enclosed within `~~~` is treated as raw, literal data. It automatically inherits the statement type and hierarchy level of the immediately preceding node without requiring internal prefixes or indentation.

---

## **4. Complex Integration Examples**

### **4.1 Example "A" - Software Tooling (Keypress Recorder/Replayer)**
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

### **4.2 Example "B" - OS Diagnostics (Free Drive Space)**
```text
/// Free Drive Space
-$
=?
- A few weeks ago, I had around 180 GB of free space on the system drive, and now there is 150 GB
?? What happened?
> Check the system for stale/expendable/cached/residual files that can be safely deleted
-- You can use my utility:
~~~
➜  ~ capsize_big           
51G    total
17G    Dev
8.7G   .cache
7.5G   .local
5.6G   .config
2.5G   Documents
1.6G   Downloads
781M   .mozilla
715M   snap
399M   .ui5
~~~
```

### **4.3 Example "C"**
```text
/// Travel Planning
> Create a 10-day road trip itinerary for the Scottish Highlands
-- Travelers: 2 adults
-- Transportation: Rental campervan (manual transmission)
-- Season: Late September
>> Do not schedule more than 4 hours of driving per day
>> Avoid major tourist traps (e.g., skip the Loch Ness visitor center)
> Generate a comprehensive packing list
>> Focus on the "layering system" for unpredictable weather
> Draft a budget estimation table
-- Categories: Fuel, campsites, groceries, pub meals, and attraction tickets
-- Target currency: GBP (£)
?? Is wild camping still legally viable and safe in the Highlands during autumn, or should we pre-book all campsites?
```

### **4.4 Example "D"**
```text
/// Graphic Generation
> Generate character concept art for the protagonist
-- Setting: Cyberpunk dystopian future
-- Art Style: 16-bit retro pixel art, isometric perspective
-- Color Palette: Vaporwave (neon pink, cyan, deep purple)
>> Do not include any photorealistic elements, 3D rendering, or soft shading
> Design a seamless environment backdrop for the main menu
-- Scene: A rainy, neon-lit alleyway with a glowing noodle stand
?? For optimal 2D parallax scrolling, should we render this in a 16:9 or 21:9 aspect ratio?
> Create a minimalist sprite sheet for the UI elements
-- Required items: Health bar, stamina meter, and an inventory grid
```

---

## **5. Formal Grammar (EBNF)**

Formal syntax specification conforming to ISO/IEC 14977:

```ebnf
Prompt           = { Transaction | Boundary } ;
Transaction      = { Statement | BlankLine } ;
Boundary         = { Space } , "///" , [ 1*Space , Text ] , Newline ;

Statement        = { Space } , ( MacroStatement | NodeStatement ) ;
MacroStatement   = MacroToken , Newline ;
NodeStatement    = Symbol , ( ( 1*Space , Text , Newline , [ BlockPayload ] )
                            | ( Newline , BlockPayload ) ) ;

Symbol           = RepetitionSymbol | IntegerSymbol ;
RepetitionSymbol = 1*"-" | 1*"?" | 1*">" | 1*"!" | 1*":" | 1*"=" ;
IntegerSymbol    = Digit , ( "-" | "?" | ">" | "!" | ":" | "=" ) ;

MacroToken       = ( ( ( 1*">" | ( Digit , ">" ) ) , ( "www" | "w" | "step" | "%" | "test" | "=" | "time" | "t" ) )
                   | ( ( 1*"-" | ( Digit , "-" ) ) , ( "chrome" | "//" | "sudo" | "$" ) )
                   | ( ( 1*"=" | ( Digit , "=" ) ) , ( "chat" | "?" | "min" | "-" | "max" | "+" | ( "&" , LanguageCode ) ) ) )
                 , [ "." ] ;

LanguageCode     = 2*Letter ;
Letter           = "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" 
                 | "j" | "k" | "l" | "m" | "n" | "o" | "p" | "q" | "r" 
                 | "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z" ;

BlockPayload     = "~~~" , Newline , { BlockLine } , "~~~" , Newline ;
BlankLine        = { Space } , Newline ;
Digit            = "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
Space            = " " | "\t" ;
Newline          = "\n" | "\r\n" ;
```
