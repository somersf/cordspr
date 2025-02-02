![spr](./docs/spr.svg)

# spr &middot; [![GitHub](https://img.shields.io/github/license/getcord/spr)](https://img.shields.io/github/license/getcord/spr) [![GitHub release](https://img.shields.io/github/v/release/getcord/spr?include_prereleases)](https://github.com/getcord/spr/releases) [![crates.io](https://img.shields.io/crates/v/spr.svg)](https://crates.io/crates/spr) [![homebrew](https://img.shields.io/homebrew/v/spr.svg)](https://formulae.brew.sh/formula/spr) [![GitHub Repo stars](https://img.shields.io/github/stars/getcord/spr?style=social)](https://github.com/getcord/spr)

A command-line tool for submitting and updating GitHub Pull Requests from local
Git commits that may be amended and rebased. Pull Requests can be stacked to
allow for a series of code reviews of interdependent code.

spr is pronounced /ˈsuːpəɹ/, like the English word 'super'.

## Changes specific to `github.com/aristanetworks/cordspr`

### Breaking Changes

#### Commit Trailers

Change the names of sections added to commits, to be more like git commit trailers.
At a minimum, there cannot be whitespace within a trailer token, so replace whitespaces with
dashes (`-`).

It also appears that trailer tokens are case-sensitive, so don't accept lowercase equivalents
when "parsing" the commit message.

**NOTE** The commit trailer / commit section handling is brittle; only minor changes were
made to the code while still evaluating the suitability of the tool.  Ideally, git tooling
(e.g. using `git interpret-trailers`) should be used for fetching and updating them.
That being said, a "test plan" doesn't really seem to be the kind of thing to put into a
commit trailer...

#### Miscellaneous

- Rename executable to `cspr`.  There are several "stacked pull request" tools in existence;
change the name to avoid at least some conflicts ("c" for "cord").
- When summarizing diffs, require the user to enter `ABORT` instead of ampty string to abort.
Change message to "No description" if none is entered.

### Configuration

There are several changes and additions to the configuration settings stored in `.git/config`
in the `[spr]` section.

#### requireTestPlan

Change the default from `true` to `false`.

#### addReviewedBy

Add a configuration setting to control adding `Reviewed-By` trailers to commit messages.
Change the default from `true` to `false`.

#### autoUpdateMessage

Add a configuration setting to automatically set `--update-message` when updating changes.
The default is `true`, which makes the git commit message and title the source of truth.

#### addSprBannerComment

Add a configuration setting to enable/disable adding `[spr]` and `Created by spr X.Y.Z`
comments to generated commits.  Other comment fragments are preserved, such as `Initial commit`,
though the initial letters are uppercase, so they read slightly better without the banner text.

Change the default from `true` to `false`.

#### addSkipCiComment

Add a configuration setting to enable/disable adding `[skip ci]` to the initial generated commit
for a PR.

Change the default from `true` to `false`.

## Documentation

Comprehensive documentation is available here: https://getcord.github.io/spr/

## Installation

### Binary Installation

#### Using Homebrew

```shell
brew install spr
```

#### Using Nix

```shell
nix-channel --update && nix-env -i spr
```

#### Using Cargo

If you have Cargo installed (the Rust build tool), you can install spr by running

```shell
cargo install spr
```

### Install from Source

spr is written in Rust. You need a Rust toolchain to build from source. See [rustup.rs](https://rustup.rs) for information on how to install Rust if you have not got a Rust toolchain on your system already.

With Rust all set up, clone this repository and run `cargo build --release`. The spr binary will be in the `target/release` directory.

## Quickstart

To use spr, run `spr init` inside a local checkout of a GitHub-backed git repository. You will be asked for a GitHub PAT (Personal Access Token), which spr will use to make calls to the GitHub API in order to create and merge pull requests.

To submit a commit for pull request, run `spr diff`.

If you want to make changes to the pull request, amend your local commit (and/or rebase it) and call `spr diff` again. When updating an existing pull request, spr will ask you for a short message to describe the update.

To squash-merge an open pull request, run `spr land`.

For more information on spr commands and options, run `spr help`. For more information on a specific spr command, run `spr help <COMMAND>` (e.g. `spr help diff`).

## Contributing

Feel free to submit an issue on [GitHub](https://github.com/getcord/spr) if you have found a problem. If you can even provide a fix, please raise a pull request!

If there are larger changes or features that you would like to work on, please raise an issue on GitHub first to discuss.

### License

spr is [MIT licensed](./LICENSE).
