# Copilot Instructions for Dopamine2-roothide

## Project Overview
- **Dopamine2-roothide** is a jailbreak-related project for iOS, with a modular structure:
  - `Application/`: Main Dopamine iOS app (Objective-C/Swift, Xcode project)
  - `BaseBin/`: Core binaries, hooks, and utilities (C, Swift, Makefiles)
  - `Packages/`: DEB packaging and installable components

## Architecture & Key Components
- **App (`Application/Dopamine/`)**: iOS app, uses entitlements, custom Info.plist, and exploits (see `Exploits/`).
- **BaseBin**: Contains submodules (e.g., `dyldhook`, `libjailbreak`, `systemhook`) built as binaries/dylibs for jailbreak operations.
- **MachOMerger**: Merges MachO binaries, requires special linker flags (see its README).
- **Packages**: Contains DEB package definitions and Makefiles for packaging.

## Build & Developer Workflows
- **Top-level build**: Run `make` in the repo root to build all components.
- **Clean**: `make clean` at root or in subdirs.
- **App build**: `make` in `Application/` uses Xcode and ldid for signing. Produces `Dopamine.ipa` and `Dopamine.tipa`.
- **BaseBin build**: `make` in `BaseBin/` builds all sub-binaries and creates `basebin.tar`.
- **DEB packaging**: `make` in `Packages/` builds all DEB packages.
- **Nightly builds**: Set `NIGHTLY=1` to embed commit hash and enable nightly mode.
- **Update device**: Use `make update` or `make update-basebin` (requires `DEVICE` env var, uses `scp`/`ssh`).
- **GitHub Actions**: See `BUILD.md` for GitHub-based build instructions.

## Project Conventions & Patterns
- **Entitlements**: Many binaries/apps require custom entitlements (see `.entitlements` files).
- **No code signing by default**: Xcode builds use `CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO`.
- **Resource structure**: Localizations in `*.lproj/`, assets in `Assets.xcassets/`.
- **Subproject Makefiles**: Each submodule (e.g., `dyldhook`, `libjailbreak`) has its own Makefile and may depend on others.
- **MachOMerger**: When merging, ensure the dylib is built with `-Xlinker -add_split_seg_info -Xlinker -no_auth_data`.

## Integration & Dependencies
- **Cross-component dependencies**: Some subprojects depend on outputs from others (e.g., `libjailbreak` is used by several hooks).
- **External tools**: Requires `ldid`, `xcodebuild`, `codesign`, `scp`, `ssh` for full workflows.
- **Swift/C/Objective-C**: Codebase mixes languages, especially in `BaseBin` and `Application`.

## References
- See `README.md` and `BUILD.md` for build and workflow details.
- See each subproject's Makefile for build logic and dependencies.
- For packaging, see `Packages/` and its subfolders.

---

**If you are an AI agent, follow these conventions and reference the above files for project-specific logic.**
