# Library, TTS, and History Improvements Implementation Plan

**Goal:** Split voice/style management into searchable paginated pages, expose IndexTTS generation settings, add keyword history search, and show every persisted style field in its detail view.

**Architecture:** Extend the existing FastAPI list endpoints with server-side `search`, `page`, `page_size`, and `total` metadata while keeping the existing `items` response compatible. Add three client-side App Router pages that reuse the same local API and persistence model; the main production page links to them and keeps only task-specific voice/style selection. Capture TTS settings in the existing config and use them when calling the current IndexTTS Gradio/FastAPI adapters.

**Tech Stack:** FastAPI, Python, Next/Vinext client components, TypeScript, existing JSON state files, unittest, and the existing frontend build test.

---

### Task 1: Extend backend list and TTS configuration APIs

**Files:**
- Modify: `webapp/server.py`
- Test: `tests/test_queue_resume.py`

Add validated TTS emotion and sampling fields to `DEFAULT_CONFIG`; pass them through the Gradio positional API and FastAPI emotion weight field. Extend `/api/voices`, `/api/styles`, and `/api/jobs` with case-insensitive search and bounded pagination while retaining `items`. Add tests for filtering, page metadata, and TTS argument forwarding.

### Task 2: Add standalone library and IndexTTS pages

**Files:**
- Create: `web/app/library-pages.tsx`
- Create: `web/app/voices/page.tsx`
- Create: `web/app/styles/page.tsx`
- Create: `web/app/tts/page.tsx`
- Modify: `web/app/page.tsx`
- Modify: `web/app/globals.css`

Build searchable paginated voice/style pages with CRUD actions. The style detail view displays id, name, aliases, type, description, recipe, image URL, timestamps, and preview image. Add a dedicated IndexTTS settings page for emotion mode/weight, emotion vectors, and advanced sampling values. Add links from the main header and replace management panels with page links.

### Task 3: Add searchable paginated generation history

**Files:**
- Modify: `web/app/page.tsx`
- Modify: `web/app/globals.css`

Add a keyword input, result count, previous/next controls, and page state to the history panel. Request filtered pages from `/api/jobs` and reset to page one when the keyword changes.

### Task 4: Verify and document behavior

**Files:**
- Modify: `README.md`

Document the three library/settings routes and run Python tests, frontend tests/build, Remotion type checking, and `git diff --check`.
