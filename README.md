<p align="center">
  <img src="https://i.ibb.co/v4LPWknc/ledger.png" alt="Ledger" />
</p>

# Ledger — PDF to Markdown / JSON Conversion

A local-only, browser-based tool that converts Australian utility bills, payslips, and council rates notices into clean Markdown notes and/or JSON files with structured YAML front-matter — ready to interrogate with AI. Also converts general PDF files to Markdown, which saves token usage when seeking AI assistance.

**Nothing is uploaded. Everything runs in your browser.**

---

## What it does

Drop a PDF onto the page and Ledger extracts the key financial fields into a labelled front-matter header, with the full document text below. It outputs two formats:

- **Markdown (`.md`)** — for Obsidian, Apple Notes, or any Markdown-based workflow
- **JSON (`.json`)** — structured for direct import into notes apps that store content in a database, with a compact summary line positioned so AI assistants can read all key figures within their per-note preview limit.
- **Ledger** handles the extraction step only — AI querying happens with your AI assistant of choice, using the structured output as its source material.

---

## Supported document types

Auto-detected by keyword scoring, or set manually:

| Type | Extracted fields |
|---|---|
| **Utility bill** | Provider, account, meter ID (NMI/MIRN/serial), period, usage with unit (kWh/MJ/kL), avg daily usage, amount due, due date |
| **Payslip** | Employer, pay period, pay date, gross, tax, super guarantee, salary sacrifice super, net, YTD gross |
| **Rates notice** | Council, assessment number, financial year, capital improved value, rateable value, amount due, due date |
| **General document** | Date, amount |

Detection is tuned for Australian providers, councils, and terminology (NMI, PAYG, SG, kilolitre, etc.).

---

## How to use

1. Save `doc-to-markdown.html` to your machine
2. Open it in any modern browser — no server, no install
3. Drop or choose a PDF
4. Review and correct the extracted fields
5. Download `.md`, `.json`, or copy the Markdown to clipboard

Keep a copy on each machine you use. There is no hosted version — though if you host `doc-to-markdown.html` yourself over http/https (with `sw.js` alongside it), your browser will offer to install it as an app with offline support.

---

## Features

- **100% local** — PDF.js runs entirely in-browser; no file ever leaves your machine
- **Auto-detection** — weighted keyword scoring identifies document type
- **Layout-preserving extraction** — optional column-spacing mode helps AI models read tables correctly
- **Heading detection** — font size metadata promotes section headers to Markdown `##`
- **Text normalisation** — ligatures (`ﬁ→fi`), smart quotes, en/em dashes, non-breaking spaces cleaned automatically
- **OCR fallback** — if a PDF has no text layer (scanned or photographed), Tesseract.js is loaded on demand and run over each page automatically; results are flagged with an "OCR'd" badge since accuracy is lower than a native text layer
- **Editable front-matter** — review and correct any field before export; add custom fields
- **JSON export** — summary line placed at the top of note content so AI assistants see all key figures within their context preview window
- **Installable** — if you host the file over http/https (rather than opening it as a local file), it's an installable PWA with offline support after first load

---

## Modifications from LiteDoc

This tool began as a fork of [LiteDoc](https://github.com/0xovo/LiteDoc) by [0xovo](https://github.com/0xovo) and has been substantially modified:

- **Domain-specific extraction** — heuristic field extraction for Australian utility bills, payslips, and council rates notices, with weighted keyword detection and regex patterns tuned to local providers and terminology
- **JSON export** — structured output for notes apps with a compact summary line optimised for AI context windows
- **Material Design 3 UI** — restyled with Inter + JetBrains Mono, MD3 tonal colour system, pill buttons, underline tabs, and tonal surface hierarchy; warm editorial aesthetic replaced with a clean productivity-tool look
- **Font size–based heading detection** — items significantly above the page median font size are promoted to `##` headings in flow output
- **Improved column spacing** — `lastEnd` estimate scales by actual item font size rather than a fixed constant
- **Text normalisation pass** — ligature and encoding artefact cleanup applied to every extracted text item
- **Scanned PDF detection** — warns when extracted text is near-empty
- **Separate super fields** — super guarantee and salary sacrifice are extracted into distinct fields with correctly prioritised label matching
- **OCR fallback** — adapted from LiteDoc's Tesseract.js integration, simplified to a single sequential worker (LiteDoc runs a multi-worker pool for its batch/multi-file use case)
- **Installable PWA shell** — adapted from LiteDoc's manifest + service worker; the manifest is inlined as a data URI in `pdf-markdown.html` (no separate file needed for that part), but `sw.js` must ship as a real same-origin file — browsers don't allow registering a service worker from a `blob:`/data URI

---

## Dependencies (all CDN, no install)

- [PDF.js](https://mozilla.github.io/pdf.js/) — PDF parsing (Mozilla, Apache 2.0)
- [Tesseract.js](https://tesseract.projectnaptha.com/) — OCR, loaded only when a scanned PDF is detected (Apache 2.0)
- [Inter](https://rsms.me/inter/) — UI font (SIL Open Font Licence)
- [JetBrains Mono](https://www.jetbrains.com/legalforms/fonts/) — monospace font (SIL Open Font Licence)

---

## Notes

- Field extraction is heuristic — always review the front-matter before trusting numbers
- Scanned (image-only) PDFs are read via on-demand OCR, which needs network access on first use and is noticeably slower and less accurate than a native text layer — double-check the fields
- Tuned for Australian documents; results will vary for non-AU formats
- The original PDF remains your legal record — the extracted note is a working copy for search and AI queries
