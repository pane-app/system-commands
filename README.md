# System Commands

System Commands is one of [Pane](https://github.com/pane-app/pane)'s
official extensions, and it runs on Windows alone: its `pane.json`
declares `windows` only, so Pane installs it as a default extension on
Windows and never elsewhere. It holds the twenty-one no-view commands of
the session, power, audio, bin, appearance and device kinds — Lock
Screen, Log Out, Restart, Shut Down, Sleep, Hibernate, Turn Off Displays,
Start Screen Saver, Volume Up, Volume Down, Toggle Mute, Set Volume,
Toggle Microphone Mute, Open Recycle Bin, Empty Recycle Bin, Toggle
System Appearance, Toggle HDR, Show Desktop, Toggle Hidden Files, Eject
Removable Drives and Toggle Bluetooth — all sharing one component, each
calling the Windows API the host function named for it.

A command's hotkey runs it without showing Pane's window, and it answers
with a HUD of the state the system ended in — "Restarting", "Sleeping",
"Volume 52%", "Muted" — or why nothing changed. Log Out, Restart, Shut
Down and Empty Recycle Bin ask first, with a destructive confirmation
offering "Don't ask again": a mistyped query never ends a session or
empties the bin. Restart and Shut Down then force applications closed,
so an application with unsaved work cannot hold one up, while Log Out
leaves applications the chance to save. Set Volume takes its level (0 to
100) through Pane's argument form.

This repository holds the extension's whole package, in the one-extension
shape of [ADR 0044](https://github.com/pane-app/pane/blob/main/docs/adr/0044-a-git-repository-holds-one-extension-or-a-collection.md):
[`pane.json`](pane.json), the images it names, and the Rust source
([`src/`](src), [`Cargo.toml`](Cargo.toml)) of its WebAssembly component.
The [release tags](#releases) hold the built component beside the
manifest and the images.

## Install

Install it from Git with Pane alone — no Git, compiler or other tool is
needed on your computer. In root search, choose **Install extension from
Git…** and name

    https://github.com/pane-app/system-commands@v0.2.2

or, from a terminal, `pane --install git:github.com/pane-app/system-commands@v0.2.2`.
A `v<version>` tag pins a release: the tag's commit holds the package
complete, at the version `pane.json` names. Without a reference, Pane
installs the default branch, which between releases holds only the source
and is explained to users as source-only.

## Releases

A release is made from GitHub: **Actions → Release → Run workflow** on the
branch to release. The workflow builds the component with the same CI,
copies it where `pane.json` names it, commits that on the `release`
branch, tags the commit `v<pane.json's version>` and pushes the branch
and the tag. The tag's commit is the *release revision*: it holds
`pane.json`, the images and the built component (`system_commands.wasm`),
and it is what Pane installs when a user names the tag — and the commit a
Pane release pins for its first setup (pane-app/pane#282).

To release a new version, raise `version` in `pane.json` (and in
`Cargo.toml`) through a pull request, then run the workflow. A tag is never
re-written: a released version that must change is released again as a new
version.

## Building

```
cargo build --release --target wasm32-wasip2
```

builds `target/wasm32-wasip2/release/system_commands.wasm`; copy it to
`system_commands.wasm` at the package root, the path its `pane.json`
names, for a revision users can install — what the Release workflow does.
`rust-toolchain.toml` pins the toolchain Pane builds its own extensions
with.

The component builds against Pane's Rust SDK,
[`pane-extension`](https://crates.io/crates/pane-extension), from crates.io
(`pane-extension = "0.2"` in [`Cargo.toml`](Cargo.toml)); building it needs
no Pane checkout.

## Issues and contributing

This extension's issues live in
[this repository's tracker](https://github.com/pane-app/system-commands/issues).
The extension API — everything a Pane command can do, and how a package is
put together — is documented in Pane's repository:
[the extension guests' README](https://github.com/pane-app/pane/blob/main/guests/README.md)
and [Pane's docs](https://github.com/pane-app/pane/tree/main/docs).

## License

Licensed under either of [Apache-2.0](LICENSE-APACHE) or
[MIT](LICENSE-MIT), as Pane is: choose either at your option.
