# Video Downloader

A clean, professional Streamlit web application for downloading publicly
accessible videos that you are authorized to download, powered by
[yt-dlp](https://github.com/yt-dlp/yt-dlp).

## Description

Video Downloader lets you paste a video URL, review its metadata
(thumbnail, title, duration, uploader), pick from the available quality
options, and download the result directly from your browser. It is built
with a simple, modular Python structure so it is easy to read, extend,
and run locally on Windows, macOS, or Linux.

This tool is intended only for downloading content that is publicly
available and that you have the right to download. See the
[Responsible use and legal note](#responsible-use-and-legal-note) section
below.

## Features

- Simple, professional interface built with Streamlit
- Fetches video metadata: thumbnail, title, duration, uploader
- Displays only the quality options actually available for a given video
  (Best available, 1080p, 720p, 480p, 360p, Audio only)
- Uses FFmpeg to merge separate video and audio streams when required
- Live download progress indicator
- Human-readable error messages for invalid URLs, unsupported sites,
  unavailable videos, network issues, and missing FFmpeg
- Automatic cleanup of temporary files
- No API keys, credentials, or external services required
- Custom design with a gradient hero header, card-based layout, and
  subtle animations (fade-in sections, hover effects, animated progress
  bar) defined entirely in `static/style.css`

## Customizing the design

All visual styling lives in a single file, `static/style.css`, which is
injected into the app on startup. To change colors, fonts, spacing, or
animations, edit the CSS variables near the top of that file (for
example `--vd-primary` and `--vd-accent` control the main color scheme)
and refresh the app in your browser - no Python changes required.

## Technologies used

- [Python 3.11+](https://www.python.org/)
- [Streamlit](https://streamlit.io/)
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- [FFmpeg](https://ffmpeg.org/) (for merging streams and audio extraction)

## Project structure

```
video-downloader/
│
├── app.py                     Main Streamlit application
├── downloader/
│   ├── __init__.py
│   ├── extractor.py           Fetches video metadata and available formats
│   └── downloader.py          Downloads the selected format with yt-dlp
│
├── utils/
│   ├── __init__.py
│   └── helpers.py             Validation, formatting, and file utilities
│
├── static/
│   └── style.css               Custom styling, colors, fonts, and animations
│
├── requirements.txt
├── README.md
├── .gitignore
└── .streamlit/
    └── config.toml            Streamlit theme and server settings
```

## Installation

### 1. Prerequisites

- Python 3.11 or newer
- [FFmpeg](#ffmpeg-setup) installed and available on your system PATH

### 2. Clone or extract the project

Extract the project files (or clone the repository) into a folder, then
open that folder in VS Code.

### 3. Create and activate a virtual environment (Windows)

Open a terminal in VS Code (PowerShell) inside the project folder and run:

```powershell
python -m venv venv
venv\Scripts\activate
```

If you use Command Prompt instead of PowerShell, activate with:

```cmd
venv\Scripts\activate.bat
```

On macOS/Linux, the equivalent commands are:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 4. Install dependencies

With the virtual environment activated, run:

```bash
pip install -r requirements.txt
```

## FFmpeg setup

### Why FFmpeg may be required

Many video platforms serve high-quality video and audio as separate
streams. To produce a single playable file at resolutions like 1080p or
720p, or to extract audio-only downloads as MP3, yt-dlp needs FFmpeg to
merge or convert these streams. Without FFmpeg, some quality options may
not be selectable and audio extraction will not work.

### How to install FFmpeg on Windows

1. Download a Windows build from the official FFmpeg site:
   https://ffmpeg.org/download.html (the "Windows builds" links point to
   trusted community builds such as gyan.dev or BtbN).
2. Extract the downloaded ZIP file to a permanent location, for example
   `C:\ffmpeg`.
3. Add the `bin` folder inside it (for example `C:\ffmpeg\bin`) to your
   system PATH:
   - Search for "Environment Variables" in the Windows Start menu.
   - Under "System variables", select `Path` and click "Edit".
   - Click "New" and add the path to the `bin` folder.
   - Click OK on all dialogs to save.
4. Close and reopen your terminal so the updated PATH takes effect.

### How to verify FFmpeg is available

Run the following command in a terminal:

```bash
ffmpeg -version
```

If FFmpeg is installed correctly, this prints version information. If you
see a "command not found" or "not recognized" error, FFmpeg is not yet on
your PATH. The application will also detect this automatically and show a
warning in the interface.

## How to run the application

With your virtual environment activated and dependencies installed, run:

```bash
streamlit run app.py
```

Streamlit will start a local server and open the application in your
default web browser, typically at `http://localhost:8501`.

## Example usage

1. Launch the app with `streamlit run app.py`.
2. Paste a publicly accessible video URL into the input field.
3. Click "Fetch video information" to load the thumbnail, title,
   duration, and uploader.
4. Choose a quality from the available options.
5. Click "Download selected quality" and wait for the progress bar to
   complete.
6. Click "Save file to your computer" to store the downloaded file.

## Troubleshooting

**"FFmpeg was not detected on this system"**
Install FFmpeg and ensure its `bin` folder is on your system PATH, then
restart your terminal and the Streamlit app.

**"This website or URL is not supported"**
The URL either does not point to a video or belongs to a site that
yt-dlp does not currently support.

**"This video is unavailable" / "This video is private"**
The video has been removed, made private, or otherwise restricted by its
owner or platform. This application does not attempt to bypass such
restrictions.

**"A network error occurred"**
Check your internet connection and try again. Some networks or firewalls
may block outbound requests to certain video platforms.

**Download seems stuck at 0%**
Some servers do not report a total file size, so the progress bar may
show generic status text instead of a percentage. The download is likely
still proceeding; check your terminal output for activity.

**Port already in use**
If port 8501 is already taken, run `streamlit run app.py --server.port 8502`
(or any other free port).

## Responsible use and legal note

This application is provided as a general-purpose tool for downloading
video content. You are solely responsible for ensuring that your use of
this tool complies with the terms of service of any website you use it
with, as well as applicable copyright and intellectual property laws in
your jurisdiction.

Only use this tool to download:

- Content that is publicly accessible without bypassing logins,
  paywalls, or DRM
- Content you own, have created, or have explicit permission to download
- Content whose license or platform terms permit downloading

This project does not implement, and will not be extended to implement,
any mechanism for bypassing DRM, paywalls, authentication, or other
access controls, and it does not facilitate the download of private or
restricted content.
