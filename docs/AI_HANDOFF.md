# BananaPhone AI Handoff

## Current Product Identity

- Public product name: **BananaPhone**
- Version line: **2.5.0**
- Tagline: **You speak. It makes sense.**
- Internal repository / historical codename: `bananaphone`
- Legacy app kept for reference: `bananafone.py` / `README_V1.md`

## Active Workspace Contract

- Canonical local checkout: `/home/aristofeles/ai/git/github/cascodigital/bananaphone`
- Canonical GitHub repo: `https://github.com/cascodigital/bananaphone`
- This is the only active dictation/Jira app. Bananafone/BananaPhone repos were retired; do not look for the active app under `~/ai/projects` or `~/code/bananafone`.
- Future AI sessions should treat this path as already correct and continue development here unless Andre explicitly requests a move.

The public-facing app name is BananaPhone. Do not rebrand releases, screenshots,
installers, desktop entries, or README copy back to BananaPhone unless the owner
explicitly asks for that rollback.

## Compatibility Decisions

Keep these internal paths and environment variables for backward compatibility:

- Linux settings: `~/.config/bananafone/settings_v2.json`
- Linux logs: `~/.local/state/bananafone/bananaphone.log`
- Jira history: `~/.config/bananafone/jira_history.json`
- Environment variable prefix: `BANANAFONE_*`
- Main source file: `bananaphone.py`

These names are technical debt, but changing them now would lose existing keys,
history, defaults, and support notes. Public branding is BananaPhone; internal
storage remains Bananafone/BananaPhone-compatible until a dedicated migration is
implemented.

## Canonical defaults (keep in sync across files)

- **Default Ollama model: `qwen2.5:7b`.** The runtime source of truth is
  `PROVIDER_DEFAULT_MODEL["ollama"]` in `bananaphone.py`. The provisioning scripts
  `install.sh` and `install_windows.ps1` MUST pull this exact tag. If they
  pull a different model (they used to pull `qwen2.5:3b`), the app requests a
  model that was never pulled and the local endpoint returns
  `404 model not found`. Change all three together or not at all.
- **Whisper speech model: `medium`** (CPU, int8). The offline-download progress
  bar estimates against `WHISPER_MEDIUM_EXPECTED_BYTES` (~1.5 GB).

## Release Checklist (DO THIS EVERY RELEASE — no exceptions)

The maintainer (André) expects the AI assistant — whichever one is driving the
session — to own the release end to end: bump, doc sync, commit, tag, push. Do
not hand these steps back to him. This file is the source of truth; there is no
external/Claude memory to rely on.

The app self-updates by comparing `APP_VERSION` against the newest GitHub
release tag (`bananaphone.py` -> `parse_version_key`). If `APP_VERSION` does not
match the tag you push, the in-app update check is wrong by a version. So on
**every** release, before tagging:

1. **Bump `APP_VERSION` in `bananaphone.py` (line ~36) to match the tag.**
   Tag `v2.0` => `APP_VERSION = "2.0"`. Tag `v2.1-beta.2` => `APP_VERSION = "2.1 Beta"`
   (the parser normalizes "2.1 Beta" to the same key as "v2.1-beta", but the
   beta sub-number only comes from the tag — so for `.N` betas, prefer setting
   `APP_VERSION = "2.1 Beta.2"` to stay in lockstep).
2. **Update `README.md`**: the `Status-<version>` badge AND the
   "Latest release: **vX.Y**" line.
3. **Update `docs/AI_HANDOFF.md`**: the `Version line:` field above.
4. **Update `FUTURE_RELEASES.md`**: the "Status as of vX.Y" line and log what shipped.
5. Commit, then `git tag vX.Y && git push origin main --tags`. The tag push is
   what triggers the CI release build.

These doc/version fields were historically left stale (README sat at 1.5.1
while the app was on 1.9). Keep them in lockstep with `APP_VERSION`.

## Release Rules

- Tags containing `beta`, such as `v1.5-beta.1`, publish prereleases. A tag with
  no `beta` (e.g. `v2.0`) publishes a full (non-prerelease) release.
