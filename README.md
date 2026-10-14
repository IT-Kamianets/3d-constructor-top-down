# Top-Down starter

Start a **Top-Down** game on the [IT Kamianets 3D Engine](https://3d.itkamianets.com): viewed from above, games like Hades, Vampire Survivors, Hotline Miami.

The camera and controls for this style are not built yet; [AGENTS.md](AGENTS.md) describes them.

## Start

1. Clone this repository under your game's name.
2. Follow [AGENTS.md](AGENTS.md): create the Unity project, add the engine packages, build the game. Hand it to your AI agent, or follow it yourself.

Engine packages for this style: `3d-unity-core`, `3d-unity-scene`, `3d-unity-input`, `3d-unity-character`, `3d-unity-assets`, `3d-unity-audio`.

Every package, with what it does and how to install it: https://3d.itkamianets.com/packages.md

## Changing an engine package

The `3d-*` packages are shared by every game. To change one, clone it inside this repository (git ignores `3d-*` folders), work on a branch or your fork, and open a pull request to its `master` branch. [AGENTS.md](AGENTS.md) has the steps.

## AI agents

[AGENTS.md](AGENTS.md) is the guide for any AI agent (Codex, Cursor, Copilot and others read it automatically); [CLAUDE.md](CLAUDE.md) points Claude Code to it.
