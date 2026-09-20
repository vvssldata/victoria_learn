# Study Trail — Project Notes

Live app: https://vvssldata.github.io/victoria_learn/
Repo: https://github.com/vvssldata/victoria_learn
Progress log (Google Sheet): https://docs.google.com/spreadsheets/d/120T9AJJTgE9pzX21jtseQQj1PO733ubD_MxHDZVJ86o/edit

Victoria's (6th grade) study app: a single self-contained `index.html` with two
subjects — **Language Arts** (Vocabulary, Reading Comprehension, Writing) and
**Pre-Algebra** — organized as a "trail" of topics. Each topic has an untimed
**Practice** quiz and a scored **Assessment** quiz (need 80%+ to master and
unlock the next topic). Progress is stored in the browser's `localStorage`
(no login), plus a best-effort backup of each completed quiz to the Google
Sheet above via a Google Apps Script webhook.

## Curriculum source material

Pre-Algebra content is pulled from the Kumon math worksheets in
`H:\my drive\kids_education_lower school\Math\Kumon\` (scanned PDFs, Levels
B–K, no extractable text — sampled visually page by page).

Kumon Level **G** matches 6th-grade pre-algebra best (it's Kumon's own
pre-algebra level: integer/negative-number operations, exponents, one-variable
equations). Level F/E are fraction/ratio review, already covered by the
original topics below.

## Pre-Algebra topics (11 total)

Original 8:
1. Ratios & Rates
2. Unit Rates & Proportions
3. Fractions & Decimals
4. Negative Numbers & Absolute Value
5. Expressions
6. Equations & Inequalities
7. Geometry: Area, Surface Area & Volume
8. Statistics: Center & Data

Added 2026-09-20, sourced from Kumon Level G (converted from Kumon's
fill-in-the-blank format to this app's multiple-choice format; every answer
hand-verified):
9. **Multiplying & Dividing Negative Numbers** (Kumon G61–G63)
10. **Exponents & Powers of Negative Numbers** (Kumon G65)
11. **Writing & Solving Equations from Words** (Kumon G191–G192)

New topics were appended to the *end* of the list (not inserted in the
middle) so existing progress — which is keyed by topic position — didn't
get scrambled.

To add more later: sample further Kumon G pages (or Level F for
ratio/fraction depth) with a PDF page-image read, convert a handful of real
problems per concept to multiple-choice with computed answers + plausible
distractors, and append new topic objects to `CURRICULUM.math.areas.prealg.topics`
in `index.html`.

## Progress-seeding fix (2026-09-20)

The app never actually reads the Google Sheet — it only writes to it as a
backup log, and progress that drives unlocking/mastery lives only in
`localStorage` on whichever device was used. A new/cleared browser would
show everything locked even though Victoria had already mastered most
topics.

Fix: reconstructed each topic's mastery status from the sheet's activity
log and baked it into the app as `SHEET_PROGRESS_SEED`, a snapshot as of
2026-09-20. `loadLocal()` now uses this seed **only when localStorage is
completely empty** (first load on a device) — it never overwrites existing
local progress.

Four topics had no "Mastered" row in the sheet (a save button/step was
missed that day), but topics later in the same unlock chain were reached
the same day, which is only possible if these were mastered first —
confirmed by the user:
- Ratios & Rates
- Unit Rates & Proportions
- Fractions & Decimals
- Synonyms & Antonyms

As of this snapshot, 25 of 28 topics are mastered (all of Language Arts;
8 original Pre-Algebra topics). The 3 new Kumon-sourced Pre-Algebra topics
and the "Writing Task: Take a Stand" writing exercise are not yet attempted.

## Commit history for this work

- `261fc72` Add Study Trail practice app
- `902038a` Auto-save results to Google Sheet via Apps Script
- `650746b` Point Progress Sheet link at the actual live sheet
- `24dfe95` Add pre-algebra topics sourced from Kumon Level G
- `6dedccf` Label new Pre-Algebra topics with their Kumon source pages
- `734a9f7` Seed first-load progress from the Google Sheet activity log
