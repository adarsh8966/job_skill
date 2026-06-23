# ATS Formatting Rules

## What ATS actually does

Applicant Tracking Systems parse your resume into structured fields (name, contact, work
history, education, skills), then score it against the job description by keyword matching.
A beautiful two-column resume template can score 0% if the parser skips the right column.

## Layout rules

- **Single column only.** Two-column layouts and sidebars — the right column is often
  completely ignored by parsers.
- **No text boxes.** Text inside Word text boxes is invisible to most ATS parsers.
- **No tables for layout.** Tables used to simulate two columns = same problem as text boxes.
  Tables are fine for actual tabular data (rare on a resume).
- **No headers/footers for important content.** Name, phone, email belong in the main
  document body — many parsers skip the page header/footer entirely.
- **No graphics, icons, or images.** Profile photos, skill bar charts, decorative icons —
  all invisible to parsers.

## Font and formatting rules

- **Standard fonts only:** Calibri, Arial, Times New Roman, Garamond, Georgia.
  Custom fonts may not render on the ATS server and produce garbled text.
- **Font size:** 10-12pt body, 14-16pt name, 11-12pt section headers.
- **Avoid excessive bold/italic** in the middle of bullet text — it can confuse text
  extraction.

## Section header names

ATS systems look for specific strings to identify sections. Use these exactly:
- `Summary` or `Professional Summary` — NOT "About Me", "Profile", "Who I Am"
- `Experience` or `Work Experience` — NOT "Where I've Worked", "Career History"
- `Education` — this one is usually fine as-is
- `Skills` or `Technical Skills` — NOT "Things I Know", "Toolbox"
- `Projects` or `Personal Projects` — NOT "Side Work", "Portfolio"

Non-standard headers cause the section to be misfiled or skipped entirely.

## File format

- **DOCX is safest** for maximum ATS compatibility. PDF is increasingly supported but
  some legacy ATS still parse it poorly.
- When given the choice, submit DOCX. When the application explicitly asks for PDF, use PDF.
- Never submit .pages, .odt, or Google Docs links.

## Keyword matching rules

- **Use exact phrases.** "React.js" and "React" are different strings. "CI/CD pipelines"
  and "continuous integration" may not match. Mirror the JD's exact phrasing.
- **Don't keyword-stuff.** Listing a skill 10 times doesn't help and looks suspicious.
  Keywords in context (inside a bullet) are weighted higher than bare skills list entries.
- **Expand abbreviations once.** Write "Machine Learning (ML)" the first time if the JD
  uses both forms.

## Common mistakes that fail ATS

1. **Canva / Zety / Novoresume templates** — almost all use text boxes and multi-column
   layouts. Pretty but invisible to parsers. Use plain Word or Google Docs instead.
2. **"Summer 2022" as a date** — doesn't parse. Use "Jun 2022" or "06/2022".
3. **Objective statement** (outdated) — some ATS parse it as the first job entry, which
   corrupts the entire work history section.
4. **Key contact info in page headers** — name and email in the document header are
   skipped. Keep them in the body.
5. **Tables for the skills section** — a two-column skills table looks clean but ATS
   reads it as a table, not a list, and may misparse every entry.
