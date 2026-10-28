# Installing just

just ships as a single small program,
and it is installed the ways single
programs are: from a release archive,
from an operating system's packages, or
from a language's package manager. This
guide's lab used the **release archive**
— a download from the project's own
GitHub releases page, unpacked into a
folder — because that route is the same
on every machine and leaves nothing
behind when the folder is deleted. The
other routes are described from the
project's documentation and marked as
such; they all end at the same `just`
command.

## The release archive (the lab's route)

The project's releases page publishes a
ready-built archive for each system:
`just-<version>-<target>.tar.gz`, where
the target names the system and shape,
for example `x86_64-unknown-linux-musl`
for a 64-bit Linux machine. Inside is
the `just` program itself, its manual
page, and shell completion files.

On a Linux machine:

```bash
mkdir -p ~/just-bin
cd ~/just-bin
curl -LO https://github.com/casey/just/releases/download/1.59.0/just-1.59.0-x86_64-unknown-linux-musl.tar.gz
tar xzf just-1.59.0-x86_64-unknown-linux-musl.tar.gz just
./just --version
```

Check: the version answers with the
release you downloaded. The lab's first
transcript (`just-lab/transcripts/t00-install.txt`)
is exactly this check:

```text
$ just --version
just 1.59.0
```

To use `just` without typing its
address every time, put the folder on
your **PATH** — the list of folders
your shell searches for commands — or
move the program into a folder already
there, such as `~/.local/bin` if that
is on your path. macOS releases are
published the same way (with an
`apple-darwin` target), as are Windows
ones (a `.zip` with `just.exe` inside);
the download-and-unpack shape is
identical, and only the archive's name
changes.

## The package-manager routes

Documented only — the lab did not use
these, but the project's README lists
them and they install the same program:

- **Homebrew** (macOS and Linux):
  `brew install just`
- **Cargo**, Rust's package manager, on
  any system with a Rust toolchain:
  `cargo install just` — this one
  compiles just from source, so it is
  the slow route and the one that works
  where no prebuilt archive exists.
- **Windows:** `winget install just`,
  or Scoop and Chocolatey packages under
  the name `just`.
- **Linux distributions:** several ship
  just in their own repositories (try
  your package manager's search for
  `just`), though a distribution's
  version may lag the releases page by
  months. For a tool whose file format
  still grows new features, that lag is
  the honest argument for the archive.
- **npm:** a `just` package exists that
  fetches the same binary, for projects
  already living in Node.

Whichever route you took, the check is
the same: `just --version` answers. If a
feature in this guide is ever unknown
to your copy of just, a stale
distribution package is the usual
reason, and the release archive is the
fix.

## Shell completions

just can finish its own recipe names
when you press Tab, once your shell is
told how. `just --completions <shell>`
prints the setup line for bash, zsh,
fish, PowerShell and others; for bash
it is a single line:

```bash
eval "$(just --completions bash)"
```

(run by the lab; the transcript is in
`t00-install.txt`). Put that line in
your shell's startup file
(`~/.bashrc` for bash) and completions
survive new terminals. The archive also
carries ready-made completion files in
its `completions/` folder for shells
that prefer them. This is a comfort,
not a requirement: everything in this
guide works by typing recipe names in
full.

## Removing it

An archive install is removed by
deleting the folder and the completion
line, if you added one. A package
manager's install is removed by the
same manager. just keeps no data of its
own anywhere else on the machine: your
justfiles are yours, in your projects,
and removing just leaves them as plain
text files — readable, and a fair
record of the commands even with the
runner gone.
