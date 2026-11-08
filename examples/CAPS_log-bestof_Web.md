# CAPS Showcase: Web, Cloud & Firebase Development

Curated real-world **CAPS (Cognitive AI Prompt Syntax)** prompts for full-stack web applications, GCP/Firebase Hosting, Cloud Firestore data modeling, API key security, and Git deployment workflows.

---

/// General antihack kill-switch
- A situation may occur where the official website (https://myapp.web.app/) or its GCP/Firebase API comes under a hacker attack (DDoS, etc.)
> Create a global kill-switch (script)
== it deactivates/disables the website so it becomes unavailable
== it rotates all API keys for the project 'Proj-Main'



/// Filling/Generating GitHub README
- There is currently an empty GitHub README section
> Build this important section according to current standards
== the section will contain everything important for incoming developers and users
?? I guess it should contain some app screenshots
?? I guess it should contain some usage scenarios
?? I guess it should contain the general app URL "https://myapp.web.app"



/// Moving loading of 'fieldname1'
- The Hybrid-BYOK mechanism allows authenticated users to use a system-provided Google Maps API key instead of supplying their own
- The system-provided API key is currently stored in the user-scoped Firestore database as the field 'fieldname1' inside a document named after the user's Google account (e.g. user@example.com)
- Currently, when a new user registers, I have to go to the Firestore console and manually add 'fieldname1' into their document
-- This is annoying
> Modify the loading of 'fieldname1' from the user-scoped document to an app-scoped document named 'shared'
:: The loading path is currently: dbmain (database) > localstorage (collection) > user@example.com (document) > fieldname1 (field)
== The loading path will be: dbmain (database) > localstorage (collection) > shared (document) > fieldname1 (field)
== 'fieldname1' must be loaded only for successfully signed-in and authenticated users!
>> Find all occurrences of 'fieldname1' and modify them as necessary



/// NEW UPGRADE - "HYBRID BYOK"
- I want to make using myapp more convenient for registered/authorized users with a whitelisted Google account
-- These users are permitted in the Firestore database security rules
== They will no longer need to BYOK (for both Firebase and Maps) after implementing this "HYBRID BYOK" upgrade
- I assume that the Firebase API key is intended to be public anyway
= The key processing order/priority should be as follows:
== opt.A) The input fields "Google Maps API Key" / "Firebase API Key" in the application settings popup override everything and have the highest priority
=== no string encryption/camouflage takes place
== opt.B) If the input fields are empty, the app uses a new default public Firebase API key that is encrypted/camouflaged in the source code
--- this is clever because a potential hacker (or AI scraper bot) scraping the Firebase API key gets a completely invalid/non-existent key
=== the app will load a valid Google Maps API key from a new Firestore document field "fieldname1" (at the same level as the field "fieldname2")
=== the Maps API key must not be displayed in the input fields of the application settings popup either



/// Finding optimal approach for parallel development and GitHub repository
- I moved my main development workspace from a cloud web IDE (cloud-ide.example.com) to Antigravity a few months ago
-- now, the legacy web IDE preserves the older version as a backup
-- now, my local git repo keeps the newest version
--- every time I make a change in the myapp project (via Antigravity), I call my function 'mygit_equal' defined in functions.zsh to synchronize everything
- The legacy web IDE has an option to publish to GitHub
-- however, it performs a force push and overwrites the target main/master branch without merging
??? if I performed the sequence "1. Legacy web IDE push to GitHub, 2. pull from GitHub to my local repository", would it completely overwrite my newest local version with the older snapshot?
>>> I need to prevent this overwrite behavior
???? What about creating a new independent git branch for my local git repo and GitHub so it can never be affected by cloud-ide.example.com?
