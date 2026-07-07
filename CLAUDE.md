# CLAUDE.md

## What this is

Floris Calkoen's personal macOS dotfiles, forked from
[holman/dotfiles](https://github.com/holman/dotfiles) (`upstream` remote) and
published at `origin` as `FlorisCalkoen/dotfiles-m5`. It's topic-based
shell/editor/system configuration, not an application — there is no build,
no package to publish, and no runtime beyond zsh/bash and the tools it
configures.

## Layout and conventions (from holman/dotfiles' topical structure)

- Config is organized into topic directories (`zsh/`, `git/`, `vim/`,
  `ruby/`, `macos/`, `homebrew/`, `system/`, `xcode/`, `yarn/`, `docker/`,
  `editors/`, `atuin/`, `functions/`, `bin/`).
- `topic/*.zsh` files are auto-loaded into the shell.
- `topic/path.zsh` loads first (sets up `$PATH`); `topic/completion.zsh`
  loads last (sets up completions).
- `topic/install.sh` runs when `script/install` is run (extension is `.sh`,
  not `.zsh`, specifically so it isn't auto-loaded into the shell).
- `topic/*.symlink` files get symlinked into `$HOME` (without the
  `.symlink` extension) by `script/bootstrap`.
- `bin/` is added to `$PATH` — anything placed there becomes globally
  available.

## Constraints

- `git/gitconfig.local.symlink` is git-ignored. It's generated from
  `git/gitconfig.local.symlink.example` by `script/bootstrap`, which fills in
  the user's git author name/email/credential helper. Never commit this file
  or write real personal values into the `.example` template.
- No secrets live in this repo (no `.env`, credentials, or tokens found).
  Keep it that way — machine-specific or sensitive values belong in the
  git-ignored local files, not in tracked topic files.

## Tooling status (stated as-is, not aspirational)

- No test suite, linter, formatter, or CI exists in this repo. Changes are
  validated by sourcing/running the shell scripts directly.
- Dependencies are declared in `Brewfile` (Homebrew Bundle) and installed via
  `script/install` / `bin/dot`. Don't scatter ad hoc `brew install` /
  `gem install` calls inside scripts — add to `Brewfile` instead.

## Code quality

- Match the existing style of the file you're editing (e.g. the
  `info`/`success`/`fail` helper pattern in `script/bootstrap`) rather than
  introducing a new one.
- Prefer POSIX-portable `sh` for `install.sh` scripts (some are run with
  `sh -c`, not zsh) and zsh-specific syntax only in `.zsh`/`.symlink` files
  that are explicitly loaded by zsh.
- Keep additions topic-scoped: a new tool/language gets its own topic
  directory rather than being bolted onto an unrelated one.
- Minimize dependencies; no speculative scaffolding or half-finished
  installers.
