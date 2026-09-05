# PDF build

`SherlockSecrets_US_Label_Response.pdf` — 11pp A4, the sendable version of
`../response-to-labeling-partner.md`, with three annotated artwork plates.

## Rebuilding

```
/opt/pw-browsers/chromium --headless --disable-gpu --no-sandbox --hide-scrollbars \
  --virtual-time-budget=8000 --no-pdf-header-footer \
  --print-to-pdf=SherlockSecrets_US_Label_Response.pdf \
  file://$PWD/report-print.html
```

## Known limitation — the artwork rasters are not embedded

The three source JPGs could not be fetched into the build. `drive.google.com:443`
is refused by this environment's egress policy (HTTP 403 on CONNECT), and the
Drive connector can only return image bytes inline, which is far too large to
write to disk. Google Fonts is blocked by the same policy, so the document is
set in locally installed faces (DejaVu Serif display, Liberation Sans text,
DejaVu Sans Mono data).

Plates A, B and C therefore carry **compliance schematics** — carton faces drawn
to scale ratio, with the real ink specification, finishes, EAN, coding-window
dimensions and verbatim panel copy read from each file, and the US mandatory
elements pinned to where they sit or are absent. They are clearly labelled as
diagrams, not reproductions.

To embed the real artwork: drop the three JPGs into `artwork/` beside this file,
replace each `<svg>` in the `.plate-art` blocks with
`<img src="artwork/<filename>.jpg" alt="...">`, and rebuild. Keeping the legend
block underneath preserves the compliance annotation.
