---
name: resume-export-to-pdf
description: Convert index.html to resume.pdf using Chromium headless. Produces a single-page PDF preserving the full visual style including dark theme and colors.
argument-hint: ""
---

# Resume Export to PDF

Convert `/home/morrisseymarr/Playground/mochdeden.com/index.html` to `resume.pdf` in the same directory using Chromium headless.

## Behavior

1. Inject compact print CSS (reduced line-height, margins, padding) into a temp HTML
2. Use `--dump-dom` + inline JS to measure the actual rendered content height
3. Generate the final PDF with `@page` set to the exact measured height — no blank space at the bottom
4. Clean up temp files
5. Report the file size to confirm success

## Command

```bash
python3 - << 'PYEOF'
import subprocess, tempfile, os, re

HTML = '/home/morrisseymarr/Playground/mochdeden.com/index.html'
PDF  = '/home/morrisseymarr/Playground/mochdeden.com/resume.pdf'

COMPACT_CSS = """<style id="print-compact">
body { padding: 12px 12px 0 12px; line-height: 1.3; }
.container { padding: 18px 22px 0 22px; }
section { margin: 0; padding: 0; }
.title { margin-bottom: 0.4em; }
.contact-info { margin-bottom: 0.6em; padding-bottom: 0.6em; }
.contact-info p { margin: 0.1em 0; }
h2 { margin-top: 0.3em; margin-bottom: 0.2em; }
h3 { margin-top: 0.3em; margin-bottom: 0.1em; }
ul { margin: 0.15em 0 0.2em 1.8em; }
li { margin: 0.05em 0; }
hr { margin: 0.3em 0; }
p { margin: 0.1em 0; }
.job-meta { margin-bottom: 0.15em; }
.company-link { margin-bottom: 0.1em; }
article { margin: 0; }
</style>"""

MEASURE_JS = """<script>
document.addEventListener('DOMContentLoaded', function() {
    var h = Math.max(document.body.scrollHeight, document.documentElement.scrollHeight);
    document.querySelector('meta[name="render-height"]').setAttribute('content', h);
});
</script>
<meta name="render-height" content="0">"""

with open(HTML) as f:
    html = f.read()

html = re.sub(r'@page\s*\{[^}]*\}', '@page { size: 8.5in 100in; margin: 0; }', html, flags=re.DOTALL)
html = html.replace('</head>', COMPACT_CSS + MEASURE_JS + '</head>')

with tempfile.NamedTemporaryFile(mode='w', suffix='.html', delete=False, dir='/tmp') as f:
    f.write(html); tmp1 = f.name

result = subprocess.run(['chromium', '--headless', '--disable-gpu',
    '--dump-dom', '--virtual-time-budget=5000',
    f'file://{tmp1}'], capture_output=True, text=True)
os.unlink(tmp1)

m = re.search(r'name="render-height"\s+content="(\d+)"', result.stdout)
height_pts = (int(m.group(1)) + 5) * 0.75  # screen px -> PDF pts (96dpi -> 72dpi) + 5px buffer

with open(HTML) as f:
    html2 = f.read()

html2 = re.sub(r'@page\s*\{[^}]*\}', f'@page {{ size: 612pt {height_pts:.0f}pt; margin: 0; }}', html2, flags=re.DOTALL)
html2 = html2.replace('</head>', COMPACT_CSS + '</head>')

with tempfile.NamedTemporaryFile(mode='w', suffix='.html', delete=False, dir='/tmp') as f:
    f.write(html2); tmp2 = f.name

subprocess.run(['chromium', '--headless', '--disable-gpu',
    f'--print-to-pdf={PDF}', '--print-to-pdf-no-headers', '--no-margins',
    '--run-all-compositor-stages-before-draw', '--virtual-time-budget=5000',
    f'file://{tmp2}'], check=True)
os.unlink(tmp2)
PYEOF
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
