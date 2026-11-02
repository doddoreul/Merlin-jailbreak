# Agent context - Merlin-jailbreak

## Project
- Desktop app: Tauri 2 + Angular 17.
- Purpose: edit Merlin playlist (`playlist.bin`), import audio/images, export to SD card.
- Important formats: MP3 stereo 128 kb/s, JPG 128x128.

## Implemented features
- YouTube import:
  - Rust command `fetch_youtube(url)` using `yt-dlp` + `ffmpeg`.
  - Downloads MP3 128 kb/s + JPG thumbnail into temp `youtube_import/{id}`.
  - Returns `AudioImportCandidate` with title, mp3 path, thumbnail data/path.
  - Multi-URL textarea in modal, per-file success/failure report (`V`/`X`).
  - Button becomes `Fini` after import and closes modal.
  - Uses multiple YouTube player clients to reduce "video not available".
  - Hides console windows during external tool execution.
  - Sanitizes YouTube titles (accents converted to ASCII).
- Automatic "Derniers ajouts":
  - Imported songs are duplicated into a protected `Derniers ajouts` folder.
  - Prompt on reopen to clear or keep this folder.
  - Folder cannot be deleted, renamed, or have its thumbnail changed.
- UI fixes:
  - Modal is viewport-fixed even after scrolling.
  - URL field disables browser recent suggestions.

## Build notes
- Frontend: `npm install`, `npm run build`.
- Backend: requires MSVC toolchain/linker; GNU/MinGW is not sufficient for the Tauri cdylib.
- Rust validation can be done with `cargo check`.
- External tools required in PATH: `yt-dlp`, `ffmpeg`.
- `/tools` is gitignored and contains local build scripts.

## Rules for the agent
- Never commit or push unless the user explicitly asks for it.
- Never assume that because the user asked once, the same action should be repeated automatically.
- Never compile or upload a release unless the user explicitly asks for it.
- Prefer minimal changes consistent with existing patterns.
- Keep validation/error handling at trust boundaries.