- GitHub Actions builds:
  - Windows installer: `BananaPhone-Setup-<version>.exe`
  - Windows source zip: `BananaPhone-Source-<version>.zip`
  - Linux AppImage: `BananaPhone-<version>-x86_64.AppImage`
  - Checksums: `*.sha256`
  - Optional diagnostic/portable artifacts: `BananaPhone-Portable-<version>.zip`
    and `BananaPhone-Debug-Console-<version>.zip`
- The active BananaPhone repository is `cascodigital/bananaphone`.
- The historical BananaPhone v2 repository is `cascodigital/bananaphone`.

## Current Workflow

- Dictate mode: speech-to-text plus optional translation to selected output
  language.
- Translate tab (Dictate panel, 2.5.0+): pasted text -> `translate_written_text`
  -> Transcript panel + clipboard. Deliberately a separate prompt from
  `transform_output_text`: written text has no speech-to-text artifacts to
  repair, so the prompt only translates and preserves layout. Source language is
  auto-detected; only the OUTPUT selector matters.
- Jira Mode: captures polished notes, generates Customer Comment and Internal
  Note, validates output, stores local history, and supports regeneration.
- Settings exposes silence timeout, provider selection, API keys, model/server
  settings, and Jira Extra Instructions.
- Hidden advanced Jira full-prompt override exists for power users.
- Gemini text calls cap thinking via `reasoning_effort`, injected in
  `run_text_chat` only when the provider is `gemini` (OpenAI/Ollama reject the
  field). Constants `GEMINI_REASONING_TRANSLATE` (`"none"`) and
  `GEMINI_REASONING_JIRA` (`"low"`) at the top of `bananaphone.py`. This exists
  because `gemini-2.5-flash` thinks by default, which caused random
  multi-minute stalls (worsened by the 90s timeout x3 retry loop) and wasted
  paid output tokens. Tune the budgets via those two constants, not per-call.

## Local Install / Runtime

On Linux, install or refresh from source with:

```bash
cd /home/aristofeles/ai/git/github/cascodigital/bananaphone
./install.sh
```

Current launcher:

- `~/.local/share/applications/bananaphone.desktop`
- Exec: `/home/aristofeles/ai/git/github/cascodigital/bananaphone/.venv/bin/python /home/aristofeles/ai/git/github/cascodigital/bananaphone/bananaphone.py`

Do not launch the GUI during automated verification unless the user explicitly
asks; use `py_compile`, static imports, and package/release checks first.

## Last UI Notes

- The Jira action row should contain only Generate, Clear, regeneration style,
  and Regenerate.
- Customer/Internal copy buttons live inside their own tabs to avoid horizontal
  overflow on the 560 px window.
- History stores the last 10 generated tickets locally and offers latest-output
  reopen/copy actions.

## Pending Branding Debt

- Icon assets still use the old banana-themed filenames and artwork.
- Config/log/env names still use `bananafone` for compatibility.
- Existing screenshots may need a visual refresh after the BananaPhone rename.

## Recommended Next Work

- Replace old banana-themed icon/screenshot assets with BananaPhone visuals.
- Add Jira documentation profiles instead of making users edit prompts first.
- Add Settings tabs or a scrollable Settings layout.
- Add call-note timestamps in Jira Mode.
- Add dedicated Jira action hotkeys.
- Add in-app update check and improve Windows signing/trust.

## Windows Installer Trust Note - 2026-06-14

The `v1.2-beta` Windows installer asset downloaded correctly and was a valid PE
file, but a Windows test machine reported:

```text
Windows cannot access the specified device, path, or file. You may not have the permissions.
```

This is not the normal SmartScreen `Run anyway` path. When the message appears
immediately, including when running as administrator, the likely causes are:

- Windows Defender or another AV blocked/quarantined the unsigned package.
- The downloaded file kept a blocked Mark-of-the-Web zone flag.
- Enterprise policy/AppLocker blocked execution from the download path.

Immediate user-side checks:

```powershell
Get-FileHash .\BananaPhone-Setup-1.2-beta.exe -Algorithm SHA256
Unblock-File .\BananaPhone-Setup-1.2-beta.exe
```

Expected SHA256 for `v1.2-beta` installer:

```text
a45bd54e4d038c363579bdd5a6bb597402b31822a1781007a727505a885cdc6d
```

For `v1.2-beta.1` and later, the release workflow also uploads a portable zip
and SHA256 files. If the installer is blocked, download the portable zip,
unblock the zip before extracting, and run `BananaPhone.exe` from the extracted
folder.

