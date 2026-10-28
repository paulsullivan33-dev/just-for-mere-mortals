# Listing and Organising

A justfile grows. What begins as three
recipes becomes thirty, and the file
nobody can survey is a file nobody
trusts. just's answer is to treat
`--list` as a first-class feature and
give you groups, aliases, and private
recipes to shape it with — plus a
formatter that keeps the file itself
tidy. The lab's file is
`just-lab/listing/justfile`; the runs
are transcript `t06-listing.txt`.

## The list is the documentation

You have met `--list`:

```text
$ just --list
Available recipes:
    plain # A recipe with no comment of its own

    [housekeeping]
    clean # Tidy up

    [making]
    build # Build the project [alias: b]
    test  # Test the project
```

Everything in that output is something
you wrote, in this order of importance:

- The **comment directly above a
  recipe** is its description. A
  recipe with no comment is listed by
  name alone — just does not guess.
- **Groups** gather related recipes
  under a heading (below).
- **Aliases** are shown against the
  recipe they shorten.

Ungrouped recipes list first, then the
groups, each set in alphabetical order
unless you ask otherwise with
`--unsorted`.

## Groups

The `[group('name')]` attribute files a
recipe under a heading:

```make
# Build the project
[group('making')]
build:
    echo "Building."
```

One rule bites, and the lab bit on it
so you don't have to: **the comment
goes above the attribute, not below
it.** Comment, then attribute, then
recipe — any other order and just
refuses the file with an "extraneous
attribute" error (chapter 14 shows it).
`just --list --group making` prints
just that group's recipes, which is how
a thirty-recipe file stays answerable:
the menu, one course at a time.

## Aliases

An **alias** gives a recipe a second,
shorter name:

```make
alias b := build
```

`just b` and `just build` are now the
same act, and the list shows the
shortcut against its recipe. Aliases
earn their place on the two or three
recipes you run hourly — the ones your
fingers know. Alias everything and you
have built a second, worse naming
scheme; the list will show that too.

## Private recipes

Some recipes exist to be *used by other
recipes* — a preparation step, a shared
check — and only clutter the menu. The
`[private]` attribute removes a recipe
from `--list` and from `--summary`,
while leaving it fully runnable:

```text
$ just secret-ingredients   (private means unlisted, not unusable)
echo "A pinch of patience."
A pinch of patience.
```

Private means *unlisted*, not
*protected*: anyone can still run it by
name, and chapter 1's rule is the only
real gate a justfile has. (A recipe
whose name starts with `_` is private
for free, by convention turned rule.)
Use privacy to keep the menu honest
about what a newcomer should reach for
first.

## The summary, and seeing one recipe

- `just --summary` prints every public
  recipe name on one line — built for
  scripts and shell completions, and
  for the "what was in that file
  again?" moment.
- `just --show build` prints one
  recipe's full text, parameters and
  all, as just understands it — the
  quickest way to check what a
  recipe will actually do before
  chapter 1 lets you run it.

## The formatter

`just --fmt` rewrites a justfile in
just's own canonical style: even
indentation, aligned assignments, one
spacing convention throughout.
`just --fmt --check` changes nothing
and reports the difference instead —
the lab ran it against a deliberately
scruffy file, and it answered with a
diff and a nonzero exit code, which is
exactly what a pre-commit check (or a
CI job) needs to keep a shared justfile
from drifting into dialects. And
`just --dump` prints the justfile as
just parsed it — the tool's-eye view,
useful when a file behaves as if it
said something other than what you
read.
