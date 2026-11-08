# CAPS Showcase: AI Engineering & Agent Orchestration

Curated real-world **CAPS (Cognitive AI Prompt Syntax)** prompts for AI engineering, LLM model selection, agent orchestration, custom skill definitions, context architecture (`GEMINI.md`), and CLI hooks.

---

///
- I want to build a simple landing page for my project 'ABC'
-- I want to use an LLM-EXP model for the planning phase and then switch to LLM-REG for implementation
??? Is it a good idea?
??? Which LLM-EXP reasoning effort is appropriate/sufficient for this kind of task (low, medium, high)?
=?
>w3



///
- the JSON AI-agent definition:
~~~
{
  "entries": [
    {
      "path": "/home/user/Documents/.agents/plugins"
    }
  ]
}
~~~
?? Does it mean that all plugins in the path are incorporated?
?? Is it possible to include just one particular plugin (e.g. the document-suite)?
? Is it necessary to preserve /home/user/Documents/.agents/rules?



/// New agent "Lawyer (Real Estate Specialist)"
- I have several folders in my system where I manage my real estate properties
-- mostly rentals
-- they contain contracts, amendments, etc.
--- mostly in .docx format, but I have prepared a mechanism/environment/workflow using "_raw" subfolders that will contain conversions of these .docx files into clean .md (Markdown)
= to have a specialized AI agent with the role of a lawyer specializing in real estate and related transactions
== this AI agent should be active only within the scope of the folder /home/user/Documents/Personal/RealEstate
?? It could probably be defined with similar instructions as the custom Gem agent (final hybrid)
? I have never set up such a specialized agent in the system before and I do not know the optimal approach
>w3 (find the current/modern way to create and manage such an agent in the Antigravity environment)
-- I assume the principle will be similar for other platforms like Claude Code (Anthropic) or Codex (ChatGPT)



/// Partitioning/Segmentation of the main GEMINI.md
- this root workspace/project dir for Shell development has swelled/bloated/grown too much
-- it is not sustainable to keep all context information in just one GEMINI.md file
> scan/explore the whole dir tree and assess if each dir is a standalone project; if so:
>> create a separate project context file GEMINI.md
--- there are projects that already have a GEMINI.md file created
=== it will reference the generic Shell-development context file /home/user/Dev/Shell/GEMINI.md
>> enable git versioning
> update/adjust the current generic Shell-development context file /home/user/Dev/Shell/GEMINI.md
: the current /home/user/Dev/Shell/Migr
== it is a separate project
== it will have .git versioning enabled
- there is a shared library /home/user/Dev/Shell/Lib/commons.sh
?? I am not sure how to treat it
--- it is referenced by many projects (especially backups, sync, etc.)



/// New Antigravity skill
> rename the scripts to open_chrome_debug-prof
> make the open_chrome* scripts generic (not only for eshop.example.com)
?? I guess they could accept a URL via a new script input parameter
> create the new Antigravity skill 'myuse_chrome_debug-prof'
== If I prompt the agent to execute a specific action (searching, filtering, reviewing, summarizing, adding to cart, ..., etc.) on a certain/defined webpage, the agent will use this new skill
== the skill instructs the agent to run the generic open_chrome_debug-prof script (Shell or Python) and pass the URL
== the skill instructs the agent to control/use Chrome via chrome-devtools-attach-debug-prof



///
> Provide simple usage (implementation) examples for the points below:
~~~
~~~
>> deliver the complete source code
>> explain syntax and meaning directly in the code using comments



///
> Show me what such a typical medium-sized/complex ... file might look like
>> explain syntax and meaning directly in the code using comments



/// Python management rules
- I have a separate dev folder /home/user/Dev/PyDev with an Antigravity context instruction file /home/user/Dev/PyDev/GEMINI.md
- I have a general/global context file @GEMINI.md
-- I do not want to pollute/bloat it with information/statements that may not even be applied at all
= I want to reference the PyDev context file in the global context file to instruct the AI agent to read/load it when dealing with Python (installing packages, etc.)



///
- I work with Gemini CLI 0.37.0 on Ubuntu 24
-- The user settings are in the file @settings.json
-- Gemini CLI supports the 'Hooks' concept
--- it is described on these pages:
---- https://geminicli.com/docs/hooks/
---- https://geminicli.com/docs/hooks/reference/
---- https://geminicli.com/docs/hooks/writing-hooks/
- Sometimes the agent asks me for confirmation of an action (e.g. writing to a file)
>> I want to create a new Gemini CLI Hook that raises an OS-level notification
--- it is usually displayed as a small popup window below the OS top bar
:::: e.g. Google Calendar notification, update notification, etc.
