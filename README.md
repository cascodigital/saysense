<div align="center">

# 🍌 BananaPhone

**Primate talks. I make sense.**

You grunt case notes into a microphone in whatever language your brain runs on — Portuguese, Spanish or English — and BananaPhone hands back clean, professional text in the language your tickets demand, already on your clipboard. Evolution, but for paperwork.

Built for IT support: turn spoken case notes into ticket-ready Jira documentation in one click — and keep every word on your own machine if you want to. No banana is sent to the cloud without your consent.

![Status](https://img.shields.io/badge/Status-2.5.0-16A34A?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-2563EB?style=flat-square)
![Casco Digital](https://img.shields.io/badge/Casco-Digital-111827?style=flat-square)
![Platforms](https://img.shields.io/badge/Windows%20%7C%20Linux-supported-success?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-cloud-412991?style=flat-square&logo=openai&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-cloud-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-100%25%20offline-000000?style=flat-square&logo=ollama&logoColor=white)

</div>

---

## The problem it kills

Working tickets in a second language means living in three windows at once: a translator, a text editor, and the ticket system. You dictate or type in your head-language, paste it into a translator, clean it up, then move it into the ticket. Every. Single. Note. That's not knowledge work — that's a primate moving text between trees.

BananaPhone collapses that loop into one hotkey. Talk in your language; polished text lands wherever your cursor is. In **Jira Mode** it goes further — dictate rough notes during the call, then generate a customer-facing reply *and* an internal worklog from all of them at once. You handle the monkey business; it handles the sense-making.

---

## See it

**You speak Brazilian Portuguese. The ticket comes out in English.** That's the whole trick — dictate rough notes in the language you think in, and get a clean customer reply plus a structured internal note in the language your tools and clients expect. The `INPUT → OUTPUT` selector sets the direction; the full text of one real case is in [Jira Mode, end to end](#jira-mode-end-to-end) below.

<div align="center">

<img src="docs/screenshots/demo.gif" alt="Jira Mode end to end" width="560">

<em>Dictate during the call in Brazilian Portuguese; the notes land already in English. One <strong>Generate</strong> turns them into a customer-facing reply and a structured internal note — each editable in place before you copy.</em>

<br><br>

<img src="docs/screenshots/jira-mode.png" alt="Jira Mode workspace" width="560">

<em>The workspace — Customer / Internal / Raw Notes / History as tabs, captured live and generated in one click.</em>

<br><br>

<img src="docs/screenshots/settings.png" alt="Settings" width="360">

<em>One provider selector drives speech, translation and Jira — cloud or fully local — with a live readout of whether audio or ticket text ever leaves the machine.</em>

</div>

---

## What it does

- 🎙️ **Press-to-talk dictation** — hold the button (or the global hotkey `Ctrl+Shift+D`), speak, and it auto-stops on silence. The result is on your clipboard before you reach for it.
- 🌍 **Language routing** — speak Portuguese, Spanish or English; output in any of the three. Brazilian-Portuguese tuned.
- 📋 **Paste & translate** — the **Translate** tab handles text you already have instead of dictating it: paste, `Ctrl+Enter`, and the translation is on your clipboard with its formatting intact.
- 🎫 **Jira Mode** — every dictated note is cleaned into professional English as you capture it. One click turns the pile of notes into a **customer reply** + a **structured internal note**, with switchable tone/length **profiles** (Company, MSP client, Internal helpdesk, Strict).
- 🗣️ **Say the word, get the ticket** — end a normal dictation with the trigger phrase (default *"banana jira"*) and the transcript is promoted into Jira Mode and generated on the spot. Also on the **→ JIRA** button and `Ctrl+Shift+J`.
- 🔁 **Regenerate on the fly** — shorter, more technical, more customer-friendly, or with a follow-up — without re-dictating.
- ✏️ **Editable output** — every generated panel (Customer, Internal, Transcript) toggles to editable in place, so you tweak the one word the model got wrong before it hits your clipboard — no full regenerate.
- 🧠 **One AI selector, four backends** — OpenAI, Gemini, local **Ollama**, or any OpenAI-compatible endpoint. It drives speech, translation and Jira text together.
- ⚡ **Light on resources** — local models unload from RAM 60s after a call instead of squatting on your memory.

---

## Jira Mode, end to end

You work the call in your own language and let BananaPhone do the paperwork. This is the exact case from the demo above — what you say out loud in Portuguese, and the one-click English output that lands.

**🇧🇷 What you say out loud — Brazilian Portuguese:**

> *"cliente reportou que o Outlook está travado em 'Tentando conectar' desde hoje de manhã, verifiquei o painel do Microsoft 365, tudo verde, recriei o perfil do Outlook, limpei as credenciais em cache no Gerenciador de Credenciais e testei pelo Outlook Web, funciona normal por lá, o problema continua só no desktop, pedi pro usuário reiniciar, aguardando confirmação"*

You never see that Portuguese on screen — it's transcribed and translated as you speak, so the note lands in the **Raw Notes** tab already in English. One click on **Generate** then splits it into two audiences:

**🇺🇸 Customer Comment — English (public-facing):**

> We've addressed the issue you reported with Outlook freezing and showing "trying to connect" since this morning. We've performed some troubleshooting steps, and we now ask that you please restart your computer. Once restarted, please let us know if Outlook is working correctly.

**🇺🇸 Internal Note — English (support team only):**

> **Issue:** User reported Outlook freezing and showing "trying to connect" since this morning.  
> **Investigation:** Checked the Microsoft 365 health dashboard; all services green. Tested Outlook Web Access (OWA), working normally. The issue appears isolated to the desktop application.  
> **Actions:** Recreated the Outlook profile. Cleared cached credentials in Credential Manager. Asked the user to restart their machine.  
> **Result:** Pending user restart.  
> **Follow-up:** Awaiting user confirmation after machine restart.

Same notes, switch the **profile** to *MSP client* or *Internal helpdesk* and the tone of both fields shifts; hit **Regenerate → shorter / more technical / add follow-up** to reshape it without re-dictating a word. It reconstructs the ticket from out-of-order notes, keeps your technical identifiers (`Credential Manager`, `OWA`), and never claims a resolution you didn't dictate.

---

## 🔒 Privacy is a setting, not a promise

Most dictation tools ship your microphone to someone else's server. BananaPhone lets you decide, per provider — and the main window shows you the truth in real time:

> **Privacy: speech = Local Whisper (audio leaves: No) · text/Jira = Local (ticket text leaves: No)**

Pick the **Ollama + local Whisper** path and *nothing* leaves the machine: audio is transcribed locally with `faster-whisper`, and translation/Jira text runs on a local LLM. No keys, no cloud, no audit trail. Perfect for ticket content you can't legally send to a third party. Prefer speed and top-tier quality? Switch to OpenAI or Gemini in one dropdown. Your call, every time.

---

## ✨ New in 2.5

- **You already have the text? Paste it** — a **Translate** tab next to Transcript takes text you did not dictate (an e-mail, a chat message, a ticket someone else wrote) and translates it into the selected OUTPUT language. Structure survives: line breaks, lists, signatures, ticket IDs, error codes and commands come out untouched. **Ctrl+Enter** fires it, **Paste** pulls the clipboard in, and the result lands in the Transcript panel — copied, editable, and one click from **→ JIRA**.
- **Silence timeout starts at 3s** — the old 4s / 6s / 8s ladder made every capture wait around after you stopped talking. Now 3s / 4s / 5s / 8s, with 3s as the default.

## ✨ New in 2.4

- **Dictated it as a plain note, want a ticket anyway** — no retyping. Close the dictation with a spoken trigger phrase (default *"banana jira"*, editable in Settings) and the transcript jumps into Jira Mode and generates itself. Works with the window hidden after a hotkey dictation, when there is nothing to click.
- **Same jump, two other ways** — a **→ JIRA** button on the Transcript panel and the global `Ctrl+Shift+J` hotkey, both taking the transcript box as-is, edits included.
- **A Windows install that survives you** (2.4.1 / 2.4.2) — the installer now picks a Python version PyAudio actually ships wheels for, installs dependencies so one failure can't silently gut the rest, and copies the app into `%LOCALAPPDATA%\BananaPhone` instead of running from the unzipped Downloads folder you were about to delete.

## ✨ New in 2.3

- **One name, everywhere** 🍌 — window, taskbar, installer and launchers all say BananaPhone. Same app, same settings, same update path — upgrades install straight over any 2.2.x build.
- **History you can actually use** — click any entry in the History tab to select it; **Reopen**, **Copy Customer** and **Copy Internal** now act on the selected ticket, not just the latest one.
- **A face in the crowd** — proper banana icon in the title bar, taskbar and dock on both Windows and Linux, instead of the generic Python/Tk placeholder.

## ✨ New in 2.2.1

- **Window stops hogging the foreground** — the dictation window pops to the front when you summon it, then releases *always-on-top* after a moment instead of staying glued over every other window for its entire lifetime.

## ✨ New in 2.2

- **Edit before you copy** — Customer Comment, Internal Note and the Dictate transcript are read-only by default but flip to editable with a single **Edit** toggle. Fix a name, a path, a tone the model overcooked — then copy. No round-trip through Regenerate for a one-word change.
- **Copy Transcript** — the Dictate tab gets its own copy button, so the polished transcript is one click away even after you've moved focus.

## ✨ New in 2.1

- **Local Jira that's actually usable** — the offline path now matches the cloud output far more closely: section structure is enforced deterministically, technical identifiers (paths, IDs) are kept verbatim, and the public Customer Comment is re-derived in a dedicated jargon-stripping pass so internal tooling never leaks to the end user. A small local model finally produces a clean, coherent ticket.
- **Pick your local quality** — choose **7B (Balanced)** to run anywhere (even CPU-only) or **14B (Best quality)** for sharper output on a GPU. The picker shows the download size and detects whether this machine has a GPU, so the recommendation is honest — no model tags to memorize.
- **One-shot Jira** — optional *Auto-generate after each dictation*: speak your note once and the ticket is generated automatically, no extra click.

## ✨ New in 2.0

- **Real download progress** — the offline-model download (Whisper + Ollama) now shows live MB/% instead of freezing on a blank screen.
- **Bulletproof local setup** — the app finds, starts and waits for Ollama even right after a fresh install, and pulls the model for you. No more "stuck on starting / model not found."
- **Built-in updater** — checks GitHub on launch and tells you when a newer build is out.

---

## Download

Grab the latest installer from the **[Releases page](https://github.com/cascodigital/bananaphone/releases/latest)**:

| Platform | File |
|----------|------|
| **Windows** | `BananaPhone-Setup-<version>.exe` (installer) or `BananaPhone-Portable-<version>.zip` (no install) |
| **Linux** | `BananaPhone-<version>-x86_64.AppImage` |

Every asset ships with a `.sha256` checksum.

---

## Run from source

### Linux
```bash
git clone https://github.com/cascodigital/bananaphone.git
cd bananaphone
./install.sh                # app + all dependencies (apt/dnf/pacman auto-detected)
./install.sh --with-ollama  # also install Ollama for the fully-offline path
./.venv/bin/python bananaphone.py
```

### Windows
Double-click **`Install-BananaPhone.bat`**, or from PowerShell:
```powershell
.\install_windows.ps1               # installs Python via winget if missing
.\install_windows.ps1 -WithOllama   # also install Ollama
```

---

## Configuration

Everything lives in the in-app **Settings** panel:

- **API keys** — OpenAI and/or Gemini, stored only in `~/.config/bananafone/settings_v2.json` (env vars `OPENAI_API_KEY` / `GEMINI_API_KEY` also work). They never leave the local file.
- **AI provider** — one selector for speech, translation and Jira text: OpenAI, Gemini, Ollama, or a custom OpenAI-compatible URL.
- **Model & server URL** — per provider, with sane defaults. **Download offline models** fetches local Whisper + the Ollama model in one go.
- **Silence timeout** — how long to wait before auto-stopping a capture (3s / 4s / 5s / 8s; default 3s).

No key is required for the Ollama path — the app can install Ollama and pull the model for you straight from Settings.

---

## How it works

```
 mic ──► Speech-to-text                      ──► text (input language)
         OpenAI /audio/transcriptions               │
         Gemini generateContent (WAV inline)        ▼
         faster-whisper (100% offline)        Text AI (OpenAI / Gemini / Ollama)
                                                    │
                                       ┌────────────┴────────────┐
                                       ▼                         ▼
                                    DICTATE                  JIRA MODE
                                    translated text          customer reply
                                    → clipboard              + internal note
```

---

## Support

BananaPhone is free and MIT-licensed. If it saves you a few tickets' worth of typing, you can throw a coffee (or a banana) my way — entirely optional, never gated.

[![PayPal](https://img.shields.io/badge/PayPal-Buy%20me%20a%20coffee-00457C?style=flat-square&logo=paypal&logoColor=white)](https://www.paypal.com/donate/?business=andre%40kittler.com.br&item_name=Support+BananaPhone&currency_code=USD)

---

## License

MIT — see [LICENSE](LICENSE).

<div align="center">
<sub>Project lineage: BananaPhone v1 → BananaPhone v2 → <b>BananaPhone</b> v2.4. 🍌<br>Internal storage paths remain <code>bananafone</code>-compatible for backward compatibility.</sub>
</div>
