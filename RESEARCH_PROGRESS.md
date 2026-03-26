# Research Progress – Building Compliance Checker

> **For Claude:** Read this file at the start of every session and update it every time a task completes (agent finishes, file saved, data.js updated, etc.). This is the single source of truth for where we left off. Do not rely on conversation history.

---

## Goal
Replace all knowledge-based requirement data with verified data fetched from official legal sources (recht.nrw.de, gesetze-im-internet.de, EUR-Lex). Each building type must match the quality standard of `retirement_home` and `residential` in data.js — detail fields must contain actual quoted text from the law where possible.

## Quality Standard
See `retirement_home` entries in `data.js`. Key points:
- `detail` field contains **verbatim quoted text** from the law in quotation marks, followed by plain-language context
- `source_law` cites the exact paragraph (e.g. `WTG-DVO NRW § 7 Abs. 2`)
- `source_url` links to the official source where the text was fetched
- `last_verified` set to the date the agent fetched it

## Agent Instructions (use for every new building type)
1. Fetch the full text of each relevant law from the official source URLs listed below
2. Extract every requirement applicable to this building type in NRW
3. For each requirement, quote the relevant sentence(s) verbatim in the `detail` field, then add plain-language context
4. Note any requirements that could not be verified (e.g. DIN norms behind paywall) — mark them `"verified": false`
5. **Save output to `researched_<type_key>.json` BEFORE returning** — this is critical, do not skip
6. Update this file (`RESEARCH_PROGRESS.md`) after saving

## Key Source URLs
- BauO NRW 2018: https://recht.nrw.de/lrgv/gesetz/01012024-bauordnung-fuer-das-land-nordrhein-westfalen-landesbauordnung-2018-bauo-nrw/
- WTG NRW: https://recht.nrw.de/lrgv/gesetz/31012023-wohn-und-teilhabegesetz-wtg/
- WTG-DVO NRW: https://recht.nrw.de/lmi/owa/br_text_anzeigen?v_id=10000000000000000512
- WFNG NRW: https://recht.nrw.de/lrgv/gesetz/10022016-wohnraumfoerderungsgesetz-nordrhein-westfalen-wfng-nrw/
- GEG 2023: https://www.gesetze-im-internet.de/geg/
- BauNVO: https://www.gesetze-im-internet.de/baunvo/
- BauGB: https://www.gesetze-im-internet.de/bbaug/
- ArbStättV: https://www.gesetze-im-internet.de/arbst_ttv_2004/
- BImSchG: https://www.gesetze-im-internet.de/bimschg/
- SGB IX: https://www.gesetze-im-internet.de/sgb_9_2018/
- SGB XI: https://www.gesetze-im-internet.de/sgb_11/

## ID Prefixes per Building Type
| Type | ID Prefix |
|---|---|
| single_family_home | NRW-SFH |
| multi_family_home | NRW-MFH |
| student_housing | NRW-STH |
| social_housing | NRW-SOH |
| office_building | NRW-OFF |
| retail | NRW-RET |
| hotel | NRW-HOT |
| restaurant | NRW-GAT |
| mixed_use | NRW-MIX |
| assisted_living | NRW-AL |
| disability_care | NRW-DIS |
| childrens_home | NRW-CHH |
| kindergarten | NRW-KIN |
| warehouse | NRW-WAR |
| light_industrial | NRW-LI |
| factory | NRW-FAC |
| school | NRW-SCH |
| hospital | NRW-HOS |
| sports_facility | NRW-SPO |
| community_centre | NRW-COM |
| place_of_worship | NRW-POW |

---

## Status

### Already Verified (do not re-research)
- [x] `retirement_home` — original researched data, intact in data.js
- [x] `residential` — original researched data, intact in data.js
- [x] `disability_care` — fetched by agent, saved in care_social_data.json, intact in data.js
- [x] `childrens_home` — fetched by agent, saved in care_social_data.json, intact in data.js
- [x] `kindergarten` — fetched by agent, saved in care_social_data.json, intact in data.js

### Needs Research (knowledge-based, not verified)
- [x] `single_family_home` — verified 2026-03-26, 13 reqs, saved in researched_sfh.json
- [x] `multi_family_home` — verified 2026-03-26, 13 reqs, saved in researched_mfh.json
- [x] `student_housing` — verified 2026-03-26, 12 reqs, saved in researched_sth.json
- [x] `social_housing` — verified 2026-03-26, 15 reqs, saved in researched_soh.json
- [ ] `assisted_living` — **NEXT UP**
- [ ] `office_building`
- [ ] `retail`
- [ ] `hotel`
- [ ] `restaurant`
- [ ] `mixed_use`
- [ ] `warehouse`
- [ ] `light_industrial`
- [ ] `factory`
- [ ] `school`
- [ ] `hospital`
- [ ] `sports_facility`
- [ ] `community_centre`
- [ ] `place_of_worship`

---

## Workflow for Each Type
1. Launch research agent → saves `researched_<type>.json`
2. Review output (spot-check quoted text and paragraph numbers)
3. Run assembly script to inject into data.js
4. Push to GitHub
5. Mark as done in this file

## Last Updated
2026-03-26 — Residential group complete (SFH, MFH, STH, SOH). Session ended. Resume with assisted_living next.
