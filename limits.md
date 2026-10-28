# Limits

What just cannot do, and what it must
not be asked to do — the chapter the
rest of the guide has been promising.

## It does not know what changed

just has no model of your files. It
cannot skip a step because its inputs
are older than its outputs, cannot
build "just the affected part", and
cannot notice that a dependency's
result is already sitting on disk from
a previous run. Every recipe runs in
full, every time. For commands, that
is the correct simplicity. For builds
of any size, it is a wall: the
tenth run of a big compile takes what
the first one took. If your project's
pain is *rebuild time*, the tool you
want is make or one of its modern
kin (chapter 12), and no justfile
cleverness changes that — at best, a
recipe can call the build tool and let
*it* be clever.

## One shell per line cuts both ways

The fresh-shell-per-line rule
(chapter 10) means a justfile cannot
hold state between lines except by
writing it somewhere: a file, an
environment variable set by the
caller, a value recomputed by each
recipe that needs it. Multi-step
logic — loops with memory, branching
on a result — belongs in a shebang
recipe or a real script the recipe
calls. A justfile that fights this,
full of values passed through
temporary files, is telling you the
job outgrew the tool's shape.

Relatedly, just's world is the
terminal it was launched from. It
prompts only where `[confirm]` asks,
reads no interactive input mid-recipe
beyond that, and is not a way to build
interactive programs: menus and
wizards belong in the script, not in
the runner.

## The justfile is trusted code

Chapter 1's rule deserves its second
statement here, as a limit of the
tool, not just of manners: **just
cannot tell a helpful justfile from a
hostile one, and provides no sandbox,
no dry permission system, and no
warning before recipes run.** `--list`,
`--show`, and `--dry-run` (which prints
the lines a run would execute, without
executing them) are reading tools, and
reading is the defence.
Downloaded projects deserve the same
thirty seconds you would give an
install script. Nothing in this guide
wants that to change — a command
runner that negotiated with you about
your own commands would be a worse
tool — but the limit should be known,
not discovered.

## One machine, one moment

just runs recipes here, now, as you.
It does not schedule (that is cron's
and systemd's country), does not run
things on other machines except by a
recipe calling `ssh` or a deploy tool
like any shell line would, keeps no
history or logs beyond what your
recipes write, and has no notion of
users, roles, or approval. Teams do
share justfiles — through the
project's own version control, where
the file gets the same review as any
other code, which is as it should be.

Parallelism, where chapter 7 offered
it, is a modest tool: `[parallel]`
overlaps a recipe's dependencies, and
`--jobs` bounds how many. It is not a
job scheduler, results interleave in
one terminal, and steps that write the
same files will happily race. Overlap
the downloads and the independent
checks; keep everything with an order
in its name sequential.

## Small print, honestly

- just's own language is not a
  programming language: expressions
  join strings, compare them, and call
  a modest set of built-in functions.
  When the logic gets interesting,
  that is the shebang recipe's cue.
- The file format still grows. New
  releases add features (modules and
  groups are recent arrivals), so a
  justfile from the future may refuse
  to run on an old just, with a clear
  error — the fix is a newer binary,
  and chapter 3's archive makes that
  a two-minute job.
- Windows is a second dialect, not a
  port: the default shell is
  PowerShell, path spellings differ,
  and a justfile that leans on Unix
  one-liners needs its `set
  windows-shell` or its recipes
  adjusted. This guide's lab could not
  run that half; the manual treats it
  as a first-class citizen, and a
  Windows reader should hold the
  manual closer than these chapters.
