# CAPS Showcase: Shell Scripting & CLI Utilities

Curated real-world **CAPS (Cognitive AI Prompt Syntax)** prompts for Bash/Zsh scripting, CLI pipeline automation, file synchronization monitors, OCR workflows, and security auditing utilities.

---

/// New tool
= to have a new tool/command 'myconv_raw' that:
== searches the current working directory (pwd) and converts all found .docx files into .md
=== the conversion will be based on and leverage the existing /home/user/Dev/Shell/Util/myconv_docx_to_md.sh in pandoc mode
=== the converted .md files will be placed into the new subdir _raw
> place the new tool into /home/user/Dev/Shell/Util/convx/myconv_raw
?? I guess /home/user/Dev/Shell/Util/myconv_docx_to_md.sh should be relocated as well



/// Finding screenshots
- Some time ago I was configuring and troubleshooting the Fiori Launchpad and its Travel app
> Find related/respective screenshots
= a brief list of filepaths (no more than 30) that I can pipe into my alias 'oo'



///
> Extract your modification actions/steps into a standalone script (Shell, Python, whatever)
== I will be able to repeat the exact modification in the future



///
> Create a new script that can be applied/run next time after a fresh ...
== It should be based on the knowledge acquired here



/// New upgrade
- I realized that it is inconvenient when a newly created screenshot immediately disappears from the root folder /home/user/Pictures/Screenshots
-- Sometimes I need to work instantly with new screenshots and I cannot because they have just been moved to a different location
= to have simple and direct access to the latest screenshot taken
?? What if we created an auxiliary symlink in /home/user/Pictures/Screenshots that points to the newly moved and renamed original screenshot file?



/// Converting screenshots to text files
- I have a working service /home/user/Dev/Shell/Util/unify_screenshots.sh that periodically checks the folder /home/user/Pictures/Screenshots and renames the screenshots I created
== I want to have a similar service that converts every new screenshot to text (OCR) and saves it to /home/user/Pictures/Screenshots/OCR under the same name with a .txt suffix
--- I do not need an absolutely perfect and all-powerful OCR tool because the converted text files will be analyzed/read by an AI/LLM (so no extra layout/graphics preservation is needed)
??? What is the current situation in the OCR tool market?
???? Is there a clear winner?



/// Antigravity session finder/searcher
- I am a fairly advanced Antigravity user and I have many chats/sessions every day
- Sometimes, when dealing with something, I remember that I already figured it out in a previous Antigravity session, but I do not remember which one
-- Usually I remember the keywords/terms/fragments used in the session
::: e.g. the term "GFXOFF" used in:
~~~
Workspace:      ~
Project Config: ~/.gemini/config/projects/default-cli-project.json
Log File:       ~/.gemini/antigravity-cli/log/cli-20260724_150547.log
Conversation:   00000000-0000-0000-0000-000000000000
~~~
= I want to have a command/tool 'ggfind' executable directly in the terminal (Shell script, function, Python, whatever...) that:
== expects 1x mandatory argument 'used_term' (as a regex string) to search across all Antigravity sessions
== lists all found sessions containing 'used_term'
=== optimally the list should display: session date/time, session name, session ID
?? I guess a shell script would be enough



/// Duplication of myscanpers design/structure
- I have another working tool 'myscanhack' implemented solely as an agent skill in /home/user/Dev/AI/Tools/.agents/skills/myscanhack
-- Currently, it is runnable only through the function 'myscanhack' in @/home/user/.oh-my-zsh/custom/functions.zsh
-- There is no automation (only manual execution in my Zsh and the agent's response)
::: Sample run in Zsh:
~~~
➜  Tools git:(master) myscanhack 
I will start by listing the contents of the global customizations root directory at `/home/user/.gemini/config` to see if there is any custom command or skill related to `myscanhack`.
I will list the contents of the Antigravity application directory `/home/user/.gemini/antigravity-cli` to inspect available resources or scripts.
I will list our current permission grants to see what directories and tools we have access to.
...
### Summary of Actions & Results
* **Analysis Status:** Completed. All rows of network sockets, process tables, cron directories, systemd timers/services, and shell startup configurations were manually checked.
* **Findings:** The system is clean. No active indicators of compromise, spyware, backdoors, cryptominers, or unauthorized persistence mechanisms were detected.
* **Verification:** Correlated dynamic high-ports (IPC for Antigravity tools), local gsconnect sessions (port 1716), and systemd timers (user-defined backup/sync automations), validating all
~~~
= 'myscanhack' works fine, but I would like it to be automated the same way as 'myscanpers' is
== 1. autorun - new user or system systemd service scheduled daily at 12:30
== 2. log into journald - if the agent detects a threat (malware), it will log a message into journald with the group/tag MY_LOG_SECURITY_THREAT
??? I have an idea to instruct the agent to call an external script in /home/user/Dev/Shell/System/myscanhack_log_threat, intended only for logging purposes so we do not have to move the existing /home/user/Dev/AI/Tools/.agents/skills/myscanhack/scripts/gather.sh
== 3. Zsh notification - terminal alerts on startup if an existing log entry with MY_LOG_SECURITY_THREAT is found



///
- I had to contact my external agent/expert and we figured it out
-- The core logic works now
>>> Review the respective code
- When I restart the system, the service is not running and files are not synced
>> Configure the service to start up automatically
- I do not need to track and sync the dirs ~/Pictures and ~/Downloads
>> Exclude those dirs
>>> Do not append them directly to the COMMON_EXCLUDES array
---- It is used by another backup service and I still want to back up ~/Pictures and ~/Downloads



///
- the command:
~~~
echo "$(date +'%Y-%m-%d %H:%M:%S')|$ETYPE|$EPATH" >> "$HOME/.local/state/lsyncd/sync_history.log"
~~~
?? Q: Is this a proper/recommended/appropriate way of logging information?
--- Until now, I have handled all kinds of logging via journalctl



///
> Create a simple daily activity monitor 'mymoni_sync_cloud'
== it should be a function placed into @/home/user/.oh-my-zsh/custom/functions.zsh
== it should report/output the last {n} synchronized objects for today
== it should display simple statistics at the beginning of the monitor output
??? Total created, Total updated, Total deleted, Total number of affected directories
---- I am not sure if it is difficult to distinguish between created/updated/deleted
>>>>> If it complicates the whole solution/tool, do not break it down and implement just 'Total copied'
== it should accept 2 optional input arguments/parameters
=== {n} will be the 1st optional argument/parameter of the new function
==== default: 30
== the 2nd optional argument should switch between simple/compact vs. full/detailed display mode
=== values: S/F
==== default: S
=== the compact mode will print each synced object on 1 row
---- meaning max 200 chars with respect to my terminal window size and font settings
???? Row structure: Timestamp, Filename, Filesize, ...
>>>>> You can add more information (up to the 200-char limit) if you consider it beneficial
--- I do not have an exact design in mind for the detailed display mode structure
---- Design it your own way
