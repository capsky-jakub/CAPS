# CAPS Showcase: Python Automation & Document Processing

Curated real-world **CAPS (Cognitive AI Prompt Syntax)** prompts for Python tool development, input event recording/replaying, YAML data formatting, and automated batch document/directory restructuring.

---

///
> Install these 3 tools: pandoc, docling, mammoth-cli
>> check if they are already present on my system
~~~
➜  ~ mytype mammoth
🔍 Searching for 'mammoth' across layers...

[Native/Apt]

[Flatpak]

[Snap]

[Binary Path]
mammoth is /home/user/.local/share/mise/installs/python/3.14.2/bin/mammoth
~~~
> create a simple script (Shell/Python)
== it accepts 1x mandatory parameter: 'filepath'
== it sequentially converts the file using all 3 tools into 3 final Markdown files
=== it adds a short suffix indicating the CLI tool used



/// Accelerated recording
- I created the macro 'ws2' and it works fine, but it is too slow
- I tried to globally speed it up with the extra option '-s 3'
-- It broke the macro because heavy apps like Firefox or Chrome need more time to initialize, load, and display... so the subsequent keypresses are not sent to the displayed app and end up corrupting the whole workspace/virtual-desktop arrangement
> Back up the current macro
> Create an optimized/accelerated version 'ws2acc' that:
== respects the lag between app launching/triggering (Super...F...i...r...Enter) and the app window appearing
== speeds up lightweight and immediate actions like regular keystrokes



///
- nice
- Other improvement ideas for the recording .yaml file:
?? What if we strip/remove the event value labels "key:, action:, delay_ms:" and rely on fixed positions in the list?
?? What if we strip/remove the key value prefix "KEY_"?
??? It is always the same anyway, right?
?? What if we make the event list even more legible?
??? e.g. every "column" would have a fixed character width
::
~~~
events:
- {key: KEY_LEFTMETA, action: down, delay_ms: 0}
- {key: KEY_LEFTMETA, action: up, delay_ms: 90}
- {key: KEY_C, action: down, delay_ms: 1470}
- {key: KEY_C, action: up, delay_ms: 111}
~~~
==
~~~
events:
- {LEFTMETA        down      0}
- {LEFTMETA        up      90}
- {C             down      1470}
- {C             up      111}
~~~



/// New subcommand 'record_extend' (-e)
- I have an existing recording .yaml file and I want to extend it with another key macro (a new sequence of key events/presses)
- Between the existing set of events and the newly appended one:
== there will be an appropriate default delay
??? I guess 2000ms is optimal
== there will be a brief separating comment line



/// Keypress recorder/replayer
- I have 2 external monitors connected to my laptop
- I work with several virtual desktops (workspaces) and usually have 2-3 windows on each monitor
-- Each app window has its fixed position
-- Arranging all workspaces and windows usually takes a few minutes
--- I do this using many different keyboard shortcuts (hotkeys)
---- e.g. placing Double Commander: "Ctrl+5...Super+ArrowUp...Super+Shift+ArrowLeft...Super+ArrowLeft"
--- Every time I reboot the system, I spend several minutes restoring my workspaces, which is very annoying and inefficient
= to have a terminal tool (or set of tools) that:
~~~
1. records my keyboard input sequence
2. saves the sequence to a file with a user-specified name (e.g. "sap_session")
3. replays/executes the sequence on demand so the workspaces/windows are arranged the exact same way as if I opened and placed them manually
~~~
?? I guess the best platform is a Python script
== the tool must be able to record/replay all kinds of keys and keyboard shortcuts
--- except system interrupt ones like "Ctrl+C", of course
== the tool should respect/preserve the delay between every single key or hotkey
--- e.g. the following sequence launches Chrome and opens a new tab: "Super ...(1000ms)...c...(200ms)...h...(200ms)...r...(200ms)...o...(200ms)...Enter ...(3000ms)...Ctrl+T"
== among other things, there should also be a simple config file for adjusting/setting useful parameters/options
--- e.g. output/target file destination, extra/additional delay between keyboard shortcuts (hotkeys), ..., etc.
>w3



/// Replacing all spaces in dir names
- it is inconvenient to have whitespace characters (especially spaces) in directory paths
> replace all spaces in all [paths.docu_roots] subdirs with '_'
::
~~~
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Access Control and Security/Data Control Language (DCL)
~~~
==
~~~
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Access_Control_and_Security/Data_Control_Language_(DCL)
~~~



/// Next related task
> Try moving each .docx/.md file to its designated/related dir by matching a substring of the dirname with the Markdown heading
-- e.g. /home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/RAP_ODATA/Feature and Authorization Management in ABAP RAP.md contains:
~~~
# STUDY MANUAL: Feature Control & Authorization Control
~~~
--- the heading "Feature Control & Authorization Control" (after "STUDY MANUAL:") also exists as part of the dirname:
~~~
SAP-Help_documentation-specification/distilled/RAP_ODATA/Transactional Behavior and Business Logic (Behavior Definition & Pool)_____Feature Control & Authorization Control/
~~~
== so both 'Feature and Authorization Management in ABAP RAP.*' files are moved into 'SAP-Help_documentation-specification/distilled/RAP_ODATA/Transactional Behavior and Business Logic (Behavior Definition & Pool)_____Feature Control & Authorization Control/'
>> if there is no string match, do not move anything and leave the unmatched files in place



/// New related task
- there are many .docx files in /home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled
>> Convert each .docx file into Markdown (.md) format
=== The .md file will have the same base name with the .md suffix
>>> Do not delete the original .docx file
??? Not sure if I have a conversion tool installed



/// New task
- there are a few _MASTER_INDEX.md files inside /home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled
>> create a new dir-tree/dir-structure for each .md file following the logic below:
--- 1st level - .md file heading "### {number}. {dirname}"
--- 2nd level - .md file list entries below the "###" heading matching "* **{dirname}**"
=== e.g. "/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/_MASTER_INDEX.md" should create dirs:
~~~
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Core Data Services (CDS) and Modern Data Modeling
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Core Data Services (CDS) and Modern Data Modeling/Table Entities
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Core Data Services (CDS) and Modern Data Modeling/View Entities
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Core Data Services (CDS) and Modern Data Modeling/Projection Views
...
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Advanced Modeling of Relationships and Queries
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Advanced Modeling of Relationships and Queries/Associations
...
etc.
~~~
~~~
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Core Data Services (CDS) and Modern Data Modeling_____Table Entities
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Core Data Services (CDS) and Modern Data Modeling_____View Entities
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Core Data Services (CDS) and Modern Data Modeling_____Projection Views
...
/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/CDS/Advanced Modeling of Relationships and Queries_____Associations
...
etc.
~~~



/// New task
- there are many .docx files inside /home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled
>> create a new separate dir for each .docx file with the same name as the .docx file (without the .docx suffix)
>>> move the .docx file into its respective/eponymous dir
=== e.g. the file "/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/RAP_ODATA/Study Manual_ ABAP RAP Architecture.docx" will be moved to the new dir "/home/user/Dev/SAPDEV/Technology/ABAP_Cloud/SAP-Help_documentation-specification/distilled/RAP_ODATA/Study Manual_ ABAP RAP Architecture/"
