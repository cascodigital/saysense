# Future Releases

Status as of v2.5.1.

## Done

### Fix Wayland input grab / stuck modifiers on Linux (v2.5.1) ✅
- On Linux running Wayland (Zorin OS / GNOME Wayland), `pynput` hooked into XWayland
  via XRecord to capture global hotkeys. Focus transitions between native Wayland
  windows and the X11 app caused dropped `key release` events, desyncing modifier
  keys (Ctrl/Shift) and pointer grab locks desktop-wide. This broke mouse text
  selection, double-click word selection, and `Ctrl+A` across all applications
  until BananaPhone was closed.
- BananaPhone now detects Wayland sessions (`WAYLAND_DISPLAY`, `XDG_SESSION_TYPE=wayland`,
  or runtime `wayland-0` socket) and disables the `pynput` keyboard listener.
- On Linux, system shortcuts are already natively handled by GNOME / Zorin shortcuts
  calling `bananaphone-toggle` (via `command.json`), so no global hotkey functionality
  is lost. `BANANAPHONE_FORCE_PYNPUT=1` remains available as an override.

### Paste & translate, and a shorter silence timeout (v2.5.0) ✅
- Text he already had (an e-mail, a chat message, a ticket) had to be dictated back
  into the app to get translated. The Dictate panel now has a **Translate** tab:
  paste, `Ctrl+Enter`, result in the Transcript panel and on the clipboard.
- `translate_written_text` is a separate prompt from `transform_output_text` on
  purpose - pasted text has no speech-to-text artifacts to repair, so the prompt
  only translates and holds the layout (line breaks, lists, signatures, IDs,
  error codes, commands). Source language is auto-detected; only OUTPUT matters.
- Silence timeout ladder was 4s / 6s / 8s / off, and 4s was already too long a wait
  after every capture. Now 3s / 4s / 5s / 8s, default 3s.

### Source install lands in the user profile, not Downloads (v2.4.2) ✅
- `install_windows.ps1` built the venv and pointed both shortcuts at whatever folder
  the source zip was unpacked into - typically `Downloads\BananaPhone-Source-<ver>`.
  Cleaning out Downloads silently killed the app, and the shortcut then pointed at a
  dead path.
- The installer now copies the app to `%LOCALAPPDATA%\BananaPhone` (overridable with
  `-InstallDir`) and installs the venv and shortcuts there. Re-running it from the
  install directory itself is detected and reuses it in place instead of self-copying.
- Copying uses robocopy with `/XD .venv .git __pycache__`; `Copy-Item -Recurse` onto an
  existing directory nests it (`docs\docs`) instead of merging.
- Desktop shortcut is always created, gets the real banana icon from
  `assets\bananaphone.ico`, and its parent directory is created first - a redirected
  Desktop (OneDrive/Known Folder Move) no longer silently skips it. The installer warns
  if the shortcut is missing after the attempt.
- Final summary prints the install path and tells you the source folder is disposable.


### Windows installer: Python 3.14 / PyAudio wheel failure (v2.4.1) ✅
- `install_windows.ps1` resolved the base interpreter with `py -3`, which returns the
  **newest** Python present. PyAudio 0.2.14 publishes prebuilt wheels only up to
  cp313, so on Python 3.14 pip fell through to a source build and died with
  "Microsoft Visual C++ 14.0 or greater is required" — unfixable on a locked-down
  work machine where a C++ toolchain is not an option.
- Worse, dependencies installed as a single `pip install -r requirements.txt`. PyAudio
  failing aborted the entire pip transaction, so customtkinter/numpy/faster-whisper
  were never installed either — and the script still printed "BananaPhone installed"
  and created shortcuts pointing at an empty venv.
- Fixes:
  - Interpreter discovery probes `py -3.13/-3.12/-3.11/-3.10` and validates via
    `sys.version_info`, instead of accepting whatever `py -3` hands back.
  - A pre-existing `.venv` built on an unsupported interpreter is detected and
    rebuilt rather than reused.
  - PyAudio installs first and alone with `--only-binary=:all:`, so pip never
    attempts a source build; failure raises an actionable error instead of taking
    every other dependency down with it.
  - The import sanity check now honors `$LASTEXITCODE`, so a broken install can no
    longer report success.

### Window always-on-top fix (v2.2.1) ✅
- The dictation window set `-topmost True` at init and never cleared it, so it stayed
  glued over every other window for its whole lifetime. Now it pops to front on launch
  and releases topmost after 400ms (`bananaphone.py` `DictationApp.__init__`), keeping the
  intended "appear when summoned" behavior without hogging the foreground.

