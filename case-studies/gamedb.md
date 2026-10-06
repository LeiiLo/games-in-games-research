# gamedb: a SQLite index of decompiled source

**Repository:** [smileybaal/gamedb@7054201](https://github.com/smileybaal/gamedb/tree/7054201291d704d64cdf37a9d3573c5d16a81fcf) (default branch, 6 Oct 2026)  
**Route:** reverse-engineering tool  
**What we read:** source archive and README. Nothing was run.

A Rust CLI that indexes already-decompiled C, C++, Java and C# into SQLite tables of functions, symbols, strings, callers and modules.

**What we learned:** it speeds up lookups; it doesn't decompile binaries, and its lightweight parsing can miss constructs. A size-and-timestamp cache can miss changed content, so force a re-index with a hash manifest when coverage matters, and point each result back to the full source and version.

---

Details describe the version linked above. Check the project's current README before relying on them. Nothing from the project is copied here; the wording and any mistakes are ours.
