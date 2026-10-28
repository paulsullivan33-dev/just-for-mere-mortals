# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's traffic is the lab talking to itself.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 13 names what the tool
   cannot do: it cannot tell what changed on disk or rebuild
   only what is out of date, it holds no state between recipe
   lines, it cannot sandbox or vet the justfile it runs, it
   neither schedules work nor reaches other machines on its
   own, and its Windows dialect rests on a different shell
   than the one this guide's lab could exercise.
8. **The safety rule is in the text, early.** Chapter 1 and
   the README state the rule plainly — *read a justfile before
   you run it, above all one that came with a project you just
   downloaded* — before any procedure depends on it, and
   later chapters point back to it where running first and
   reading later would be easy.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a small Ubuntu sandbox. just **1.59.0**
was downloaded as the official release archive for
`x86_64-unknown-linux-musl` from the project's GitHub releases
page into a scratch folder outside this repository and run
from there; nothing was installed system-wide. All transcripts
live in `just-lab/transcripts/`, and the practice justfile for
each experiment sits beside them under `just-lab/`, named for
the chapter that uses it. Where a transcript shows a folder
path, the sandbox's own absolute paths were normalised to a
reader's home folder (`/home/reader/...`); every command,
output, and exit code is otherwise exactly as produced.

Run for real, with transcripts as evidence:

- Version output and the bash completion line the binary
  prints for itself (t00).
- A first justfile: bare `just` running the first recipe,
  recipes run by name, and `--list` taking its descriptions
  from the comments (t01).
- Parameters: a required argument, a defaulted one, a
  variadic `*rest`, the error for a missing argument with its
  usage line, and the error when an extra word is read as
  another recipe's name (t02).
- Variables: plain assignment, a backtick value taken from
  `date`, `export` bridging a value into the shell's
  environment, `env_var` reading the caller's environment,
  and `env_var_or_default` taking its fallback (t03).
- Dependencies: before-dependencies, after-dependencies
  behind `&&` running after the body, a dependency receiving
  an argument in parentheses, and a recipe asked for twice
  running once (t04).
- Running from anywhere: the upward search from two folders
  deep, recipes executing in the justfile's folder while
  `invocation_directory()` reports the true one, and
  `--justfile` with `--working-directory .` overriding both
  (t05).
- Organising: groups (including the comment-then-attribute
  ordering rule, found by breaking it first), `--list
  --group`, an alias in use and in the listing, a `[private]`
  recipe hidden from `--list` and `--summary` yet runnable,
  and `--summary` itself (t06).
- Failures: an unknown recipe, and a recipe whose middle
  line fails, stopping the run with the recipe name, line
  number, and exit code (t07).
- Scripts: a shebang recipe whose loop shares one shell, the
  same lines as an ordinary recipe failing because each line
  gets a fresh shell, and an `@`-quieted line (t08).
- Settings: `set dotenv-load` feeding a recipe from a
  neighbouring `.env` file, and `set shell := ["bash",
  "-cu"]` proven by the shell naming itself in its own
  line (t09).
- Inspecting: a command-line variable override beating the
  file, `--dry-run`, `--evaluate`, and `--show` (t10).
- Modules and expressions: a `mod kitchen` module listed as
  a group and run as `just kitchen bake`, string joining
  with `+`, an `if` expression, and a `[no-cd]` recipe
  keeping the caller's folder (t11).
- The cookbook patterns: a default recipe that lists the
  menu, a `-` line whose failure is tolerated, and
  `[parallel]` dependencies whose output interleaves (t12);
  the composed cookbook justfile of chapter 11 run end to
  end, dated backup archive and `[confirm]`ed clean
  included (t15); and the cookbook's recipes re-run under a
  bare environment (`env -i` with a minimal PATH) for
  chapter 14.
- `[confirm]` answered both ways, yes and no (t14).
- The error for a body line that lost its indentation,
  refusing the whole file before anything runs (t13), and
  `just --fmt --check` answering a scruffy justfile with a
  diff and a nonzero exit code, with `--dump` printing the
  file as parsed.

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- Installs other than the release archive the lab used:
  Homebrew, Cargo, winget/Scoop/Chocolatey, distribution
  packages, and the npm wrapper.
- Everything Windows and macOS specific: PowerShell as the
  per-line shell, `set windows-shell` and
  `set windows-powershell`, `.exe` archives, and macOS path
  spellings.
- Flag-style parameters (`--env=test`) and
  `set positional-arguments`.
- `import` merging a second file's recipes (modules, the
  other splitting mechanism, were run).
- Recipes being private by a leading `_` in the name.
- The interactive `--choose` picker, which needs a terminal
  and an external fuzzy finder.
- The comparison tools in chapter 12 are characterised, not
  benchmarked.

Fixtures are this guide's own and deliberately generic: a
pretend widget project, greetings and snacks, and a notes
folder with one invented sentence in it. No example is
modelled on any real person, network, service or
organisation, and nothing was run except commands the lab
wrote itself, in folders it made for the purpose.
