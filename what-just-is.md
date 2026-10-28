# What just Is

Think of the last project you worked
on. Somewhere there is a command you
have typed fifty times: a test run with
three flags, a build with an output
folder, a deployment that is really
four commands in a trench coat. You
remember it *approximately*. Every run
starts with a scroll through shell
history, or a search through a notes
file, hoping past-you wrote it down.

**just is a command runner: a program
that keeps those commands in one small
file and runs them by name.** The file
is called a **justfile**. In it, each
saved command (or small set of commands)
is a **recipe**: a name, a colon, and
the lines to run, indented beneath it.

```make
# Build the project (a pretend one)
build:
    echo "Compiling the widget..."
    echo "Widget compiled."
```

Type `just build` anywhere inside the
project and those lines run, in order,
in your shell — as if you had typed
them yourself, because in effect you
did, once, carefully, and saved them.

## A runner, not a builder

just's famous neighbour, **make**, is a
*build tool*. Give make a target and it
works out what to do from the files on
disk: this source file is newer than
that object file, so recompile the one
and not the other. That cleverness is
make's whole purpose, and it is why
makefiles spend so much of their length
describing files.

just does none of that. It has no idea
which of your files changed, and it
does not care. A recipe runs when you
ask for it, runs everything it says,
every time. This sounds like a lack
until you notice how many project
commands were never about files at all:
*run the tests*, *start the server*,
*clean the scratch folders*, *deploy to
the test box*. For those, a tool that
tracks timestamps is solving a problem
you do not have, and charging you in
syntax for it. Chapter 12 gives make
its due; chapter 13 is honest about the
jobs where just's indifference to files
makes it the wrong tool.

The project's own description of just
is disarmingly small: *a handy way to
save and run project-specific commands*.
This guide takes that sentence
seriously. Everything in the next
thirteen chapters is a consequence of
it.

## The rule, stated before everything else

A justfile is not a description of your
commands. It *is* your commands, in a
form a program will execute without
asking. Type a bare `just` in a folder
and the first recipe in its justfile
runs. Type `just anything` and, if a
recipe by that name exists, it runs.
Nothing in this is dangerous when the
justfile is yours. It is worth one
moment's respect when it is not:

> **Read a justfile before you run it —
> above all one that arrived with a
> project you just downloaded.**

A recipe can delete files, reformat a
disk's contents, or send your work
somewhere, as easily as it can run a
test suite, and just will not warn you:
running commands is its job. This is
not a flaw in just; it is the nature of
every script and build file you have
ever run, stated where you can see it
instead of in fine print. The lab in
this guide wrote every justfile it ran.
When a project you trust ships one, read
it the way you would read an install
script — quickly, but actually.

## A first taste

After installing (chapter 3), a folder
holding the justfile above answers like
this — the lab's first runs are in
`just-lab/transcripts/t01-first.txt`:

```text
$ just
echo "Compiling the widget..."
Compiling the widget...
echo "Widget compiled."
Widget compiled.
```

Each line of the recipe appears twice:
once in just's voice, as it announces
the line it is about to run, and once
in the command's own output. That
announcement is a feature to hold on
to: with just, you always see the real
command, not just its result. Chapter
10 shows how to silence the
announcement when it stops earning its
keep.

Check: `just --list` shows the menu of
what a justfile offers, with the comment
above each recipe as its description:

```text
$ just --list
Available recipes:
    build # Build the project (a pretend one)
    test  # Run the checks
```

If that list is the only part of this
guide you ever use, it has already paid
for itself: a project's commands,
documented where they live, in the
project itself.

## Who it is for

The developer whose deploy is four
commands and a prayer. The maker whose
workshop machine builds firmware with a
command no human should have to
memorise. The student who keeps being
handed projects whose README says "run
the usual steps" — and who can now
answer: *write them in the justfile,
and the usual steps have names*. The
common thread is one person, a project
folder, and a set of commands that
deserve better than shell history.

## What it is not

just is not a build tool: it will not
spare you a recompile by noticing
nothing changed (chapter 13). It is not
a shell: the lines in a recipe are
ordinary shell commands, and learning
them is the Bash guide's job, not this
one's. It is not a scheduler or a
service manager: it runs things when
you ask, not at times or on events. And
it is not a package manager: it does
not fetch or install the tools your
recipes call. It keeps commands. That
turns out to be enough.
