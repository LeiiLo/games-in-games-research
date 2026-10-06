# ReSkate Trainer

**Repository:** [andrewnakas/reskate-trainer@e749bd8](https://github.com/andrewnakas/reskate-trainer/tree/e749bd833becc2960d1088f66ca7d297fd14e329) (default branch, 6 Oct 2026)  
**Route:** trainer and installer for a game runtime (a fork of another project)  
**What we read:** installer scripts and README. Nothing was run.  
**AI credit (as the project states it):** commits co-authored by Claude.

**What we learned** (from its installers)

- Its batch installers check file or process names only, so they'd pass on the wrong build. Hash or fingerprint the target.
- It uses the backup DLL as the only "backup done" marker and copies the DLL and launcher in separate steps. Write the marker last, after every file is verified.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.
