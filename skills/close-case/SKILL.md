---
name: close-case
description: Detach the observed target repository and return the Heimdall checkout to its target-agnostic state, after a final leak sweep. Use when an investigation is finished, when the user asks to detach, remove, or clean up the target, or before handing the checkout to anyone else.
---

# Close case

Detach the target and return this checkout to the state the leak rules assume:
target-agnostic, and shareable because it is.

## Where the scripts are

Heimdall runs from a checkout or from an installed plugin, and the scripts sit
in a different place in each. Resolve it once, at the top of your shell work:

```sh
H="${CLAUDE_PLUGIN_ROOT:-.}"    # plugin cache when installed, this checkout otherwise
```

Then invoke everything as `"$H"/bin/<name>`. The variable is set in the
environment by the plugin host, so the expansion is correct in both modes
without the skill needing to know which one it is in.

## Why this exists

The gitignore keeps the target out of Heimdall's *history* — not out of
backups, archives, or anything else that reads the filesystem. An attached
`.heimdall/` and the case files under `cases/` are the two places the target
lives on this machine, and closing the case deals with both deliberately.

## Procedure

1. **Confirm the case files were handed over.** They are handoff artifacts and
   live only here; once the case closes, this machine may be the last copy.
   If any case file was never delivered, deliver it now.
2. **Run the closer:**

   ```sh
   "$H"/bin/detach-target.sh                      # sweep, list case files, detach
   "$H"/bin/detach-target.sh --purge-case-files   # additionally delete them
   ```

   It runs `"$H"/bin/check-no-leak` **before** detaching — the guard reads its
   search terms from the attachment's own metadata, so the sweep must happen
   while it still knows what to look for. A failed or inconsistent sweep
   blocks detachment (exit 7); resolve the leak first, never force past it.

   It also refuses (exit 8) on a checkout that holds readings with nothing
   attached. There the guard has no identifiers to search for, so it would
   exit 0 without having looked — and a close reported from that state is a
   claim about a sweep that never ran. Re-attach the target those readings
   are about and close properly, or hand them over and delete them by hand.
   Do not report a clean close over an exit 8.
3. **Report the outcome**: swept clean, target detached, which case files remain
   on this machine (or that they were purged). If the script refused, report
   the refusal and its reason — never paraphrase it as a close.

Never purge case files on your own initiative — deleting the possibly-last copy
of a reading is the user's call. Default is to keep and say so.

## After

The checkout is cold again: `/open-case` opens the next one. Nothing about the previous target remains outside any case files
deliberately kept.
