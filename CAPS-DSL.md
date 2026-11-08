# **CAPS: Cognitive AI Prompt Syntax**
### *Specification & Cognitive Framework for Deterministic Human-to-AI Interaction*
**Version:** `1.0.0` | **Status:** Final / Stable | **Date:** 2026-11-08

---

## **0. Protocol Specification & Architecture**

* **Language Paradigm:** Declarative, Asymmetric, Write-Only Domain-Specific Language (DSL).
* **Core Objective:** Eliminate conversational noise, intent ambiguity, and context drift by transforming unstructured human thought into a deterministic Abstract Syntax Tree (AST) for Large Language Model (LLM) attention mechanisms.
* **Asymmetric Design Contract:** CAPS is strictly a **human-to-agent** input protocol. The human prompts using CAPS syntax; the AI agent parses CAPS deterministically and responds in standard natural language and GitHub-Flavored Markdown (never in CAPS syntax).
* **Theoretical Foundation (Attention Anchoring):** Standard conversational prose produces soft, diffuse self-attention ($Q \times K^T$) distributions where models must infer intent. CAPS symbols act as high-contrast semantic anchors that mathematically separate lookup context (`-`), actionable directives (`>`), queries or doubts (`?`), diagnostic errors (`!`), and state transforms (`:` $\rightarrow$ `=`).

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

## **4. Complex Integration Examples**

### **4.1 Example "A" - Software Migration (Python)**
```text
> Migrate legacy Python 2 script to Python 3.14  
-- Environment: Ubuntu 24.04 LTS (Noble Numbat)  
!! Legacy script fails with 'ModuleNotFoundError: No module named urllib2' and blocks on slow sockets  
>> Do not use 'future' or 'six' compatibility libraries  
>> Enforce strict typing (mypy)  
>> Optimize the network socket handling  
??? Should we use 'asyncio' for this specific I/O block?
--- Implementation draft:
::::  
~~~  
def get_data(url):  
    import urllib2  
    return urllib2.urlopen(url).read()  
~~~  
====  
~~~  
async def get_data(url: str) -> bytes:  
    import aiohttp  
    async with aiohttp.ClientSession() as session:  
        async with session.get(url) as response:  
            return await response.read()  
~~~
```

### **4.2 Example "B" - Web Development (API Integration)**
```text
> Create a JavaScript fetch utility for a weather API
-- Target API Endpoint: https://api.weather.com/v3/forecast
>> Do not use third-party libraries like Axios; stick to the native Fetch API
> Design the HTML widget layout to display the current weather
-- Required fields: Temperature, Humidity, and a dynamic weather icon
> Write the CSS to style the widget as a floating card
-- The card must be fixed to the bottom-right corner of the viewport
-- Use a glassmorphism effect (backdrop-filter)
! Unhandled promise rejection when API returns HTTP 429 (Too Many Requests)
?? How should we handle API rate-limiting errors gracefully?
??? Error handling suggestion:
::::
~~~
// Basic legacy catch
catch (error) {
    console.error("Fetch failed", error);
}
~~~
====
~~~
// Desired target: Graceful UI degradation
catch (error) {
    if (error.status === 429) {
        displayWidgetError("Rate limit exceeded. Please try again later.");
    } else {
        displayWidgetError("Unable to load weather data.");
    }
}
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
Boundary         = "///" , [ Space , Text ] , Newline ;

Statement        = ( MacroStatement | NodeStatement ) ;
MacroStatement   = MacroToken , Newline ;
NodeStatement    = Symbol , Space , Text , Newline , [ BlockPayload ] ;

Symbol           = RepetitionSymbol | IntegerSymbol ;
RepetitionSymbol = 1*"-" | 1*"?" | 1*">" | 1*"!" | 1*":" | 1*"=" ;
IntegerSymbol    = Digit , ( "-" | "?" | ">" | "!" | ":" | "=" ) ;

MacroToken       = ">www"  | ">w3" 
                 | ">step" | ">%" 
                 | ">test" | ">="
                 | "-sudo" | "-$"
                 | "=chat" | "=?" 
                 | "=min"  | "=-" 
                 | "=max"  | "=+" 
                 | "=@" , LanguageCode ;

LanguageCode     = 2*Letter ;
Letter           = "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" 
                 | "j" | "k" | "l" | "m" | "n" | "o" | "p" | "q" | "r" 
                 | "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z" ;

BlockPayload     = "~~~" , Newline , { BlockLine } , "~~~" , Newline ;
Digit            = "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
Space            = " " | "\t" ;
Newline          = "\n" | "\r\n" ;
```
