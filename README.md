# Photo Selector

A single-file browser app for quickly curating photos from a local folder, Google Drive, or Google Photos. Swipe-style UI inspired by dating apps — keep what you love, skip the rest, then save your picks in one click.

No installation, no server required for local use. Just open `index.html` in Chrome or Edge.

---

## Requirements

| | |
|---|---|
| **Browser** | Chrome or Edge |
| **OS** | macOS or Windows |
| **For cloud sources** | App must be served via `python3 -m http.server 8000` (not opened as a file) |

> Safari is not supported — it does not implement the write portion of the File System Access API.

---

## Getting Started

### Local folder (no setup needed)

1. Open `index.html` in Chrome or Edge.
2. Click **Open Local Folder** and select a directory.
3. Grant read/write access when the browser prompts.
4. Swipe through photos, then save.

### Google Drive or Google Photos

Cloud sources require a Google Cloud OAuth Client ID. **One-time setup:**

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a project (or select an existing one)
3. Enable both **Google Drive API** and **Photos Library API**
4. Go to **Credentials → Create → OAuth 2.0 Client ID → Web Application**
5. Under **Authorized JavaScript origins**, add: `http://localhost:8000`
6. Copy the generated Client ID (ends in `.apps.googleusercontent.com`)
7. Serve the app locally:
   ```
   python3 -m http.server 8000
   ```
8. Open `http://localhost:8000` in Chrome
9. Click **Google API Setup** in the app and paste your Client ID

---

## Features

### Landing — three source options

- **Open Local Folder** — opens a local directory via the File System Access API
- **Connect Google Drive** — OAuth login, then browse your Drive folder tree
- **Connect Google Photos** — OAuth login, then pick an album (or "All Photos")
- **Google API Setup** — configure your Client ID (stored in browser localStorage)
- Connection status badge + Disconnect link when signed in to Google

### Viewer

- One photo at a time in a card-style layout
- **Select (✓)** — adds to selection; card animates out right with a **KEEP** stamp
- **Ignore (✕)** — skips; card animates out left with a **SKIP** stamp
- **Keyboard shortcuts:** `→` or `D` to select, `←` or `A` to ignore
- Next photo is preloaded in the background
- Counter bar: `Selected: N` · `current / total` · progress bar
- **Review (N) →** button always accessible in the top-right

### Drive Folder Browser

When using Google Drive, a folder browser lets you navigate your Drive hierarchy:

- Back button + breadcrumb trail
- Lists subfolders; shows image count for the current folder
- **Open This Folder** confirms selection and begins loading images

### Google Photos Album Picker

When using Google Photos:

- Lists all your albums
- **All Photos** tile loads your full library
- Click any album card to load its photos

### Review Screen

Accessible at any time via the **Review (N) →** button. Auto-opens when you reach the end of a folder/album.

- Grid of thumbnails for every selected photo
- Hover a thumbnail to reveal a red **✕** — click to remove from selection
- **Continue reviewing** (if unseen photos remain)
- **Save N photos** button

### Saving

**Local source:** creates a `selected photos` subfolder inside your source directory.

**Drive source:** creates a `selected photos` folder inside the same Drive folder you opened.

**Photos source:** creates a `selected photos` folder at the root of your Google Drive.

All saves also write a `filenames.txt` listing selected filenames (one per line). A progress bar tracks each file as it copies.

### Done Screen

- Confirms how many photos were saved and where
- **Back to selections** — return to review to adjust and re-save
- **Start over** — reset the app and open a new source

---

## Supported Image Formats

`jpg` · `jpeg` · `png` · `gif` · `webp` · `bmp` · `tif` · `tiff` · `avif` · `heic` · `svg`

---

## Output Structure

### Local / Google Drive

```
your-folder/
├── photo1.jpg
├── photo2.jpg
├── ...
└── selected photos/
    ├── photo1.jpg          ← copies of selected files
    ├── photo3.jpg
    └── filenames.txt       ← plain text list, one filename per line
```

### Google Photos

```
Google Drive (root)/
└── selected photos/
    ├── IMG_001.jpg
    ├── IMG_004.jpg
    └── filenames.txt
```

Originals are **never modified or deleted**. Files in `selected photos` are overwritten on re-save.

---

## Google OAuth Notes

- The OAuth Client ID is stored in your browser's `localStorage` — you only need to enter it once.
- The Client ID is not a secret (it is intentionally public in the browser OAuth flow).
- For Google Photos saves, the app uses the `drive.file` scope (limited to files the app creates), plus `photoslibrary.readonly`.
- For Google Drive, the app uses the full `drive` scope.
- Google Photos image URLs expire after approximately 60 minutes. If saving fails with an expiry error, start over to reload the photos.
- Disconnect revokes the access token immediately.

---

## File Structure

```
photo selecter/
├── index.html   — the entire app (HTML + CSS + JS, single file)
├── agents.md    — prompt spec used to build this app
└── README.md    — this file
```
