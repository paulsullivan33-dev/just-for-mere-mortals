# Arguments and Parameters

A recipe that can only ever do one
exact thing is a recording. A recipe
that takes **arguments** is a tool: the
same deploy, aimed at the test box or
the real one; the same greeting, with
a name in it. This chapter gives
recipes parameters, defaults for the
common case, and a way to accept any
number of extras. The lab's file is
`just-lab/params/justfile`; its runs
are transcript `t02-params.txt`.

## A parameter

Write parameters after the recipe
name, in the header:

```make
# Greet someone by name
greet name:
    echo "Hello, {{name}}!"
```

Inside the body, `{{name}}` is replaced
by whatever the caller supplied:

```text
$ just greet Ada
echo "Hello, Ada!"
Hello, Ada!
```

Parameters are positional: the first
word after the recipe name fills the
first parameter, the second the
second. A recipe's parameters are its
own — other recipes do not see them —
and each is also available to the
recipe's dependencies in the one case
chapter 7 covers.

## A default for the common case

`name="world"` gives a parameter a
**default**, used when the caller
supplies nothing:

```make
# Greet someone by name
greet name="world":
    echo "Hello, {{name}}!"

# Serve a snack, with a choice of drink
snack food drink="tea":
    echo "Serving {{food}} with {{drink}}."
```

```text
$ just greet
echo "Hello, world!"
Hello, world!

$ just snack toast
echo "Serving toast with tea."
Serving toast with tea.

$ just snack toast coffee
echo "Serving toast with coffee."
Serving toast with coffee.
```

Required parameters come first;
parameters with defaults follow them.
That ordering is forced on you for a
good reason: the caller fills slots
left to right, so a slot that may be
left empty has to be one the caller can
reach the end without filling.

Forget a required argument and just
says so, with the recipe's shape spelled
out — no partial run, nothing executed:

```text
$ just snack
error: recipe `snack` got 0 positional arguments but takes at least 1
usage:
    just snack food [drink]
exit code: 1
```

## Any number of extras

A parameter whose name begins with `*`
is **variadic**: it collects everything
left over into one value, its pieces
joined by spaces. With `+` instead, it
collects one-or-more — at least one is
required.

```make
# Add up any number of items
total first *rest:
    echo "First item: {{first}}; the rest: {{rest}}"
```

```text
$ just total apple banana cherry
echo "First item: apple; the rest: banana cherry"
First item: apple; the rest: banana cherry
```

Variadic parameters are how recipes
wrap tools that take file lists:
`just lint src/a.py src/b.py` forwards
the whole bunch to one command, and
the recipe stays one line long.

## The words after the recipe are not all arguments

One behaviour surprises everyone once.
Call a one-argument recipe with two
words:

```text
$ just greet Ada Lovelace
error: justfile does not contain recipe `Lovelace`
```

just did not overflow the argument. It
ran `greet` with `Ada`, then read the
next word as **another recipe to run**
— and there is no recipe called
`Lovelace`. Multiple recipes in one
invocation is a real feature
(`just build test` runs both), and
this error is its shadow: too many
words looks exactly like another
recipe's name. When a name contains a
space, quote it — `just greet "Ada
Lovelace"` — and the shell hands it to
just as one word, as it should.

## Flags of your own

Parameters can also arrive as
named flags (`just deploy --env=test`)
rather than by position, with `set
positional-arguments` as the opposite
arrangement, passing all parameters to
the recipe as the shell's `$1`, `$2`,
and so on. Both are documented in
just's manual and described here from
it, not run by the lab — positional
arguments and defaults carry the vast
majority of real justfiles, and they
are the shapes the rest of this guide
uses.