### Local Jira quality + model tiers + one-shot (v2.1.0) ✅
- **Robust local Jira pipeline.** Small local models (qwen2.5:7b) used to break the
  Jira output four ways: sections jammed onto one line, dropped/`None` follow-ups,
  internal jargon leaking into the public Customer Comment, and genericized
  identifiers. All addressed without changing models:
  - A one-shot few-shot example (local providers only) anchors format and verbatim
    identifier preservation. No ticket number/name in the example so the model can't
    parrot it into unrelated tickets.
  - `normalize_jira_sections()` deterministically forces each section label onto its
    own line, independent of how well the model followed the prompt.
  - A forced (then discarded) `resolution_state` key makes the model commit to
    resolved/workaround/open before writing the Result, fixing the "issue persists"
    contradiction.
  - The public Customer Comment is re-derived in a separate, narrowly-scoped
    jargon-strip pass (`refine_customer_comment_local`) — a task a 7B does cleanly —
    instead of being produced in the same breath as the technical note.
  - HARD RULES rewritten as short numbered imperatives (small models follow lists
    better than dense prose). A one-shot repair pass re-asks if a backbone section is
    missing. Local generation timeout raised (a 7B on CPU can take minutes).
- **Local model tiers in Settings.** For Ollama, the free-text model field becomes a
  named picker: **7B — Balanced (recommended)** (`qwen2.5:7b`, ~4.7 GB, runs anywhere)
  or **14B — Best quality** (`qwen2.5:14b`, ~9 GB, needs a 12 GB+ GPU), plus a
  **Custom model…** escape hatch. A best-effort NVIDIA/AMD GPU probe drives an honest
  recommendation, shown only for the 14B tier. The Server URL field is hidden for a
  local preset tier (localhost implied). Tier owns the model tag; the rest of the
  pipeline is unchanged.
- **One-shot Jira.** Optional *Auto-generate after each dictation* setting: a single
  dictated note generates the ticket automatically, no Generate Jira click.

### Generic Jira presets (v2.0.1) ✅
- Renamed the default built-in Jira profile from a client-specific name to the
  generic "Company (Jira)" (id `default`). No client names in the source. A
  saved `active_jira_profile` pointing at the old id resolves gracefully to the
  default built-in (see `get_jira_profile`), so existing configs don't break.

### Offline-models UX + self-update (v2.0) ✅
- Real progress bar during the "Download offline models" flow: the Whisper
  medium (~1.5GB) download now polls the HF cache and shows MB/%, instead of
  freezing on a dead label. The Ollama pull also drives the same bar.
- Robust Ollama bring-up: `find_ollama_binary()` locates the binary at the
  default Windows/macOS install paths even when it isn't on PATH yet (fresh
  install), and the start/install workers retry the serve+poll loop patiently
  instead of giving up after one pass. Fixes the "stuck on Starting Ollama ->
  404 model not found" race where the pull was silently skipped.
- In-app self-update check against GitHub releases. Lists `/releases` (all the
  beta tags are prereleases, so `/releases/latest` is useless) and prompts to
  open the download page when a newer tag exists. Remembers the dismissed tag.

### Jira Profiles (v1.7) ✅
- Structured, switchable profiles driving the built-in Jira prompt: tone,
  length, internal-note section names and extra instructions — no prompt
  editing required.
- Built-in read-only presets: Company (Jira), Casco / MSP client, Internal
  Helpdesk, Strict (factual). Clone-to-edit for custom profiles.
- Quick profile switch in the Jira panel; full manager (new/clone/delete/
  test) in the Jira Profiles dialog.
- Full-custom prompt override kept as an advanced global escape hatch.
- Legacy `jira_extra_instructions` auto-migrated into an editable profile.

### Jira Mode behavior (v1.6) ✅
- Entering Jira Mode (Dictate -> Jira) clears leftover notes so a previous
  ticket's noise doesn't bleed into the next one.
- Output language follows the OUTPUT selector even in Jira Mode (e.g. PT
  output stays Portuguese), with a hard rule in the prompt.

### Main window layout (v1.8 - v1.8.4) ✅
- Two-column layout: left = controls, right = output panel at full window
  height (notes area no longer one cramped line).
- Dictate output uses a single-tab "Transcript" tabview, pixel-identical
  to the Jira tabs (no size/position jump when switching modes).
- Left column pinned to a fixed width (pack_propagate) so the variable
  talk-button text no longer shifts the right panel between modes.

### Call Notes ✅
- Notes already carry timestamps; history preserves time and order.

## Remaining

### Settings UX
- Split the main Settings dialog into tabs or a scrollable layout
  (General / AI Provider / Jira / Advanced). Currently a single flat window.

### Global Hotkeys (partial)
- Only one global hotkey exists today (Ctrl+Shift+D quick dictation).
- Add configurable global hotkeys for: push-to-talk, add Jira note,
  generate Jira output, copy Customer Comment, copy Internal Note.

### Installer and Updates
- In-app update check.
- Improve Windows installer trust/signing story.
- Show version and changelog from inside the app.

### Profile polish (nice-to-have)
- Per-profile language hint / default OUTPUT language.
- Reorder profiles in the dropdown.
- Export/import profiles for sharing across machines.
