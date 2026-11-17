# HTML Doctor

A single-file **HTML editor for beginners** that shows the mistakes browsers
quietly forgive: a code editor with line numbers and syntax colouring on the
left, a live preview on the right, and real-time linting in between.

- **HTML checks** — duplicate singleton elements (`<html>`, `<body>`…),
  intersecting tags (`<b><i></b></i>`), wrong parents (`<td>` without `<tr>`),
  unknown and HTML5-only tags, invalid attributes and attribute values,
  missing closing tags, closing tags on void elements (`</br>`), unclosed
  quotes, content in the wrong place, unterminated `<title>`/`<style>`, stray `&`.
- **CSS checks** — unknown properties, invalid values, missing units, missing
  `:`/`;`/`}` and declarations outside a rule.
- Findings appear as wavy red/orange/blue underlines (error / warning / hint);
  hovering or clicking one opens a bottom pane with a plain-English
  explanation, the snippet and how to fix it. An ▲/▼ button collapses and
  expands the pane.

Scope is **HTML 4** (the H2 Computing 9569 syllabus); HTML5-only tags get a
warning, not an error. A missing `<!doctype html>` is an error (quirks mode).

The page opens pre-filled with a deliberately buggy example so there is
something to diagnose straight away. Correct examples:
[`?example=starter`](https://nyjc-computing.github.io/html-editor/?example=starter)
and
[`?example=table`](https://nyjc-computing.github.io/html-editor/?example=table).

No dependencies, no build step — the whole app is `index.html`. Developed as
part of [nyjc-computing/page-viewer](https://github.com/nyjc-computing/page-viewer)
(sources: `editor/editor.html` there).

**Live:** https://nyjc-computing.github.io/html-editor/
