---
description: Review the current uncommitted changes against Dextall conventions and gotchas.
---

Review the current working-tree changes in this Dextall sub-repo.

1. Determine the diff. Since each top-level folder is its own git repo, run
   `git status` and `git diff` inside the relevant project folder (not the
   `C:\Projects\Dextall` root, which is not a repo).
2. Delegate to the `code-reviewer` agent, which applies the `dextall-gotchas`,
   `dotnet-conventions`, and `dependency-impact` skills.
3. Report findings grouped as **Blocking / Should-fix / Nit**, each with a
   `file:line` reference and a concrete reason.

Do not modify code — this is review only.
