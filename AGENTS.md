# Top-Down starter: agent guide

This repository starts a **Top-Down** game on the **IT Kamianets 3D Engine**: viewed from above, games like Hades, Vampire Survivors, Hotline Miami. A developer clones it under the game's name, and it becomes their game.

For games viewed primarily from above, with direct player movement and action-focused gameplay.

It holds no Unity project yet: creating it is the first task below. Until then the repository is this guide, the git settings and the license.

## Read first

- **https://3d.itkamianets.com/packages.md**: every engine package with its purpose, usage, dependencies, install order and git URL. Fetch it before adding, removing or writing a package; it is the current list, this file is not.
- https://3d.itkamianets.com/llms.txt: short index of the docs.
- https://3d.itkamianets.com/constructors/top-down: this constructor on the website.

## 1. Create the Unity project

1. In Unity Hub, create a new project with the **Universal 3D** template and Unity **6000.6**, in a temporary folder.
2. Move its `Assets/`, `Packages/` and `ProjectSettings/` into this repository, next to this file. Leave `Library/` behind; Unity rebuilds it.
3. Open this repository's folder in Unity Hub (Add › this folder).
4. In `Project Settings › Player`, set Product Name and the application identifier (`com.itkamianets.<game>`).

## 2. Add the engine packages

Merge these into `dependencies` in `Packages/manifest.json`. Unity installs each package's Unity dependencies (such as the Input System) itself, but not other git packages, so every engine package below is listed, including indirect ones.

```json
{
  "com.itkamianets.3d-unity-assets": "https://github.com/IT-Kamianets/3d-unity-assets.git",
  "com.itkamianets.3d-unity-audio": "https://github.com/IT-Kamianets/3d-unity-audio.git",
  "com.itkamianets.3d-unity-character": "https://github.com/IT-Kamianets/3d-unity-character.git",
  "com.itkamianets.3d-unity-core": "https://github.com/IT-Kamianets/3d-unity-core.git",
  "com.itkamianets.3d-unity-input": "https://github.com/IT-Kamianets/3d-unity-input.git",
  "com.itkamianets.3d-unity-scene": "https://github.com/IT-Kamianets/3d-unity-scene.git"
}
```

| Package | Why |
|---|---|
| [`3d-unity-core`](https://3d.itkamianets.com/packages/3d-unity-core) | The shared base for every Unity package: configuration, services, events, logging and startup. |
| [`3d-unity-scene`](https://3d.itkamianets.com/packages/3d-unity-scene) | Loads Scene JSON into live Unity scenes and saves changes back to the shared format. |
| [`3d-unity-input`](https://3d.itkamianets.com/packages/3d-unity-input) | One set of actions for mouse, keyboard, touch, gamepad and VR controllers. |
| [`3d-unity-character`](https://3d.itkamianets.com/packages/3d-unity-character) | A reusable player character: walking, running, jumping, ground, slopes and steps. |
| [`3d-unity-assets`](https://3d.itkamianets.com/packages/3d-unity-assets) | Loads models, materials and textures by id, locally or from a server, with caching. |
| [`3d-unity-audio`](https://3d.itkamianets.com/packages/3d-unity-audio) | Positional 3D sound, sound events with variations, footsteps by surface, volume groups and music. |

**Camera and controls for this style** (not built yet):

| Package | What it does |
|---|---|
| `3d-constructor-top-down-camera` | Overhead camera at a fixed angle that follows the player, with optional zoom. Not built yet. |
| `3d-constructor-top-down-controls` | Movement relative to the screen, not the camera; aim toward the mouse or the right stick. Not built yet. |

**Optional**, add when the game needs it (with its dependencies; packages.md lists the install order):

| Package | Add for |
|---|---|
| [`3d-unity-network`](https://3d.itkamianets.com/packages/3d-unity-network) | online co-op or versus |
| [`3d-unity-firebase`](https://3d.itkamianets.com/packages/3d-unity-firebase) | accounts, saved runs, leaderboards |

## 3. Build the game

1. Create the constructor packages in the engine, modeled on the Character View ones: `3d-constructor-top-down-camera` (overhead follow) and `3d-constructor-top-down-controls` (screen-relative movement, aim). They drive `3d-unity-character`, which already handles movement.
2. Create `Assets/Scenes/Main.unity` with a ground plane, a player using `3d-unity-character`, and the camera.
3. Add the interface packages (`<game>-menus`, `<game>-hud`), then the gameplay (`<game>-core`, `<game>-maps`, `<game>-enemies`).

**Name game packages `<game>-<part>`**, package id `com.itkamianets.<game>-<part>`, e.g. `temnorid-arena-core`. Suggested for this kind of game: `<game>-menus`, `<game>-hud`, `<game>-core`, `<game>-maps`, `<game>-enemies`, `<game>-upgrades`.

- **Interface first, then gameplay.** Menus and HUD give a playable loop early; rules and content follow.
- Game packages live in their own repos, so several games can share one, or under `Packages/<game>-<part>/` while the game is young.
- Scenes, prefabs that wire packages together, and build settings stay in this project; reusable code goes into packages.
- Rewrite `README.md` for the game and this file's first paragraph once the game has a name: what it is, its packages, how to play.

## Rules

- **Read input only through `3d-unity-input`.** Never read devices directly.
- **No game rules in engine or constructor packages.** If more than one game could use something, propose it as an engine package instead of keeping it in the game.
- **Dependencies point one way:** game → constructor → engine → schemas. An engine package never references a game.
- **Schemas are the source of truth** for scenes (`3d-scene-schema`), assets (`3d-asset-schema`) and multiplayer messages (`3d-network-schema`).
- Commit `.meta` files with their assets. Never hand-write them; Unity generates them.

## This repository is the game's workspace

The game's Unity project sits at the root of this repository. Engine repos you need to read or change are cloned **inside** it and ignored by git (`.gitignore` ignores every `/3d-*/` folder), so they never end up in the game's history:

```text
<game-name>/               this repo: the game
  Assets/ Packages/ ProjectSettings/
  AGENTS.md  README.md
  3d-unity-character/      cloned engine repo, git-ignored
```

Normally there is nothing to clone: Unity downloads the engine packages by their git URLs.

## Changing an engine package

Engine packages (`3d-*`) are shared by every game, so changes go back to them through a pull request, never as a copy inside the game.

1. Clone the package into this repository: `git clone https://github.com/IT-Kamianets/3d-unity-character`. Without write access to IT-Kamianets, fork it on GitHub first and clone your fork.
2. Create a branch for the change.
3. Point the game at your copy while you work: in `Packages/manifest.json`, `"com.itkamianets.3d-unity-character": "file:../3d-unity-character"`.
4. Open a pull request to the package's `master` branch. Keep game rules out of it: the change must make sense for every game.
5. After it is merged, switch the manifest back to the git URL (pinned to the new tag) and delete the clone.

**Pin versions** before a release: `https://github.com/IT-Kamianets/3d-unity-core.git#v0.1.0`.

## Git

- `.gitignore` is Unity's standard set plus OS and editor files: never commit `Library/`, `Temp/`, `Logs/`, `UserSettings/` or builds.
- `.gitattributes` marks Unity scenes and prefabs as text (merged with Unity Smart Merge once configured; the command is in the file) and art and audio as binary. Enable the Git LFS lines before the first large asset.
