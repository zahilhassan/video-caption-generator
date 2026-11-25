# 🎬 video Caption Generator 

Upload a video, let **Whisper AI** write the captions automatically, design them live (templates, fonts, colors, animations, drag-to-position) and export a new **MP4 with the captions burned in**. Everything runs **locally on your PC** - your videos are never uploaded to any server.

---

## ✨ Features

- Automatic speech-to-text with OpenAI Whisper (word-level timing)
- 9 templates, 13 fonts, text / outline / highlight / box colors, shadow, bold / italic / uppercase
- 7 animations: none, fade, pop, slide up, typewriter, word highlight, word-by-word
- Drag captions anywhere on the video + position and width sliders
- Edit any word after the AI writes it
- Export the finished **MP4**, or **SRT / TXT** files
- Languages: Urdu, Hindi, English, Arabic and many more; optional translate to English
- Runs fully offline after the first-time setup

---

## 💻 Requirements

| Requirement | Details |
|---|---|
| Operating system | Windows 10 / 11, 64-bit |
| Python | 3.10, 3.11 or 3.12 (if missing, `run.bat` tries to install 3.11 automatically via `winget`) |
| Internet | Needed **only for the first run** (downloads ~1-2 GB) |
| Free disk space | ~5 GB recommended |
| RAM | 8 GB recommended (4 GB works with the `tiny` / `base` models) |
| Microsoft Visual C++ Runtime | Needed by PyTorch. Most PCs already have it - [download here](https://aka.ms/vs/17/release/vc_redist.x64.exe) if missing |
| GPU | Not required. Works on CPU. |

---

## 🚀 Installation (Windows) - step by step

### Step 1 - Get the project

**Option A: Download ZIP (easiest)**
1. On this GitHub page click the green **Code** button → **Download ZIP**.
2. Right-click the downloaded ZIP → **Extract All...**
3. Use a **short folder path without spaces**, for example `D:\CaptionGenerator` or `C:\CaptionGenerator`.

**Option B: Git**
```bash
git clone https://github.com/YOUR_USERNAME/video-caption-generator.git
cd video-caption-generator
```

### Step 2 - Run `run.bat`

Double-click **`run.bat`** inside the project folder. That's all - the script does the rest by itself.

> If Windows shows a blue "Windows protected your PC" (SmartScreen) window, click **More info → Run anyway**. The script is plain text; you can open it in Notepad to check it.

### Step 3 - Wait for the first-time setup (5-15 minutes)

> ### ⏳ The first run is slow - this is normal
> This repository is intentionally tiny (only the app code). On the first run, `run.bat` downloads and installs everything the app needs **onto your PC**. Please **keep the black window open** until your browser opens. Speed depends on your internet connection.
>
> **Every next start takes only a few seconds** - nothing is downloaded again.

What `run.bat` does on the first run:

| # | Step | What happens |
|---|---|---|
| 0 | Check Python | Looks for Python 3.10 / 3.11 / 3.12. If not found, tries `winget install Python.Python.3.11`. If that was needed, **close the window and run `run.bat` again** |
| 1 | Create environment | Creates a private Python environment in the `venv/` folder (your system Python is not touched) |
| 2 | Download FFmpeg | Downloads FFmpeg (~90 MB) into `ffmpeg_bin/` |
| 3 | Install packages | Installs PyTorch, Whisper, Flask and the other packages from `requirements.txt` |
| 4 | Check runtime | Checks that the Microsoft Visual C++ Runtime is present |
| 5 | Start app | Starts the local server and opens your browser at `http://localhost:8000` |

### Step 4 - Use the app

1. Your browser opens the app automatically. If not, open **http://localhost:8000**.
2. Upload a video.
3. Choose the language and the AI model, then press **Generate captions**.
   - The **first time** you generate captions, the Whisper model is downloaded once (see the table below) and saved in `models/`.
4. Edit the words, pick a template / font / colors / animation and drag the captions where you want them.
5. Export the **MP4** (captions burned in), or **SRT / TXT**.

To stop the app, close the black `run.bat` window.

### Next time

Just double-click **`run.bat`** again. It starts in a few seconds.

---

## 📦 What gets installed?

Everything is installed **inside the project folder** (except Python itself, if it was missing).

| Component | Purpose | Approx. size | Location |
|---|---|---|---|
| Python 3.11 (only if missing) | Runs the app | ~100 MB | System (via winget) |
| Virtual environment | Keeps packages separate from your system | small | `venv/` |
| FFmpeg | Reads the video / audio and burns captions into the MP4 | ~100 MB | `ffmpeg_bin/` |
| PyTorch | AI engine used by Whisper (CPU build) | ~200-250 MB download | `venv/` |
| openai-whisper | Speech-to-text AI | small | `venv/` |
| numpy, numba, llvmlite, tiktoken, tqdm, more-itertools, regex, sympy, networkx ... | Dependencies of Whisper / PyTorch | several 100 MB | `venv/` |
| Flask, Waitress, Requests | The local web server | small | `venv/` |
| Whisper model | The AI model itself (downloaded on first "Generate captions") | 75 MB - 1.5 GB | `models/` |

**Total:** roughly **1-2 GB download** and **3-5 GB disk space** (depending on the Whisper model you use).

### Whisper models

| Model | Download size | Speed | Accuracy | Good for |
|---|---|---|---|---|
| `tiny` | ~75 MB | Very fast | Basic | Quick tests |
| `base` | ~140 MB | Fast | Okay | Clear English audio |
| `small` | ~460 MB | Medium | Good | **Recommended** - Urdu / Hindi / English |
| `medium` | ~1.5 GB | Slow | Very good | Difficult audio, accents |
| `turbo` | ~1.5 GB | Medium | Very good | Best quality/speed balance if you have a strong PC |

> Bigger models need more RAM and take longer on a CPU. Start with `small`.

---

## ⚙️ Configuration (optional)

To change the port, copy `config.example.env` to `config.env` and edit it:

```
PORT=8000
```

---

## 🧰 Troubleshooting

| Problem | Solution |
|---|---|
| `Python 3.10, 3.11 or 3.12 was not found` | Install Python 3.11 from [python.org](https://www.python.org/downloads/) and tick **"Add python.exe to PATH"** during setup. Then run `run.bat` again |
| Python was installed by `run.bat` but still "not found" | Close the window and double-click `run.bat` again (Windows needs a fresh window to see the new Python) |
| `Microsoft Visual C++ Runtime is missing` | Install it from https://aka.ms/vs/17/release/vc_redist.x64.exe and run `run.bat` again |
| Setup stopped / internet dropped | Just run `run.bat` again - it continues where it stopped |
| FFmpeg download failed | Download FFmpeg manually from [gyan.dev](https://www.gyan.dev/ffmpeg/builds/), copy `ffmpeg.exe` from its `bin` folder into a new `ffmpeg_bin` folder in the project, then run `run.bat` |
| Port 8000 is already in use | Create `config.env` with `PORT=8001` (or any free port) |
| Browser does not open | Open **http://localhost:8000** manually (use `localhost`, not another address) |
| Captions are slow to generate | Use a smaller model (`tiny`, `base`, `small`). CPU processing of long videos can take several minutes |
| Antivirus blocks something | Allow the project folder, or add it as an exception - the app is a normal Python script |
| Something is broken after an update | Delete the `venv` and `ffmpeg_bin` folders and run `run.bat` again (clean reinstall) |

### Offline use
After the first run, the app works without internet. To use a Whisper model offline, copy its `.pt` file into the `models/` folder.

---

## 🔄 Update

- **ZIP:** download the new ZIP and replace the old files (you can keep your `venv/`, `ffmpeg_bin/` and `models/` folders so nothing is downloaded again).
- **Git:** run `git pull`.

If `requirements.txt` changed, delete `venv\.deps_installed` and run `run.bat` once more.

## 🗑️ Uninstall

Delete the project folder - that removes the environment, FFmpeg and the AI models. (Python itself, if it was installed by `winget`, can be removed from **Settings → Apps**.)

---

## 🔒 Security built in

- Host check against DNS rebinding; Origin check on all actions
- Strict Content-Security-Policy, no-sniff, no-framing, no-referrer
- Upload type whitelist, input validation, rate limits, one job at a time per video, no shell commands
- Server listens on `127.0.0.1` only (not reachable from other devices)
- Uploaded and generated files older than 24 hours are deleted automatically

---

## 📁 Project structure

```
video-caption-generator/
├── run.bat               # one-click installer + launcher
├── app.py                # web server (Flask + Waitress)
├── transcriber.py        # Whisper speech-to-text + FFmpeg caption burning
├── requirements.txt      # Python packages (installed automatically)
├── config.example.env    # optional settings (copy to config.env)
├── static/               # interface: index.html, style.css, core.js, app.js
│
├── venv/                 # created by run.bat  (not in GitHub)
├── ffmpeg_bin/           # created by run.bat  (not in GitHub)
├── models/               # Whisper models      (not in GitHub)
├── data/                 # temporary data      (not in GitHub)
└── workspace/            # uploaded videos     (not in GitHub)
```
---


**Created by Zahil Hassan**
