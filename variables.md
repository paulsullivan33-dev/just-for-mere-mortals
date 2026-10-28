# Variables

Parameters belong to one recipe. A
**variable** belongs to the justfile:
a value named once, near the top, and
used by every recipe that needs it —
the version number, the server's
address, the folder everything lands
in. Change it in one place and every
recipe follows. This chapter sets
variables, builds them from commands
and from the environment, shares them
with the shell when that is the right
direction, and overrides one for a
single run. The lab's files are
`just-lab/variables/justfile` and
`just-lab/inspect/justfile`; transcripts
`t03-variables.txt` and `t10-inspect.txt`.

## Setting and using

An assignment uses `:=`:

```make
project := "widget"
version := "1.2.0"
```

Use in a recipe is the same `{{ }}`
substitution arguments use:

```make
    echo "{{project}} version {{version}}"
```

prints `widget version 1.2.0`. Strings
join with `+`, and an `if` expression
chooses between values:

```make
greeting := "Hello" + " " + "there"
size := if greeting == "Hello there" { "familiar" } else { "strange" }
```

Both ran in the lab (transcript
`t11-extras.txt`). One warning about
timing: just evaluates a variable when
something needs it, not when the file
is read — so an unused variable with a
surprising definition costs nothing,
and a well-used one is worked out once
per run, not once per recipe.

## Values from commands

Backticks run a command and take its
output as the value:

```make
today := `date +%F`
```

gives `today` the date in `2026-10-28`
shape, fresh on every run. This is the
way to stamp builds, name backups after
the day, or ask git for the current
branch — computed by the machine, at
the moment it matters, instead of typed
by a human who might be wrong.

## Values from the environment

Two functions read the world just was
launched into:

- `env_var('HOME')` — the value of an
  environment variable. If it is not
  set, just stops with an error:
  a missing value you depended on is
  worth stopping for.
- `env_var_or_default('WIDGET_COLOUR', 'purple')`
  — the same, but with a fallback used
  quietly when the variable is absent.

The lab's `show` recipe used both,
and the transcript shows the fallback
being taken:

```text
echo "A variable with a fallback: purple"
A variable with a fallback: purple
```

## Sharing with the shell: export

A just variable lives in just's world.
The shell running your lines cannot see
it — `echo $version` prints nothing
(or errors, chapter 10). The bridge is
`export`:

```make
export GREETING := "Hello from the justfile"
```

An exported variable is *also* placed
in the environment of every shell just
starts, so the line can use the shell's
own spelling:

```text
echo "$GREETING"
Hello from the justfile
```

Choose the direction deliberately.
`{{version}}` is substituted by just,
before the shell sees the line, so it
works inside single quotes and cannot
be disturbed by the shell.
`$GREETING` is the shell's business,
subject to the shell's quoting and
splitting rules. When a value might
contain spaces or characters the shell
finds interesting, just's substitution
is the safer road.

## Overriding for one run

Any variable can be set from the
command line, as `name=value` before
the recipe name, and the command line
wins over the file:

```text
$ just colour=gold paint
echo "Painting the widget gold."
Painting the widget gold.
```

The file is not changed; the override
lasts for that invocation. This is the
polite way to make a justfile
configurable without editing it —
release switches, target machines,
verbosity — and chapter 11's recipes
lean on it.

Check what just thinks the values are
with `just --evaluate`, which prints
every variable as just resolved it,
after overrides:

```text
$ just --evaluate
colour := "purple"
item   := "widget"
```
