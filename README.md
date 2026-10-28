# just for Mere Mortals

Every project grows a pile of commands.
The one that runs the tests, with the
flags you always forget. The one that
builds the thing. The three-step dance
that deploys it. They live in your shell
history, in a notes file, in your head —
and they evaporate the moment you switch
machines or come back after a month away.

**just** is a **command runner**. You
write those commands once, in a small
text file called a **justfile**, give
each one a name, and from then on you
run them by name:

```bash
just build
just test
just deploy testing
```

just is not a build tool in the old
sense: it does not compare file dates
and work out what needs rebuilding. It
simply runs the commands you saved, in
the order you saved them, and gets out
of the way. That smallness is the point,
and this guide explains it properly:
what a **recipe** is, how recipes take
**arguments**, share **variables**, and
depend on each other, why you can run
them from any folder in the project, and
— stated plainly — where just's
usefulness ends and a real build tool's
begins.

One rule belongs up front, and chapter 1
states it in full: **a justfile is a
program.** Running `just` in a folder
executes whatever its recipes say,
including the very first one when you
type a bare `just`. So read a justfile
before you run it, the way you would
read any script a stranger hands you.
Your own justfiles, written by you, for
your own projects: that is what this
guide is about.

The lab behind this guide ran the real
thing: **just 1.59.0**, the official
release binary from the project's
GitHub page, on a small Linux machine.
Against a string of tiny practice
projects it wrote a first justfile, ran
recipes with arguments and default
values, shared variables between
recipes, read values from the
environment, chained recipes together
with dependencies, ran everything from
two folders deep and watched just find
the justfile on its own, organised a
long list of recipes into groups, hid
the private ones, wrote recipes that
are whole small scripts, fed just a
`.env` file, and made the mistakes on
purpose — unknown recipes, missing
arguments, a line that failed halfway
through — so the error messages in this
guide are the real ones.

What the lab could not do, it did not
fake. It had one Linux machine, so
just's behaviour on Windows and macOS —
where the shell under each recipe line
is a different program — is taught from
the project's documentation and
labelled as such wherever it appears.
The installs in chapter 3 are verified
only for the route the lab took (the
release archive); the package-manager
routes are described, not demonstrated.
`criteria.md` keeps the full ledger of
what was run and what was only read.

## What this guide covers

- [**What just is**](what-just-is.md) —
  the mental model: saved commands with
  names, the difference between running
  commands and building software, and
  the read-before-you-run rule
- [**The moving parts**](the-moving-parts.md) —
  the just program, the justfile,
  recipes, the shell that runs each
  line, variables, dependencies,
  settings and modules, and which part
  holds what
- [**Installing just**](installing-just.md) —
  the release archive the lab used, the
  package-manager routes, and checking
  the install with a version number and
  shell completions
- [**Your first justfile**](your-first-justfile.md) —
  a two-recipe file, running it, and
  reading what just prints as it works
- [**Arguments and parameters**](arguments-and-parameters.md) —
  recipes that take values, defaults
  for the common case, and recipes that
  accept any number of extras
- [**Variables**](variables.md) —
  values set once and used everywhere,
  values read from the environment or
  from commands, and overriding a
  variable for a single run
- [**Dependencies**](dependencies.md) —
  recipes that run other recipes first,
  recipes that run afterwards, and why
  nothing runs twice in one invocation
- [**Running from anywhere**](running-from-anywhere.md) —
  just finds the justfile by walking
  upwards, recipes run in the justfile's
  folder, and the options that change
  both behaviours
- [**Listing and organising**](listing-and-organising.md) —
  `--list` and the comments that feed
  it, groups, aliases, private recipes,
  and the formatter that tidies a
  justfile
- [**Recipes as scripts**](recipes-as-scripts.md) —
  one line per shell versus a whole
  script with a shebang, quiet lines,
  lines allowed to fail, settings, and
  `.env` files
- [**Recipes**](recipes.md) — a menu
  that lists itself, a build-test-clean
  trio, a backup with a name and a
  date, and other small jobs, whole
- [**The neighbours**](the-neighbours.md) —
  make, Task, npm scripts, and a folder
  of shell scripts: what each does that
  just doesn't, and vice versa
- [**Limits**](limits.md) — no
  file-freshness graph, one shell per
  line, the justfile as code you must
  trust, and the jobs just is the wrong
  shape for
- [**Troubleshooting**](troubleshooting.md) —
  no justfile found, the recipe that
  "does not exist" because it was an
  argument, the attribute in the wrong
  place, and the line that failed with
  an exit code
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

just as a build system for large
compiled projects, where working out
what changed is the whole job — chapter
13 explains why that belongs to make
and its kin. Writing shell commands
themselves: this guide assumes you have
commands worth saving, and the series'
Bash guide is the place to learn them.
Running other people's justfiles unread
is not a harder chapter of this book;
it is how machines get hurt, and
chapter 1 explains the line.

## How to use this guide

Read chapter 1 for the model and the
rule, and chapter 2 before installing
anything — knowing which part holds
what turns later surprises into named
behaviour. Chapter 4 writes a justfile
in the same sitting as the install, and
its recipes are deliberately boring:
the point is to watch just work, not to
admire the commands. Chapters 5 to 7
are the heart of the thing — arguments,
variables, dependencies — and each one
pays off in every justfile you write
afterwards. Chapter 11 is the one that
turns a list of commands into a tool
other people can use without reading
your mind. The lab's practice justfiles
live in `just-lab/`, named after the
chapter that uses them, and its
transcripts in `just-lab/transcripts/`;
its notes are `examples/lab-notes.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Make for Mere Mortals](https://paulsullivan33-dev.github.io/make-for-mere-mortals/)
- [Bash for Mere Mortals](https://paulsullivan33-dev.github.io/bash-for-mere-mortals/)
- [Git for Mere Mortals](https://paulsullivan33-dev.github.io/git-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/just-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/just-for-mere-mortals/blob/main/LICENSE-CODE).
