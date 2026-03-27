---
name: resume-export-to-pdf
description: Convert index.html to resume.pdf using Chromium headless. Produces a single-page PDF preserving the full visual style including dark theme and colors.
argument-hint: ""
---

# Resume Export to PDF

Convert `/home/morrisseymarr/Playground/mochdeden.com/index.html` to `resume.pdf` in the same directory using Chromium headless.

## Behavior

1. Run Chromium in headless mode with `--no-margins` and `--print-to-pdf-no-headers`
2. Use `@page { size: auto; margin: 0; }` to fit all content on a single page
3. Save the output as `resume.pdf` in the project root
4. Report the file size to confirm success

## Command

```bash
chromium --headless --disable-gpu \
  --print-to-pdf=/home/morrisseymarr/Playground/mochdeden.com/resume.pdf \
  --print-to-pdf-no-headers \
  --no-margins \
  --run-all-compositor-stages-before-draw \
  --virtual-time-budget=5000 \
  "file:///home/morrisseymarr/Playground/mochdeden.com/index.html"
```

Run this command exactly as written. After it completes, run:

```bash
ls -lh /home/morrisseymarr/Playground/mochdeden.com/resume.pdf
```

Report the file size to confirm the PDF was generated successfully.

## Requirements

Chromium must be installed. If the command fails with "command not found", tell the user to install it:

```bash
sudo pacman -S chromium
```

## Arguments

$ARGUMENTS
