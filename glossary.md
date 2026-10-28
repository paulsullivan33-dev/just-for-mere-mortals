# Glossary

**Alias** — a second, shorter name for
a recipe, declared with `alias b :=
build`; shown in `--list` against its
recipe (chapter 9).

**Argument** — a value a caller passes
to a recipe, as in the `Ada` of
`just greet Ada`; it fills a parameter
(chapter 5).

**Attribute** — a bracketed note before
a recipe — `[private]`, `[group('x')]`,
`[no-cd]`, `[confirm]`, `[parallel]` —
that changes how just treats it. It
belongs between the comment and the
recipe header (chapters 9–10).

**Default (parameter)** — the value a
parameter takes when the caller
supplies none, written `name="world"`
(chapter 5).

**Default recipe** — whichever recipe
is first in the file; a bare `just`
runs it. Often a menu, by the
convention of chapter 11.

**Dependency** — a recipe named after
another recipe's colon, run before it;
after `&&`, run after it. Each recipe
runs at most once per invocation
(chapter 7).

**Dry run** — `just --dry-run`: prints
the lines that would run, runs none.

**Export** — an assignment marked
`export`, which also places the
variable in the environment of the
shells that run recipe lines
(chapter 6).

**Group** — a named heading under
which `--list` gathers related
recipes, set with `[group('name')]`
(chapter 9).

**Interpolation** — the `{{ }}`
substitution that puts a variable's or
parameter's value into a recipe line
before the shell sees it.

**Invocation directory** — the folder
you were standing in when you typed
`just`, available to recipes as
`invocation_directory()` (chapter 8).

**justfile** — the plain-text file,
usually at a project's root, holding
recipes, variables and settings. just
finds it by walking up from the
current folder.

**Module** — a second justfile pulled
in by a `mod` line, whose recipes are
run under a prefix, as in
`just kitchen bake` (chapter 2).

**Parameter** — a named slot in a
recipe header that callers fill with
arguments (chapter 5).

**Private recipe** — a recipe hidden
from listings by the `[private]`
attribute or a leading `_` in its
name. Unlisted, not unusable
(chapter 9).

**Recipe** — the unit of a justfile: a
name, a colon, optional parameters and
dependencies, and an indented body of
shell lines.

**Setting** — a whole-file adjustment
written with `set`, such as
`set shell := [...]` or
`set dotenv-load` (chapter 10).

**Shebang recipe** — a recipe whose
body begins with a `#!` line, run as
one script by that interpreter instead
of line-by-line shells (chapter 10).

**Variadic parameter** — a parameter
beginning with `*` (any number,
including none) or `+` (at least one)
that collects all remaining arguments
(chapter 5).

**Working directory** — the folder a
recipe's lines run in: the justfile's
folder, unless `--working-directory`
or `[no-cd]` says otherwise
(chapter 8).
