# Professional-survey cleaning and provenance exclusions

This file records the deterministic row-level exclusion rule used to obtain the professional-survey analysis sample.

## Cleaning sequence

1. The locally preserved pre-deduplication source contains **230 submissions**.
2. A previously identified block of **25 answer-identical records** (with different timestamps) is removed in full.
3. The resulting **205 non-duplicate submissions** correspond to the public professional-survey release.
4. Q39 (response provenance) is then used to define the manuscript's professional analysis sample.
5. Exactly **five** of the 205 public records are provenance-ineligible, leaving **N = 200** eligible human professional responses.

This sequence is important: `230 - 25 = 205`, followed by `205 - 5 = 200`.

## Excluded public records

Row numbers below refer to the public 205-response workbook. “Spreadsheet row” counts the header as row 1.

| Data row | Spreadsheet row | Q39 response | Reason |
|---:|---:|---|---|
| 14 | 15 | I prefer not to answer | Response provenance withheld |
| 25 | 26 | No, I am an AI system answering without role-playing a human | Explicit AI-system response |
| 72 | 73 | No, I am an AI system answering without role-playing a human | Explicit AI-system response |
| 73 | 74 | Yes, but an AI system generated or substantially rewrote some of my answers | Human response substantially generated/rewritten by AI |
| 159 | 160 | I prefer not to answer | Response provenance withheld |

The eligible Q39 response is:

> Yes, and I answered from my own professional experience

No additional respondent-level exclusion is introduced in this file.

The five ineligible records remain in the public archive for transparency and are excluded only at analysis time.
