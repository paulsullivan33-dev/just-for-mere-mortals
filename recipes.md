# Recipes

Small jobs, whole. Every ingredient in
this chapter was run by the lab in the
chapter that taught it; this chapter
composes them, and the composed file —
`just-lab/cookbook/justfile` — was then
run end to end as well (transcript
`t15-cookbook.txt`). Adapt the names;
keep the shapes.

## The menu that shows itself

Make the first recipe list the others,
and a bare `just` becomes a safe thing
to type in a strange project:

```make
# Show this menu
default:
    @just --list
```

The `@` keeps just from announcing the
line, so the output is the clean menu
and nothing else. Anyone arriving at
the project — including you, in six
months — gets documentation as the
default behaviour.

## Build, test, check, clean

The classic quartet, wired with
dependencies:

```make
# Build the widget
build:
    mkdir -p build
    echo "fresh widget" > build/widget.txt
    echo "Built."

# Test the widget
test: build
    test -f build/widget.txt && echo "The widget is present."

# Build, then test
check: build test

# Remove what the build made (asks first)
[confirm]
clean:
    rm -rf build
    echo "Cleaned."
```

`just check` runs the build once —
`check` asks for it, `test` asks for
it, just remembers (chapter 7). The
`clean` recipe carries `[confirm]`
because deleting deserves a pause, and
the pause is part of the recipe, not
of the operator's memory.

## A dated backup with a name

```make
# Pack the notes folder into a dated backup
backup label="notes":
    mkdir -p backups
    tar czf backups/{{label}}-{{`date +%F`}}.tar.gz notes
    ls backups
```

```text
$ just backup
...
tar czf backups/notes-2026-10-28.tar.gz notes
ls backups
notes-2026-10-28.tar.gz
```

Three chapters meet in four lines: a
parameter with a default (chapter 5),
a value computed by a command at run
time (chapter 6), and output you can
check by eye. `just backup photos`
packs a different folder under a
different label, and yesterday's
archive is never overwritten, because
the date is in the name.

## The step that may fail

```make
# Tidy the scratch file, whether or not it exists
tidy:
    -rm scratch.tmp
    echo "Tidied."
```

The `-` prefix (chapter 10) lets the
removal fail — the file may simply not
be there — while the recipe carries
on and reports success honestly: the
failure prints, and so does the
tidying. Reserve the prefix for steps
whose failure you have already decided
not to care about.

## One recipe per environment

The shape for "same job, different
target", using an argument to choose
and a dependency to share the work:

```make
deploy env: (ship env)
    echo "Deploying to {{env}}."

ship env:
    echo "Shipping to {{env}}."
```

Real versions put the actual shipping
in `ship` and the target-specific
steps in `deploy`, and the caller
types the environment once:
`just deploy testing`. When the two
environments grow genuinely different
steps, split them into `deploy-test`
and `deploy-live` recipes that share
the preparation as a dependency —
prefer two readable recipes over one
recipe full of conditions.
