# The Moving Parts

just is a small tool, and its parts can
be counted on one hand. By the end of
this chapter you will be able to say
which part is responsible for anything
just does — which turns every later
surprise into a named behaviour instead
of a mystery.

## The program

`just` itself is a single program, one
file, no plugins and no daemon. It does
four things: find a justfile, read it,
work out what you asked for, and run
the recipes. When nothing is running,
just is not there: no background
process, no state kept between runs
except the justfile you wrote. Chapter
3 installs it.

## The justfile

The **justfile** is a plain text file,
usually sitting at the top of a project
folder, named `justfile` (just also
accepts `.justfile`, and on any system
a file named exactly `justfile` is the
one to write). It holds everything:
recipes, variables, settings. There is
no other configuration file to hunt
for, and chapter 8 shows how just finds
this one by walking up from wherever
you are standing.

A justfile is read whole, every run.
One syntax error anywhere in it — a
line that forgot its indentation — and
just stops before running anything,
pointing at the offending line
(chapter 14 shows the message). This
sounds strict until you have been saved
by it: a half-broken command file that
ran *mostly* would be worse.

## Recipes

A **recipe** is the unit of work:

```make
# Run the checks
test:
    echo "All checks passed."
```

The name sits at the start of a line,
followed by a colon. The body is the
indented lines beneath. The comment
directly above doubles as the recipe's
description in `just --list`. Recipes
can take **arguments** (chapter 5),
run other recipes first
(**dependencies**, chapter 7), and wear
**attributes** — bracketed notes such
as `[private]` that change how just
treats them (chapter 9).

Recipe names are also how you talk to
just: `just test` means "the recipe
called test", and words after it belong
to that recipe as arguments, or name
further recipes to run in the same
invocation.

## The shell under each line

just does not run recipe lines itself.
Each line is handed to a **shell** —
by default `sh` on Linux and macOS
systems, PowerShell on Windows (that
half is documented-only in this guide;
the lab is a Linux machine). Two
consequences shape the whole tool, and
chapter 10 is built on them:

- **Each line gets a fresh shell.** A
  variable set on one line does not
  exist on the next. Lines are
  independent errands, not one
  continuous script — unless you ask
  for a script, which chapter 10 does
  with a **shebang recipe**.
- **just watches the exit code.** A
  line that fails stops the recipe,
  and just reports the recipe, the line
  number, and the code it died with.

just prints each line before running
it (to standard error, so it does not
mix into a recipe's real output), which
is why the transcripts in this guide
show every command twice.

## Variables and expressions

A **variable** is a value given a name
at the top of the justfile:

```make
version := "1.2.0"
```

Variables are just's own, not the
shell's: they live in the justfile's
world and are substituted into recipe
lines where you write `{{version}}`.
They can be built from text, from other
variables, from the output of a command
(in backticks), and from the
environment you launched just in.
Chapter 6 is theirs. The bridge
between the two worlds is the word
`export`: an exported variable is also
placed in the environment of the shells
that run your lines.

## Settings

A justfile can adjust just itself with
**settings**, lines beginning `set`:

```make
set shell := ["bash", "-cu"]
set dotenv-load
```

Settings choose the shell, load a
`.env` file before running, allow a
justfile to work on another operating
system, and a handful of other
whole-file behaviours. They take effect
for the whole file, wherever in it they
are written. Chapter 10 uses the two
above and chapter 13 lists the limits
settings cannot lift.

## Modules and imports

A big justfile can be split. A line
like `mod kitchen` pulls in a second
file (here, `kitchen/mod.just`) whose
recipes become available under a
prefix: `just kitchen bake`. **Imports**
(`import 'other.just'`) merge another
file's recipes into the same namespace
instead. The lab ran a module for real
(transcript `t11-extras.txt`); imports
are described from the documentation.
Most justfiles never need either —
reach for them when `--list` no longer
fits on one screen.

## Which part holds what

| Question | The part that answers |
|---|---|
| What commands exist? | The justfile's recipes |
| What does `just x` do? | Recipe `x`, its dependencies, then its lines |
| Why did it stop halfway? | A line failed; just reports its exit code (chapter 14) |
| Where did this value come from? | A variable, an argument, the environment, or a default (chapters 5–6) |
| Why did it run *there*? | The justfile's folder (chapter 8) |
| Why is this recipe hidden? | An attribute (chapter 9) |

With the parts named, the rest of the
guide is assembly: install the program,
write the file, and teach it better
tricks one chapter at a time.
