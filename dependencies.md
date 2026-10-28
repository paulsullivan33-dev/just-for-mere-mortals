# Dependencies

Real jobs have a shape: the build needs
the code fetched, the report needs the
build finished. **Dependencies** let a
recipe say so. A recipe can name other
recipes that must run *before* it, and
others that run *after* it, and just
weaves the whole set into one run —
each recipe executed at most once,
however many times it was asked for.
The lab's file is `just-lab/deps/justfile`;
its runs are transcript `t04-deps.txt`.

## Dependencies before

List them after the colon, after any
parameters:

```make
build: prepare
    echo "Building on the prepared groundwork."
```

`just build` runs `prepare` first,
then `build`. The dependency is a full
recipe: its own lines, its own further
dependencies, all resolved in order.
Ask for two recipes that share a
dependency and it still runs once:

```text
$ just build build   (the same recipe twice)
echo "Preparing ingredients."
Preparing ingredients.
echo "Building on the prepared groundwork."
Building on the prepared groundwork.
```

Nothing in that transcript is repeated.
Within one invocation, just keeps a
list of what has already run and does
not run it again — the property that
makes dependencies safe to sprinkle
freely: `test` can depend on `build`,
`deploy` can depend on `test`, and
`just deploy` gives you each step once,
in the only order that makes sense.

## Dependencies after: `&&`

Dependencies written after `&&` run
*after* the recipe's own body — the
shape for tidying, reporting, and
notification steps that belong to a job
but are not its substance:

```make
# The full works: prepare, then build, then report done
all: build && report
    echo "Middle step."
```

The lab ran it, and the order in the
transcript is the whole lesson:

```text
$ just all
... prepare, then build (all's dependencies) ...
echo "Middle step."
Middle step.
echo "Reporting: all done."
Reporting: all done.
```

First the before-dependencies (prepare,
then build), then `all`'s own line,
then the after-dependency. If the body
fails, the after-dependencies do not
run: reporting success would be a lie,
and just does not let the recipe tell
it.

## A dependency with arguments

A dependency in parentheses receives
arguments, like a call:

```make
deploy env: (ship env)
    echo "Deploying to {{env}}."

ship env:
    echo "Shipping to {{env}}."
```

```text
$ just deploy testing
echo "Shipping to testing."
Shipping to testing.
echo "Deploying to testing."
Deploying to testing.
```

The depending recipe's own parameter is
passed along, so the value is typed
once by the caller and flows down.
A dependency written *without*
parentheses takes no arguments, and
must need none — give it parameters
with defaults, or call it in
parentheses with values.

## When dependencies are the wrong tool

Dependencies wire recipes together for
a *run*; they are not a way to share
code between recipes. Two recipes with
three identical lines should probably
be one recipe with a parameter
(chapter 5), or a shebang script both
can call (chapter 10). And a dependency
says *order*, not *need*: `report`
depending on `build` runs a build even
when the report was all you wanted and
the build is an hour long. just has no
way to know the build is fresh — it
does not look at files at all
(chapter 13). Wire the order you mean,
and keep the wiring shallow enough to
read at a glance in `--list` order.

## Running side by side

Independent dependencies normally run
one after another. Marking a recipe
`[parallel]` lets just overlap its
dependencies, bounded by `--jobs <N>`:

```make
[parallel]
gather: chop stir
    echo "Gathering the results."
```

The lab's run shows the two
dependencies' output interleaving
(transcript `t12-misc.txt`). Reach for
this when the steps genuinely do not
touch each other's work — two downloads,
two independent checks — and keep it
away from anything where order is the
point.
