# The Neighbours

just is not the only way to give a
project's commands names, and several
of its neighbours do things it
deliberately does not. An honest map.
(The comparisons are characterised
from each tool's documentation, not
benchmarked; this guide's series has a
Make guide and a Bash guide, named
here once each, as the criteria
promise.)

## make

The ancestor, and still the right tool
for its own job. **make** (the series'
*Make for Mere Mortals* covers it in
full) is a build tool: describe files
and how they are made from other
files, and make rebuilds exactly what
is out of date, no more. A large
compiled project under just would
recompile the world on every run;
under make it recompiles the two
files you touched.

The price is make's vocabulary: file
targets, pattern rules, automatic
variables, and a famous sensitivity to
tabs. Most projects need *both* shapes
of help — real builds, and a crowd of
commands that build nothing — and a
common, peaceful arrangement is a
justfile whose recipes call `make` for
the building and handle everything
else themselves. Choose just alone
when nothing in the project is
"rebuilt": services, scripts,
documents, deployments, checks.

## Task

**Task** (taskfile.dev) is just's
closest living relative: a command
runner with the same pitch, configured
in YAML files (`Taskfile.yml`) instead
of just's own small language. Task
adds file-based up-to-date checks,
checksums, and includes across files;
its tasks are more verbose to write
and easier to generate from other
tools, being YAML. If your world is
already YAML and you want freshness
checks on top of named commands, Task
is the neighbour to visit. If you want
the file a newcomer can read top to
bottom in one pass, just's plainer
text usually wins.

## npm scripts

If a project is JavaScript, its
`package.json` probably already has a
`"scripts"` section — named commands,
run with `npm run <name>`. For a Node
project that never leaves Node's
world, that may be all you need. The
limits arrive quickly: the scripts are
one line each unless you shell out,
there are no arguments with defaults
or dependencies worth the name, and
the file's first job is describing the
package, not your workflow. Projects
that outgrow it often keep the scripts
as thin wrappers that call just.

## A folder of shell scripts

The oldest solution: `scripts/build.sh`,
`scripts/deploy.sh`, executable and
honest. It scales further than its
reputation — until it doesn't. There
is no list of what exists (you *can*
`ls`, if the names are kind), no shared
variables except what each script
re-derives, no arguments convention,
and every script re-implements the
"run from anywhere" problem that
chapter 8 showed just solving once,
for the whole project. The series'
*Bash for Mere Mortals* teaches the
scripting itself; just is what you
reach for when the folder of scripts
starts needing an index. Many
justfiles keep the scripts and make
recipes that call them — the scripts
hold the logic, the justfile holds
the map.

## Your shell's own memory

Aliases in `~/.bashrc`, functions,
history search, a notes file: the
zero-install option, and the one this
guide exists to retire for project
work. Shell memory is personal — it
does not travel with the project, it
is not shared with the next person,
and it evaporates on a new machine.
just's whole pitch is that the
commands live *in the project*, where
they can be read, listed, and handed
over. Keep aliases for what is truly
yours alone.
