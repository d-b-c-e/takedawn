# Burnout 3: Takedown (Takedawn) — agent guide

Portfolio rules: `E:\Source\dbce-project-mgmt\HOW-WE-WORK.md` (read first; it replaces older process, lease, review and testing instructions). Current focus: `E:\Source\dbce-project-mgmt\NOW.md`. Feature status lives in the portfolio grid (`python E:\Source\dbce-project-mgmt\tools\portfolio_grid.py plan burnout3`), not in this file. Grid id: `burnout3`.

## Project

Takedawn: unofficial static recompilation of the original Xbox Burnout 3: Takedown (NTSC-U) into a native program for Mac, Windows, Linux, Steam Deck and Android. Upstream is `xenophobentx/takedawn` and its toolkit `xenophobentx/xboxrecomp` (branch `takedawn`); remote `d-b-c-e/takedawn` (visibility unconfirmed, so do not push). MIT for this repo's code; it grants nothing in the game. No game data in the repo; users supply their own disc.

## Build

Needs your extracted disc at `Burnout 3 Takedown/default.xbe` and a checkout of the toolkit at `../xboxrecomp` (`XBOXRECOMP_DIR`). Then `./regen.sh --disasm` (writes `src/game/recomp/gen/`), `cmake -S . -B build/<platform> -G Ninja -DCMAKE_BUILD_TYPE=Release -DXBOXRECOMP_DIR=../xboxrecomp`, `cmake --build build/<platform>`. Windows uses llvm-mingw (see `docs/windows.md`); all platforms in `docs/building.md`.

## Test

- Installer driver: `python3 -m unittest discover -s tools/install/tests` (no game data).
- Unit tests: build `bo3_controls_test bo3_mixer_test bo3_gp_test bo3_fatal_test`, then `ctest --test-dir build/<platform>/tests/<name>`. Most other tests live in the toolkit.
- Use a scratch profile for every test run (`RECOMP_SAVE_DIR=<scratch>`, `RECOMP_SETTINGS=none`; on Windows point `APPDATA` and `LOCALAPPDATA` at a scratch folder). Details in `docs/testing.md`. The real saves are in `%APPDATA%\Takedawn`.

## Install / run on the rig

Installed copy: `%LOCALAPPDATA%\Programs\Takedawn`, game data in `%LOCALAPPDATA%\Takedawn\Game`. Rig install procedure: unknown — check `docs/windows.md` and `docs/installer.md`.

## Shared code

Shared recomp runtime and enhancements layer come from the `xboxrecomp` toolkit (not the DBCE toolkit). DBCE wheel/FFB toolkit use: unknown — check README. Change shared code in the toolkit and re-sync; never edit the vendored copy.

## Project-specific rules

- Never commit generated C (`src/game/recomp/gen/`), builds or disc files.
- Only the NTSC-U `default.xbe` listed in `tools/install/supported.json` is supported.
- Do not touch real saves, settings or the shader cache in tests.

## Key docs

- `README.md`, `docs/building.md`, `docs/testing.md`
- `docs/windows.md`, `docs/running.md`
- `CONTRIBUTING.md`


<!-- BEGIN project-file-location -->
## Project files and temporary work

Keep project-related research, evidence, scripts, packages, and other working files inside the relevant project folder whenever practical. Keep disposable or machine-specific outputs git-ignored when they do not belong in version control; never ignore files that should be committed merely to hide them. When a project folder is unsuitable, use `E:\Source\temp\<project>` instead of creating scattered drive-root folders. Record the location of material needed by later work and update references when relocating it.

Clean up temporary files and disposable copies promptly once they are no longer useful. Preserve active work, user data, settings, saves, required evidence, backups, rollback artifacts, and worktrees until their retention or replacement is established. Follow existing project, rig, and data-loss safeguards; do not remove files merely because they are old or untracked.
<!-- END project-file-location -->