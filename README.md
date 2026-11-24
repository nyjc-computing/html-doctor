# HTML Doctor

A single-file **HTML editor for beginners** that shows the mistakes browsers
quietly forgive: a code editor with line numbers and syntax colouring on the
left, a live preview on the right, and real-time linting in between.

- **HTML checks** — duplicate singleton elements (`<html>`, `<body>`…),
  intersecting tags (`<b><i></b></i>`), wrong parents (`<td>` without `<tr>`),
  unknown and HTML5-only tags, invalid attributes and attribute values,
  missing closing tags, closing tags on void elements (`</br>`), unclosed
  quotes, content in the wrong place, unterminated `<title>`/`<style>`, stray `&`.
  Only HTML is checked for now — CSS checking arrives in a separate lesson.
- If the preview would show nothing at all, a placeholder explains why instead
  of leaving a blank frame.
- Findings appear as wavy red/orange/blue underlines (error / warning / hint);
  putting the cursor on one — or picking it from the list below — opens a
  bottom pane with a plain-English explanation, the snippet and how to fix it.
  An ▲/▼ button collapses and expands the pane.

Scope is **HTML 4** (the H2 Computing 9569 syllabus); HTML5-only tags get a
warning, not an error. A missing `<!doctype html>` is an error (quirks mode).

The page opens pre-filled with a deliberately buggy example so there is
something to diagnose straight away. Correct examples:
[`?example=starter`](https://nyjc-computing.github.io/html-doctor/?example=starter)
and
[`?example=table`](https://nyjc-computing.github.io/html-doctor/?example=table).

**Share link** copies an address that opens the current page in the editor:
`#html=<byte count>.<base64url of the page>`. The page travels inside the link
itself — nothing is uploaded, and because it sits after the `#` no server sees
it (GitHub Pages rejects query strings over about 8 KB). base64 grows text by a
third, so pages up to **12,000 characters** fit in a 16 KB link; the character
count appears in the editor's title bar only once a page is over that limit.

**Classroom embedding:** the app is a single file, so it can be published to
the campus-classroom app host and embedded in a document question. Verified
against the campus-apphost contract: the injected export runtime lands after
the app's own script (the file's last `</body>` is the real one), share links
keep pointing at this site (the embed URL is token-gated and expires), drafts
are namespaced per question, and an export request swaps the student's HTML
into the snapshot so a submission contains the code, not a picture of the
editor.

The preview is a **sandboxed `<iframe>`** (no `allow-same-origin`, no
`allow-modals`) in which **JavaScript is switched off**. Each render injects a
content security policy with a fresh random nonce that allows only the Doctor's
own blank-preview probe, so the page's `<script>` blocks, inline handlers and
`javascript:` links never run, and `alert`/`confirm`/`prompt` are unavailable.
HTML and CSS render exactly as a browser would show them. A page opened from a
shared link says so above the editor, because its preview then shows HTML
written by someone else.

No dependencies, no build step — the whole app is `index.html`. Developed as
part of [nyjc-computing/html-inspector](https://github.com/nyjc-computing/html-inspector)
(sources: `editor/editor.html` there).

**Live:** https://nyjc-computing.github.io/html-doctor/

## Testing

Handy for e2e tests (and the browser console):

- `window.htmlDoctor` — `setCode(html)` types on your behalf (lint, preview and
  autosave all react), `getFindings()` returns the findings as plain objects
  (severity, code, title, line, column), `shareLink(html?)` builds a share
  link, `previewState()` reports `{ stale, blank, fromSharedLink }`.
- `<html data-ready="1">` appears when boot is done; `#pv-note[data-stale]` is
  present from an edit until the preview has re-rendered (400 ms debounce);
  `#pv-empty[hidden]` says whether the preview shows anything.
- The preview iframe is opaque-origin and runs no JavaScript, so it cannot be
  scripted or queried from outside. To assert what happens inside a page, put
  instrumentation in the page itself (`parent.postMessage(...)`) and listen
  for `message` events on the host — and never use `alert`/`confirm`/`prompt`
  in test pages.
- Storage: `editor.code.v3` holds the draft, `editor.split` the divider
  position. `?example=starter|table` loads a built-in example; a `#html=…`
  share link overrides both until the page is edited.
