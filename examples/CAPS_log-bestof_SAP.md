# CAPS Showcase: SAP Development (ABAP Cloud, RAP, CDS, Fiori, BTP)

Curated real-world **CAPS (Cognitive AI Prompt Syntax)** prompts for SAP enterprise development, including ABAP Cloud, ABAP RESTful Application Programming Model (RAP), Core Data Services (CDS), SAP Fiori Elements, Eclipse ADT, and SAP BTP administration.

---

/// VSCode installation
-$
>w3
> Install VSCode with Antigravity and the following plugins/extensions:
-- Antigravity IDE extensions
--- https://antigravity.google/docs/ide/extensions/
-- SAP Fiori tools extension
-- ADT for Visual Studio Code
> Do not uninstall the Antigravity IDE yet, provided it does not cause any collisions/defects/inconsistencies
= migration/switching my secondary IDE from Antigravity to VSCode



/// New agy skill 'my...' for local->remote synchronization
- I have a working tool/command 'mygit_locrem_s4_travel' that also integrates 'mygit_xpull_adt'
- Once the AI agent (you) finishes/completes a job/task related to backend SAP development (RAP, CDS, OData), I have to run 'mygit_locrem_s4_travel' manually in another terminal window
= to have a new skill 'my...' that the AI agent automatically runs after completing its task
== the skill instructs to run 'mygit_locrem_s4_travel'
=== only if backend (CDS, OData, Fiori) artifacts were changed (/home/user/Dev/SAPDEV/S4-CLI/src/DEMO_TRAVEL/RAP)
---- there is no need to run 'mygit_locrem_s4_travel' for local changes (/home/user/Dev/SAPDEV/S4-CLI/src/DEMO_TRAVEL/FIORI)
?? Not sure about a proper skill name
--- what about 'myexp...', 'mysync...', ...
?? Not sure if the new skill should be mentioned/referenced in GEMINI.md or another rule file
--- /home/user/Dev/SAPDEV/S4-CLI/src/DEMO_TRAVEL/GEMINI.md
--- /home/user/Dev/SAPDEV/S4-CLI/src/DEMO_TRAVEL/.agents/rules/rap.md
--- /home/user/Dev/SAPDEV/S4-CLI/src/DEMO_TRAVEL/.agents/rules/fiori.md



///
> Explain this technology to me
?? What are the typical real-world use cases?
?? When can it come in handy for me?
?? Do I need it at the beginning of my SAP BTP/RAP/Cloud/Fiori studies?



/// New github repo for ABAP cloud project (AbapGit)
- I have a GitHub account with several repos
- You should have the GitHub MCP server active
- I created a new ABAP Cloud project in Eclipse ADT
>> Create a new empty GitHub repo named 'SAP_BTP_TRIAL_DEMO_2'
??? I guess I will have to push the project to GitHub manually via Eclipse ADT (abapGit plugin)



///
> Hide the column "Changed by" on the main list page
-- you can utilize registered skills related to SAP (RAP, CDS, etc.) before starting analysis/design/implementation
--- in case you get lost and cannot find the right solution, activate 'mysap_jit_skills'
>>w3
> Do not test it (I will test it manually)
> Do not change artifacts in the @[FIORI] repo
-- the task should be feasible just by editing a few text files inside the @[RAP] repo



/// Button on detail page
- I followed a beginner's RAP tutorial and successfully developed a new Fiori app!
- The new app works fine in general
-- The tutorial only outlines this feature conceptually without full sample code
--- There should also be a button for uploading an attachment file:
~~~
...
c. In addition your application allows you to upload pictures of type jpg and png.
~~~
> Do not change the codebase yet
>> Just suggest a solution



/// Cloud IDE OAuth login issue
-$
- I have my own cloud trial account
-- I can log into the web console without any issues using my account "user@example.com"
- I ran the automated environment setup booster for development
- I have created a new cloud project in my desktop IDE
-- When I try to open this project, an IDE popup 'Logon for project...' appears with a button "Open Logon page in browser"
--- I clicked the button and another browser popup 'Sign in' appears with the link:
https://00000000-0000-0000-0000-000000000000.cloud-env.example.com/auth/reentranceticket?redirect-url=http%3A%2F%2Flocalhost%3A34163%2Fide%2Fredirect&_=12345678901234
---- Even though I use the same web console credentials, they are not accepted and the browser popup appears again
----- I am unable to log in...



/// BTP Trial re-creation
- My previous BTP Trial account would have expired in 5 days (90 days in total)
-- I have deleted it and created a new one
--- I expect I acquired new account IDs, etc.
> Review/validate all my CLI tools and context information
-- I have already successfully connected the Eclipse cloud project with service key @...
