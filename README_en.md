# Local Literature & Notes Manager (Single-file HTML)

A backend-free, offline-capable literature & notes manager in a **single HTML** file. Uses **Tailwind CSS** for styling and **Marked** for Markdown rendering. Data is persisted in the browser `localStorage`.

## Features

- **Literature**
  - Single add with relative path or URL.
  - **Drag-and-drop / Batch import** by selecting a **folder** (keeps sub-folder tags/folders).
  - Card actions: ⚙️ Manage (author, importance, reading status, tags), 🔗 Update link, 🗑 Delete.
  - **De-dup & path update (batch)**: if a **same-name file** exists, do **not** add; when the path differs, **update path only**.
- **Filtering & Search**
  - Sidebar tags (multi-select), folders, authors, importance, reading progress.
  - Top search: title / tags / author; one-click **Clear filters**.
- **Folders**
  - Filter by folder; ✎ rename, ➕ create, 🗑 delete (migrates items to “默认/Default”).
  - You can also add a folder inside the manage modal and it auto-checks for the current paper.
- **Inline Tag Editing**
  - Toggle “✎ Edit” on the sidebar to **rename / remove** tags directly on chips.
- **Notes**
  - Markdown notes, or **file-notes** (PDF, etc.).
  - “➕ Associate literature” to select items; finish with ✅ or `Enter`.
  - Note detail view supports iframe preview (file-notes) or rendered Markdown.
- **Persistence**
  - Auto-saved to `localStorage`.
  - **Export / Import** as JSON in the sidebar.

> Batch import default extensions: `.pdf` `.doc` `.docx` `.txt` `.md` (adjust in the whitelist inside `filesAdd()`).

## Quick Start

1. Put `sealist.html` at your workspace root (e.g., `学习/`).
2. Open it in a Chromium browser (Chrome / Edge) or via a local server.
3. Usage:
   - **Single add**: input `./relative/path.pdf` (or URL) → **Add**.
   - **Batch**: click **Drag/Batch** → drag files, or **Choose Folder** to import.
   - **Filter & search** with the sidebar + top search.
   - **Manage** via ⚙️ / 🔗 / 🗑 on each card.
   - **Notes**: add/edit and associate literature.

## Paths & Layout

- Keep `sealist.html` at the root and literature in sub-folders (e.g., `./21cm/Astro2020/...`) so **relative paths** work reliably.
- Re-importing a sub-folder:
  - Same-name files are **not duplicated**;
  - When the file moved, only the **path is refreshed**.

## Usage Highlights

- **Search**: `Enter` to trigger. Local search honors filters; global search ignores them.
- **Folders**: ✎ rename / ➕ create / 🗑 delete; deleting a folder migrates its items to “默认/Default”.
- **Inline tags**: enable “✎ Edit” to show remove/rename controls; blur or `Enter` to commit.
- **Update link (🔗)**: supports `./relative`, absolute (`file:///`, drive letters, or `/`), and `http(s)://` URLs.
- **Association mode**: click “➕ Associate literature”, multi-select (blue), then `Enter` or ✅ to finish.

## Data Model (exported JSON)

```json
{
  "literature": [
    {
      "id": "auto-id",
      "title": "foo.pdf",
      "path": "./21cm/Astro2020/foo.pdf",
      "folders": ["21cm", "Astro2020"],
      "tags": ["21cm", "Astro2020"],
      "author": "HERA",
      "importance": "3",
      "read": "已读"
    }
  ],
  "notes": [
    {
      "id": "note-id",
      "title": "Power Spectrum Notes",
      "type": "markdown",
      "markdown": "....",
      "litIds": ["auto-id"]
    },
    {
      "id": "note-id2",
      "title": "Some PDF",
      "type": "file",
      "path": "./notes/abc.pdf",
      "litIds": []
    }
  ],
  "folders": ["默认", "21cm", "Astro2020"]
}
```

## Compatibility & Tips

- **Browsers**: best on Chromium (Chrome/Edge). Folder selection uses `webkitdirectory`.
- **File access**: with `file://`, absolute/cross-drive paths may be restricted; prefer **relative paths** and a stable folder layout.
- **Storage limit**: `localStorage` is ~5MB; export regularly or migrate to IndexedDB.

## FAQ

1. **Why duplicates after batch import?**  
   Batch import deduplicates by **file name** and updates path only. If duplicates appear, they likely came from **manual single adds** (dedup key is “title + path”). Remove them or rely on batch import.

2. **Links don’t open / wrong paths?**  
   Check the relative position between `sealist.html` and your literature folders; prefer `./` relative paths; on Windows use forward slashes or `file:///D:/...`.

3. **“Invalid JSON structure” on import?**  
   Ensure top-level keys `literature`, `notes`, and `folders` exist and field types are correct.
