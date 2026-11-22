# CAPS Showcase: Web, Cloud & Firebase Development

Curated real-world **CAPS (Cognitive AI Prompt Syntax)** prompts for full-stack web applications, GCP/Firebase Hosting, emergency API key rotation, and Git deployment workflows.

---

```text
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



/// Finding optimal approach for parallel development and GitHub repository
- I moved my main development workspace from a cloud web IDE (cloud-ide.example.com) to LLM-frontier1 a few months ago
-- now, the legacy web IDE preserves the older version as a backup
-- now, my local git repo keeps the newest version
--- every time I make a change in the myapp project (via LLM-frontier1), I call my function 'mygit_equal' defined in functions.zsh to synchronize everything
- The legacy web IDE has an option to publish to GitHub
-- however, it performs a force push and overwrites the target main/master branch without merging
??? if I performed the sequence "1. Legacy web IDE push to GitHub, 2. pull from GitHub to my local repository", would it completely overwrite my newest local version with the older snapshot?
>>> I need to prevent this overwrite behavior
???? What about creating a new independent git branch for my local git repo and GitHub so it can never be affected by cloud-ide.example.com?
```
