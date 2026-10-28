# Your First Justfile

By the end of this chapter a folder on
your machine will hold a working
justfile: two recipes, both runnable,
and a list that explains itself. The
commands inside are deliberately dull —
echoes that print sentences — because
the subject is just, not the commands.
The lab's file is
`just-lab/first/justfile`, and its runs
are transcript `t01-first.txt`.

## The file

In any folder you like, create a file
named exactly `justfile` containing:

```make
# Build the project (a pretend one)
build:
    echo "Compiling the widget..."
    echo "Widget compiled."

# Run the checks
test:
    echo "All checks passed."
```

Anatomy, piece by piece:

- A line beginning with `#` is a
  **comment**. The comment directly
  above a recipe has a second job: it
  becomes the recipe's description in
  listings.
- `build:` at the start of a line opens
  a **recipe** named `build`. The colon
  is not decoration; it is what makes
  the line a recipe header.
- The lines beneath, **indented**, are
  the recipe's body: shell commands,
  run top to bottom. just accepts
  spaces or tabs for the indentation,
  but every line of one body must be
  indented, and indented past the
  header. An unindented line is read
  as something new — chapter 14 shows
  the error that produces.
- A blank line ends the recipe. The
  next header begins the next one.

## Running it

Three ways in, all from a terminal in
the folder (or anywhere under it —
chapter 8 explains the walking-up):

```bash
just          # runs the first recipe: build
just build    # the same, said out loud
just test     # a different recipe
```

And what comes back — the transcript,
cleaned of nothing:

```text
$ just
echo "Compiling the widget..."
Compiling the widget...
echo "Widget compiled."
Widget compiled.

$ just test
echo "All checks passed."
All checks passed.
```

Read the doubling correctly: the line
in just's voice (`echo "Compiling the
widget..."`) is just announcing the
command before running it; the plain
line after is the command's own output.
For these recipes the two look similar,
which stops being true the moment a
recipe does something quiet, like
copying a file: then the announced line
is your only narration, and it is
worth having.

Check: the bare `just` ran `build`,
because `build` is the **first recipe
in the file**. There is no setting to
find and no magic name: position is
the whole rule. (A recipe literally
named `default` is a common convention
for the job, but it works only because
its author also put it first.)
Chapter 11 uses that fact deliberately,
making the first recipe a menu.

## The list

```bash
just --list
```

```text
Available recipes:
    build # Build the project (a pretend one)
    test  # Run the checks
```

The descriptions came from the
comments. A recipe with no comment
above it is listed by name alone — just
does not invent descriptions, which is
one more reason to write the comment
while you still remember what the
recipe is for. Keep this command in
mind: in somebody else's project, it is
the safe way to read the menu *before*
the chapter 1 rule lets you order from
it.

## What you now know how to do

Create the file, name a recipe, indent
its commands, run it by name or by
position, and list what a justfile
offers. That is a complete, useful just
practice — many real justfiles never
grow past this chapter. The chapters
that follow add, in order: recipes
that take arguments, values shared
between recipes, recipes that call
recipes, and the organisation that
keeps a long file friendly.
