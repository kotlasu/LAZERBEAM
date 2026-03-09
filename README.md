# 👽💾☄️ LAZERBEAM BACKUP 3000 ☄️💾👽

### *ALL FILES MUST BLEED*

```
░▒▓█▓▒░       ░▒▓██████▓▒░░▒▓████████▓▒░▒▓████████▓▒░▒▓███████▓▒░░▒▓███████▓▒░░▒▓████████▓▒░░▒▓██████▓▒░░▒▓██████████████▓▒░  
░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░    ░▒▓██▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓█▓▒░      ░▒▓████████▓▒░  ░▒▓██▓▒░  ░▒▓██████▓▒░ ░▒▓███████▓▒░░▒▓███████▓▒░░▒▓██████▓▒░ ░▒▓████████▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓██▓▒░    ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░ 
░▒▓████████▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓████████▓▒░▒▓████████▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓███████▓▒░░▒▓████████▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░ 
```

---

## ⚡ What is this?

`lazerbeam.sh` is a Linux terminal script that backs up photos from your iPhone using `rsync` over GVFS. It handles all the pain of USB transfers, works around iOS quirks, shows real-time progress, and is fully resumable.
It was born because my spouse’s phone had tens of thousands of chaotic, half-blurry, WTF photos and memes... and she said _“copying them takes too long - just skip it”_.
Which, for my broken brain, was obviously a challenge. 😈
I didn’t have a Mac nearby, so this little daemon was born.

---

## 💣 Features

- Detects your iPhone mounted via GVFS (gphoto2)
- Auto-generates `.env` and `.env.example` if missing
- ✅ Resumable rsync (interrupt-safe, unplug-safe)
- 💀 Shows each folder + file count + progress
- 🧠 SHA256 post-run duplicate check
  - compares file count and folder size (iPhone vs backup)
- 📓 Logs everything to a text file
- Plays a bell sound when complete (optional: can be silenced)
- Supports:
  - `--help` flag to describe usage
  - environment variable overrides:
    - `BACKUP_DIR`
    - `LOG_FILE`
    - `DISABLE_SOUND`
- 💻 Works with **immutable** distros like Bluefin/Silverblue via toolbox/distrobox
- 🤘 ASCII dystopian-metal banner (generated via [patorjk.com/software/taag](https://patorjk.com/software/taag)), because yes

---

## 🧪 Requirements

- Linux (tested on Fedora Silverblue & Bluefin)
- iPhone mounted via GVFS (via GNOME Files, nautilus etc.)
- `rsync`, `sha256sum`, `gio` - should already be installed
- If using immutable system: run it inside `toolbox` or `distrobox`

⚠️ macOS users: this script won’t work there — use AirDrop, Finder, or photos app.
This tool is for people who chose pain (aka Linux on desktop ❤️‍🔥).

---

## 🚀 Usage

1. Plug in your iPhone.
2. Unlock it and tap “Trust” on the prompt.
3. Run:

```bash
chmod +x lazerbeam.sh
./lazerbeam.sh
```

Optionally, specify a backup path:

```bash
./lazerbeam.sh /your/custom/folder
```

The default path is:

```
~/Pictures/iphone-lazerbackup/
```

You’ll see per-folder status and live rsync output:

```
📁 Found 21 folders. Deploying lazers...

[1/21] 🔫 Checking 202310__
📥 Copying 645 file(s) to ~/Pictures/iphone-lazerbackup/202310__
```

### CLI flags

- `--help` — shows usage information

### Environment variables

You can override default behavior with:

```bash
BACKUP_DIR="/custom/path"
LOG_FILE="/custom/logfile.log"
DISABLE_SOUND=1
```
---

## 🛑 What happens on interrupt?

- You can hit `Ctrl+C` or unplug the phone - no problem.
- Next run will **skip** copied files using checksums (no duplicates).
- Interrupts are logged in `lazerbeam.log`.

---

## 🧪 Post-check: duplicate detection

After transfer, a `sha256sum` scan runs across all copied files.
True duplicates (same hash) are listed in:

```
~/Pictures/iphone-lazerbackup/lazerbeam-duplicates.txt
```
---

## 🛰️ MINI-LAZER

A lightweight standalone script (`mini-lazer.sh`) that verifies if all your photos were backed up - without copying anything.

What it does:

- Compares file count and folder size between iPhone and backup
- Displays ✅ / ⚠️ / ❌ result for each folder
- Logs the result (same log file as main script)

Usage:

```bash
./mini-lazer.sh
```

You can override the backup folder:

```bash
BACKUP_DIR="/your/custom/path" ./mini-lazer.sh
```

---

## 🔍 Example scenario (totally hypothetical, sweatycheese.jpg)

> I needed to offload 160 GB of photos from my girlfriend's iPhone,and her image library was basically a postmodern collage of:
>
> - memes from 2016
> - blurry pigeons
> - ten selfies with the same face
> - 3,911 photos of her dog
>   This script restored my sanity.

---

## 🧃 License

MIT.
Feel free to fork, adapt, or contribute chaos.

---

## ☕ Support this madness

If LAZERBEAM saved you hours of scrolling through blurry memes and cursed screenshots - consider buying me a coffee. It helps me stay caffeinated enough to pump-out more absurd stuff like this one.

<p align="center">
  <a href="https://ko-fi.com/kotlasu" target="_blank">
    <img src="https://ko-fi.com/img/githubbutton_sm.svg" alt="Buy Me a Coffee at ko-fi.com">
  </a>
</p>

