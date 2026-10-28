# Running from Anywhere

A project is not one folder. It is a
tree, and you are usually standing
somewhere inside it — in the docs
folder, in one module's corner — when
you want the project's commands. cd-ing
to the top first, every time, is a
small tax just refuses to charge. This
chapter shows the walk upwards, the
folder recipes actually run in, and
the two options that override both.
The lab's file is
`just-lab/subdirs/justfile`; the runs
are transcript `t05-subdirs.txt`.

## The walk upwards

When you type `just`, it looks for a
justfile in the folder you are in. Not
there? It looks in the parent folder.
Then the parent's parent, all the way
to the top of the filesystem. The first
justfile found is the one that runs.

So a justfile at the project's root
serves the whole tree:

```text
$ cd deep/deeper
$ just where    (from two folders down)
```

found the lab's justfile two levels up
and ran its recipe, with no path typed
and no complaint. When no justfile
exists anywhere up the tree, you get
the plain error from chapter 14:
`error: no justfile found`.

## Recipes run in the justfile's folder

Finding the file is only half of it.
Recipes also **run** as if you were
standing in the justfile's folder,
whatever folder you actually invoked
just from. The lab proved it with a
recipe whose whole job is to say where
it is:

```make
where:
    pwd
    echo "The justfile lives in: {{justfile_directory()}}"
    echo "You invoked just from: {{invocation_directory()}}"
```

From two folders down, `pwd` still
prints the justfile's folder. This is
the behaviour that makes project
commands robust: a recipe that says
`cp build/widget /tmp/` means the same
thing from every corner of the project,
because relative paths in recipes are
always relative to the same place —
where the justfile lives.

Two functions name the two folders,
and the transcript shows them parting
company:

- `justfile_directory()` — where the
  justfile is; where recipes run.
- `invocation_directory()` — where you
  were standing when you typed `just`.

A recipe that must touch the file you
were *looking at* — format this file,
preview this document — uses
`invocation_directory()` to reach back
for it. Most recipes never need either;
they matter the day a relative path
surprises you, and now the surprise
has names.

## Overriding both choices

Two options, used together, hand the
choices back to you:

```bash
just --justfile ../../justfile --working-directory . where
```

`--justfile` names the file explicitly,
skipping the walk. `--working-directory`
names the folder recipes run in,
instead of the justfile's folder — here
`.`, the folder you are standing in,
and the transcript shows `pwd` agreeing.
This pairing is how scripts and other
tools drive a justfile from outside the
project, and how you rescue a recipe
whose notion of "here" is wrong for
one run.

A single recipe can also opt out of
the move with the `[no-cd]` attribute:
its lines run in the invocation folder
while the rest of the file keeps the
usual rule. The lab's `[no-cd]` recipe,
driven from `/` with an explicit
`--justfile`, printed `/` from its
`pwd` (transcript `t11-extras.txt`).
Recipes that wrap a tool which must see
the caller's folder — a formatter, a
file picker — are its natural home.
