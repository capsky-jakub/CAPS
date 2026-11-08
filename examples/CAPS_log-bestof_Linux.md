# CAPS Showcase: Linux & OS Administration

Curated real-world **CAPS (Cognitive AI Prompt Syntax)** prompts for Linux (Ubuntu/GNOME) system administration, package management (APT vs. Snap), storage diagnostics, desktop environment tuning, and snapshot retention.

---

/// Firefox
? I should have both versions: APT and Snap
== I want to keep them both
- when I press "Super ... Firefox ..." it launches the Snap version, which is not desired
-- I also have a desktop icon named FirefoxSnap
-$



/// Free drive space
-$
- Not long ago (a few weeks ago) I remember having around 180GB of free space on the system drive, and now there is 150GB
?? What happened?
> Check the system for stale/expendable/cached/residual files that can be safely deleted
-- you can use my utility:
~~~
➜  ~ mysize_big           
51G    total
17G    Dev
8.7G    .cache
7.5G    .local
5.8G    .gemini
5.6G    .config
2.5G    Documents
1.6G    Downloads
781M    .mozilla
715M    snap
492M    .antigravity
491M    .antigravity-ide
399M    .ui5
237M    sap
82M    SoftMaker
~~~
=?



/// Conv .docx to .md
- I have a regular .docx document
-- it is not too complex, just a few chapters and a table of contents at the start
- I tried converting it into Markdown, but I was not satisfied with the output
-- the converter did not preserve the document heading structure using '#', etc.
- I heard about some CLI conversion tools that could possibly do a better job
-- pandoc, ..., etc.
> recommend some reliable and popular CLI tools
=?
-$



/// Ubuntu 26 update
- Recently I updated the system from 24 to 26
- The new version works properly and I noticed several fixes (especially related to multi-monitor settings), which is great!
- I have not noticed any new significant/handy UI improvements or features
?? Does the new version come with other enhancements and features that I might appreciate?
--- I am especially curious about UI and window/desktop controls
>w3



/// Keeping only the latest 5x snapshots
- I did not enable Timeshift's built-in scheduling and snapshot retention functionality
-- Instead, I have my own scheduling mechanism based on systemd timers and the script @backup_local.sh
- I noticed that after a few months there are dozens of old snapshots that will probably never be needed
= Keep only a predefined number of the latest snapshots
>w3



/// CoolApp issue - freezing after system suspend/sleep or locking
- I am struggling with CoolApp freezing
-- It never happens right after a system reboot
--- CoolApp works stably for hours
-- It happens after system suspend/sleep/lock when I switch to a different OS workspace (virtual desktop)
--- I have spent many hours and tried almost everything - nothing has resolved the issue yet (e.g. AMD PSR, turning animations off, network pinging, etc.)
> Deeply scan the project/workspace files and initialize the context file (GEMINI.md) first
>> Analyze mainly the previous session information and recaps


/// Firefox as APT package
- Ubuntu comes with Firefox pre-installed as a Snap package
- I decided to install the APT version of Firefox alongside the existing Snap version
> Rename the current 'Firefox' Snap desktop entry to 'FirefoxSnap'
> Install the stable up-to-date Firefox APT version on my system
?? It is not possible to install it via the standard 'sudo apt install firefox' because that APT package actually points to the default Firefox Snap wrapper
> Move the Firefox (APT) browser cache from SSD to RAM via PSD (profile-sync-daemon) the same way as Chrome
?? Is it also possible to move/set the browser cache directory to RAM?



/// Ubuntu tweaking
= I would like to configure/tune my Ubuntu OS for optimal performance
-- I have already switched off visual animations
-- Desktop Linux distributions often include default background services and daemons that I may not need, causing higher resource consumption
>>> check it out
> Scan my system and recommend actions if possible
>w3
== Stability FIRST: I do not want to risk system failures for a 0.000001% speed gain
