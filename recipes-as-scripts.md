# Recipes as Scripts

Everything so far treated a recipe as a
list of independent lines. That is
just's default and its most important
quirk. This chapter shows the quirk
biting, then the three ways past it:
**shebang recipes** that run as one
script, line prefixes that quiet or
forgive a line, and **settings** that
change the shell and feed recipes an
environment. The lab's files are
`just-lab/shebang/justfile` and
`just-lab/settings/justfile`;
transcripts `t08-shebang.txt` and
`t09-settings.txt`.

## One shell per line

Each line of an ordinary recipe runs in
its own fresh shell (`sh -cu`, by
default). Nothing carries over — not
the folder, if a line changes it, and
not variables:

```make
separate:
    x=5
    echo "Can the next line see x? It reads: [$x]"
```

```text
$ just separate
x=5
echo "Can the next line see x? It reads: [$x]"
sh: 1: x: parameter not set
error: recipe `separate` failed on line 12 with exit code 2
```

The second shell has never heard of
`x`, and because just runs its shells
with `-u` (treat unset variables as an
error), the line fails loudly instead
of printing an empty nothing. Read
that failure as a signpost, not a bug:
*this job wants to be one script.*

## Shebang recipes

Put a **shebang** line — `#!` followed
by an interpreter — as the body's first
line, and just writes the whole body to
a temporary file and runs it as one
script with that interpreter:

```make
countdown:
    #!/usr/bin/env bash
    for i in 3 2 1; do
      echo "Counting: $i"
    done
    echo "The loop ran inside one shell, start to finish."
```

Loops, conditionals, variables that
live across lines: everything a script
can do, a shebang recipe can do,
because it *is* a script, wearing a
recipe's name. The interpreter can be
anything on the machine — `bash`,
`python3`, a project tool — which makes
shebang recipes just's escape hatch
into real programming when a job
outgrows shell one-liners. Parameters
and variables still substitute into the
body before it is written, so the
script can be tailored by its caller.

Choose deliberately between the two
shapes. Lines are easier to watch
(just announces each one) and easier to
stop (a failure is pinned to a line
number). A script is one opaque act
from just's point of view: when it
fails, just can name the recipe but
not the line.

## Two small prefixes

- **`@` before a line** keeps just from
  announcing it. For lines whose output
  *is* the point — printing a report, a
  path, a version — the announcement is
  noise:

  ```make
  whisper:
      @echo "This line ran silently."
      echo "This line was announced first."
  ```

  A recipe *name* can start with `@` in
  the header (`@whisper:`) to quiet the
  whole body at once. Chapter 11's
  self-listing menu depends on this.
- **`-` before a line** lets it fail
  without stopping the recipe:

  ```make
  tolerant:
      -false
      echo "The failure above was tolerated."
  ```

  The failure still prints; the run
  still succeeds. Use it for steps that
  are genuinely optional — removing a
  file that may not exist — and
  nowhere else: a tolerated failure
  you did not mean is a bug wearing a
  disguise.

## Settings

Settings are whole-file adjustments,
written with `set`:

**`set shell := ["bash", "-cu"]`** —
the program and arguments used for
ordinary recipe lines, for when `sh`
is not the shell your lines were
written in. (On Windows the default is
PowerShell, and `set windows-shell`
and `set windows-powershell` exist for
the same purpose; that side is
documented-only here.)

**`set dotenv-load`** — before running
anything, just reads a `.env` file
sitting next to the justfile and
places its entries in the environment
of every line:

```text
$ cat .env
SNACK=crumpets
DRINK=cocoa

$ just menu
echo "From the .env file: $SNACK and $DRINK"
From the .env file: crumpets and cocoa
```

This is the tidy way to give a project
its local configuration — addresses,
names, choices that differ per machine
— without hard-writing them into
recipes or exporting variables by hand.
Treat the `.env` file itself with the
care its contents deserve: if it holds
secrets, it is not a file to commit.

## Asking first

One attribute deserves its own
mention. `[confirm]` makes just ask
before running a recipe:

```make
[confirm]
reset:
    echo "Everything reset."
```

```text
$ just reset   (and the answer is y)
Run recipe `reset`? echo "Everything reset."
Everything reset.
exit code: 0

$ just reset   (and the answer is n)
Run recipe `reset`? error: recipe `reset` was not confirmed
exit code: 1
```

(the prompt shows the recipe's first
line, so you confirm the act, not just
the name; the full exchange is
transcript `t14-confirm.txt`).
Destructive recipes (clean-ups,
resets, restores) earn their keep with
this one word in front of them: the
recipe stays one command long, and the
pause is built in rather than
remembered.