Follow-up: the portable zip extracted correctly as `BananaPhone.exe` plus the
`_internal` dependency folder, but `BananaPhone.exe` still did not launch after the
SmartScreen bypass on the user's Windows machine. That can still be endpoint
policy, but it can also be a hidden runtime crash because the normal PyInstaller
build uses `--windowed`.

For `v1.4-beta` and later, the Windows release workflow also uploads:

- `BananaPhone-Debug-Console-<version>.zip`: a console PyInstaller build. Run
  `BananaPhone-Debug.exe` from PowerShell to see traceback/runtime errors.
- `BananaPhone-Source-<version>.zip`: source + Windows installer scripts. Extract,
  run `Install-BananaPhone.bat`, then launch via the generated shortcut,
  `Run-BananaPhone.bat`, or `.venv\Scripts\pythonw.exe bananaphone.py`.

Portable PyInstaller structure reminder:

```text
BananaPhone.exe
_internal\
```

`_internal` is not executable by itself; `BananaPhone.exe` must live beside it.

## Local Linux Quick Dictation Hotkey - 2026-06-14

Goal: make BananaPhone usable without permanently occupying screen space on the
Linux desktop.

Current desktop environment observed:

- Desktop: Zorin/GNOME
- Session type: Wayland
- `xdotool` is installed and can see the Tk/XWayland BananaPhone window.
- GNOME custom shortcut slot used: `custom3`

Shortcut installed by `install.sh` when `gsettings` is available:

```text
Name: Toggle BananaPhone
Command: /home/aristofeles/.local/bin/bananaphone-toggle
Binding: <Shift><Control>d
```

Before this change, `custom3` was:

```text
Name: dictate
Command: /home/aristofeles/ditado_gui.py
Binding: <Shift><Control>d
```

Local wrapper installed by `install.sh`:

```text
/home/aristofeles/.local/bin/bananaphone-toggle
```

Wrapper behavior:

- Defines the source checkout as `/home/aristofeles/ai/git/github/cascodigital/bananaphone`.
- Defines the app entrypoint as `bananaphone.py` inside that checkout.
- Writes a command request atomically to:

```text
/home/aristofeles/.config/bananafone/command.json
```

- Command payload format:

```json
{"id":"<uuid-or-timestamp>","action":"toggle_quick_dictation","created_at":1781469296}
```

- If a BananaPhone window exists, it tries to activate/raise it with `xdotool`.
- If no window exists, it launches:

```bash
/home/aristofeles/ai/git/github/cascodigital/bananaphone/.venv/bin/python /home/aristofeles/ai/git/github/cascodigital/bananaphone/bananaphone.py
```

App-side implementation:

- `COMMAND_FILE = os.path.join(CONFIG_DIR, "command.json")`.
- `poll_command_file()` runs every 150 ms via `root.after`.
- Commands older than 15 seconds are ignored to prevent stale auto-recording on
  a later app launch.
- On `{"action": "start_hotkey_recording"}` or
  `{"action": "toggle_quick_dictation"}`:
  - `deiconify()`, `lift()`, `focus_force()`
  - if already recording, call `stop_recording()`
  - if model is still loading, mark pending start
  - otherwise call `start_recording(from_hotkey=True)`
- `start_recording(from_hotkey=True)` marks the recording so the app minimizes
  itself after no-audio, transcription success, or transcription error.

Final UX:

1. Press `Ctrl+Shift+D`.
2. BananaPhone opens/raises and immediately starts recording.
3. User speaks normally.
4. Silence timeout stops recording, or pressing `Ctrl+Shift+D` again forces
   stop/transcribe.
5. App transcribes, copies to clipboard, then minimizes.
6. On Windows, the same `Ctrl+Shift+D` toggle is registered globally with
   `pynput` while the app is running. If endpoint policy blocks global hooks,
   the focused-window shortcut still works.

Why this route was chosen:

- Pure global hold-to-talk with `Ctrl+Shift` under GNOME/Wayland is unreliable
  without a dedicated background daemon or compositor-specific integration.
- A GNOME shortcut invoking a small wrapper is much simpler and matches the
  current local install.
- Polling a local command file avoids needing DBus or socket plumbing for this
  private desktop workflow.
