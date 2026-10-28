# Troubleshooting

The failures this guide's lab met on
purpose, and the ones it met by
accident, each with its cause and its
fix. just's error messages are good;
this chapter is mostly about learning
to believe them.

## "error: no justfile found"

You ran `just` somewhere with no
justfile in that folder or any parent.
Either you are not inside the project
you think you are (check with `pwd`),
or the project does not have one yet
(chapter 4 writes it). There is no
global or personal justfile for just
to fall back to — deliberate, and
chapter 13 explains why that is a
virtue.

## "justfile does not contain recipe `Lovelace`"

You almost certainly did not mistype
a recipe name. You gave a recipe more
words than it has parameters, and just
read the extra word as the name of
another recipe to run (chapter 5).
Fixes: quote values that contain
spaces so the shell passes them as one
word — `just greet "Ada Lovelace"` —
or give the recipe a variadic
parameter (`*rest`) if leftovers are
legitimate.

## "recipe `snack` got 0 positional arguments"

A required parameter was left empty.
The error prints the recipe's usage,
with defaulted parameters shown in
brackets — `just snack food [drink]`
— so the fix is written on the
failure: supply the missing value, or
give the parameter a default if it
has a sensible one (chapter 5).

## The recipe stopped halfway, with an exit code

```text
error: recipe `steps` failed on line 4 with exit code 1
```

A line in the recipe failed, just
stopped, and the message names the
recipe, the line (counted in the
justfile), and the code the command
died with. Nothing after that line
ran, and no dependency or
after-dependency tidied up. To see the
failure yourself, run the very line
just announced — it prints every line
before running it, so the failing
command is the last thing above the
error. If the line is *allowed* to
fail, prefix it with `-` (chapter 10),
and mean it.

## "x: parameter not set" (or a variable that reads empty)

Two recipes in one: a value set on one
line was read on another. Each line is
a fresh shell (chapter 10), so shell
variables do not carry. Either keep
the whole job on one line (joined
with `&&` or `;`), move the value into
a just variable (chapter 6) and
substitute it with `{{ }}`, or make
the recipe a shebang script, where
lines share a shell because there is
only one.

## "error: extraneous attribute"

An attribute is in the wrong place.
Attributes go *between the comment
and the recipe*, in that order:

```make
# Build the project
[group('making')]
build:
```

Comment below the attribute, or
attribute floating above a comment,
and the whole file is refused. The
error points at the attribute's line;
reorder the three lines and it passes.
(The lab met this one unplanned, while
writing chapter 9's examples, which is
why it is documented with feeling.)

## A syntax error about a line that looks fine

```text
error: expected '*', ':', '$', identifier, or '+', but found string
 ——▶ justfile:3:6
  │
3 │ echo "This line forgot its indentation."
```

The line has lost its indentation, so
just reads it as a new construct at
the top level and trips over it. One
unindented body line refuses the
*whole file* — no recipe in it will
run — because just parses everything
before running anything. Transcript
`t13-indent-error.txt` kept the full
message. Run `just --fmt` to see the
file in canonical shape; the line that
juts out is the culprit.

## It ran the wrong folder's work

You invoked just in one folder and the
recipe acted on another: recipes run
in the justfile's folder (chapter 8),
not where you stood. If a recipe must
act where you stand, it needs
`{{invocation_directory()}}` in its
paths or the `[no-cd]` attribute; if
*you* must, `--working-directory`
overrides for one run.

## It works in the terminal but not from cron or CI

The usual cause is not just. A
scheduled environment has a smaller
PATH and a different home than your
interactive shell, so a line that
finds its tools at your desk may not
find them at 3 a.m. just reports the
line's failure faithfully. Set the
PATH inside the recipe (or in a
variable at the top of the justfile),
and test the recipe the way the
scheduler will run it: `env -i`, with
only the PATH the scheduler would
have, shows you the recipe with
nobody's environment to lean on (the
lab ran its cookbook recipes exactly
so, and they behaved).
