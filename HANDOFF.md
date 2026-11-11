# Algos & Capital (Historical Wealth Mind Map / "Wealth Web"): Handoff

Algos & Capital is an interactive "ecosystem of money": a network graph of 157 wealth builders, past and present, and the 348 companies, funds and institutions that connect them. You can see where their businesses overlap (hub and spoke: McDonald's and its suppliers, Motown and its offshoots, Drexel and KKR and Reginald Lewis) and how each business maps back to stock-market sectors (GICS sector ETF, then industry, then sub-industry). It is a personal research and learning tool for the owner (GitHub user AlxndrHQ). It gives Black wealth builders, historical and modern, their own grouping so they can be seen at a glance.

## Links

- Live site: https://alxndrhq.github.io/historical-wealth-mind-map/
- GitHub repo: https://github.com/AlxndrHQ/historical-wealth-mind-map (public, GitHub Pages from `main`, root folder)
- Sister site: PG County Real Estate Map, https://alxndrhq.github.io/pg-county-land-map/ (local folder `~/Developer/Real Estate Map`, which has its own HANDOFF.md and CLAUDE.md)
- Style reference: Semiconductor Earnings site, https://alxndrhq.github.io/SemiConductor-Earnings-Breakdown/

## Current state (as of 2026-11-10)

The live site is the 2026-10-05 build plus one fix. It has been checked against the code in `main`.

- **Data:** 157 people and 348 entities in `data.json` (505 records).
  - People by archetype: Black Wealth 48, Entrepreneurs 33, Tech 23, Business Operators 20, Capital Allocators 20, Intrapreneurs 13. "Other" exists in the page as an empty slot.
  - People by wealth tier: $1B+ 99, $100M–$500M 19, $500M–$1B 9, $5M–$100M 28, $500K–$5M 2 (Maggie Lena Walker, James Forten).
- **Tabs:** Map (the D3 network graph, `#/`), Directory (`#/grid`), Overlaps (`#/overlaps`). Profile dossiers open at `#/p/<id>`.
- **Features:**
  - **Dossier drawer:** each person's humble beginnings, breakthrough, shielding strategy, wealth note and sources.
  - **Filters:** Sector, Era (Today = living, Historical = deceased), Tier and Archetype, plus "Overlaps only".
  - **Filter visibility:** a "Clear filters ×" control, and a "Filtered to …: showing X of Y people" note on Overlaps and Directory. Company cards list the people the filters hide as "Outside current filters".
  - **Sector ring:** all 11 GICS sector ETFs (XLK, XLY, XLP, XLF, XLC, XLE, XLI and the rest), with industries around them. Sub-industries and company names appear as you zoom in.
  - **Theme:** light/dark toggle.
- **Working tree:** clean. HEAD is `77bf026` ("Ignore macOS .DS_Store files", 2026-11-10). `chat/` holds only a `.DS_Store`. The chat-app files that used to be there (index.html, places.json, map-tab.patch) are gone, presumably moved to the Black Business Directory project.
- **Reverted work:** a Map/Atlas geographic tab with a live "Black-Owned Index (MD/DC)" Google Sheet layer was pushed on 2026-10-15 (`14c8e3d`, `ed8a992`). It was then reverted (`bcff7ae`) because the owner is moving it to its own project, Black Business Directory. The code is still in git history.
- **Not in the data yet:** no Lerner family profile exists (checked `data.json`).

## How it's built

- **Stack:** one static page, plain HTML/CSS/JS plus D3 7.9.0 from cdnjs. No build step and no framework.
- **Files:**
  - `index.html` (about 23k lines). It holds all the CSS, the app code, and a full copy of the data between `/*DATA-START*/` and `/*DATA-END*/` (`const EMBEDDED_DATA`). The page fetches `data.json` first (`cache: 'no-store'`) and falls back to the embedded copy, so the preview still works when opened as a file. Key constants: `CATS` (archetypes and colors), `TIERS`, `GICS` (sub-industry to sector table), `SECTORS` and `SECTOR_RING`, `TYPE_TO_CAT`, `OVL_HOPS` (overlap chain depth).
  - `data.json`: an array of records.
    - **People** have `type` (entrepreneur/operator/investor/tech/historic/intrapreneur), `category` (archetype), `netWorthTier`, `tierBasis`, `wealthNote`, `humbleBeginnings`, `breakthrough(Detail)`, `shieldingStrategy/Detail`, `lifespan`, `base`, badges, `connections` and `sources`.
    - **Entities** have `type: "entity"` and a `gics: {sub, basis}` tag.
  - `.gitignore`: ignores `.DS_Store`.
- **Keeping the data in sync:** the embedded copy currently matches `data.json` exactly. Keep it that way: after editing `data.json`, re-embed it between the markers.
- **Run locally:** `python3 -m http.server 8765` in the folder, then open http://localhost:8765. Opening `index.html` directly also works, using the embedded data.
- **Deploy:** `git push` to `main`. GitHub Pages rebuilds in about a minute. If the old version still shows, check the live page with a cache-buster.
- **How data was added:** in past sessions, parallel research agents each wrote one "cluster" file (for example QSR/McDonald's, music, duty-free, software, AI, finance, Black Wealth A/B). A merge-and-validate script checked for dangling connection IDs, duplicate entity IDs and unknown GICS sub-industries before merging. That script lived in a temporary scratchpad and is **not in the repo**.

## Decisions and conventions

- **Name:** the working title is **Algos & Capital**. It was earlier "Quiet Capital"; the repo name stays `historical-wealth-mind-map`. The owner also calls it the "Wealth Web".
- **Style:** match the Semiconductor Earnings site.
  - Light "paper" palette: #FAFAF7 background, #1F1D18 text, #8A5A2B accent. Dark "ink" palette: #0E0E10 background, #E8E6DF text, #C9A875 accent.
  - Fonts: Iowan Old Style serif for names and headings, JetBrains Mono uppercase letter-spaced labels, system sans for body text.
  - Hairline rules, companies drawn as hairline boxes, no glows or gradients.
  - The owner wants to keep moving toward Apple / Steve Jobs-style essentialist minimalism, so prefer removing elements over adding them. (Reason: the owner said the restyle looked "1 million times better" and wants one identity across their sites.)
- **Black Wealth:** every Black profile, historical or modern, goes under the single archetype "Black Wealth". This includes Black intrapreneurs such as Ursula Burns. (Reason: the owner wants to see them all at a glance.)
- **Archetypes:** Business Operators, Capital Allocators, Entrepreneurs, Tech (renamed from "Tech Compounders"), Intrapreneurs, Black Wealth, and Other (for anyone who fits nothing else). The old "Historical Visionaries" group was folded into these.
- **Intrapreneurs:** hired executives with outsized impact (Tim Cook, Lisa Su, Satya Nadella, Jamie Dimon and others), added at the owner's request on 2026-10-05. This came after an earlier choice to add only founders and owner-operators from the Fortune 500, plus today's AI leaders.
- **Wealth floors:** $5M peak wealth in general. $500K for Black historical figures only, in the $500K–$5M tier. (Reason: "historically it's much harder" for them to have built recorded wealth.)
- **Hub and spoke:** link real, documented business ties (anchor customers, suppliers, acquirers, board seats, deals) so hub-and-spoke structures appear. Prefer real links over thematic "parallel" edges. (Reason: the point of the map is seeing where businesses connect.)
- **GICS tagging:**
  - Every entity gets `gics: {sub, basis}`. `basis: "official"` only for listed companies whose classification was verified (the S&P 500 list on Wikipedia). Otherwise use "inferred". Universities, foundations and civic groups use the sub-industry "Non-market".
  - People inherit the sector of their main business; the page computes this.
  - Sharing a sector does **not** count as an overlap.
  - Any new sub-industry must be added to the `GICS` table in `index.html`.
- **Sources:** every person should have two or more sources. Mark low-confidence wealth estimates honestly. Celebrity Net Worth was flagged as a weak source.
- **Privacy:** the repo is public. Before pushing, scan for keys, emails and local paths. Never send the owner's personal details to outside services. A research agent once put the owner's email in a request header to Wikipedia, which led to this rule.
- **Undoing changes:** use `git revert`, not history rewrites (the 2026-10-15 rollback was done this way).

## Open items (most important first)

1. **Link back to the PG County Real Estate Map.** The owner wants profiles whose families hold land in Prince George's County to link to https://alxndrhq.github.io/pg-county-land-map/, for example with a "Land in PG County" link. Start with the Lerner family (Lerner Enterprises, former Landover Mall site). No Lerner profile exists yet, so one must be researched and added first ($5M floor, two or more sources, GICS-tagged entities). The Real Estate Map's handoff also plans to link back to Wealth Web profiles (`#/p/<id>`), so agree on profile IDs in both directions.
2. **Consider a "Today" line.** The owner is considering a short, sourced "Today" line on profiles of present-day families.
3. **Check whether the Chuck Feeney DFS date was fixed.** In 2026-10-05 the profile said 1982 and Wikipedia says 1984. The chat said it would be checked; whether it was is not recorded.
4. **Replace weak wealth sources** (Celebrity Net Worth) on music and other profiles where better sources exist.
5. **Grow the sector tree.** The owner's long-term vision is for every hub (sector, industry, sub-industry) to show its people and how their businesses tie to each other, all mapping back to the stock market. Today only 11 sector ETFs are shown; the owner mentioned "12", but GICS has 11.
6. **Fill the empty "Other" archetype,** or remove it if it stays unused.
7. **Restore the merge/validate tooling.** It is not in the repo. Adding it would make future data batches safer.
8. **Question:** should the reverted Map/Black-Owned Index code stay only in the Black Business Directory project, or should Algos & Capital later get its own geographic view? The chat only says it is "moving to its own project".

## Session log

- **2026-10-05.**
  - Created the GitHub repo and turned on Pages.
  - Grew the map from 7 to 70 people and added the Overlaps tab, then the GICS sector layer.
  - Restyled to the paper/ink theme with a light/dark toggle, and renamed the site to Algos & Capital.
  - Restructured the archetypes (Black Wealth and the others), then added the Era filter, Fortune 500 ranks, founders, AI leaders and Intrapreneurs (114, then 156 people).
  - Added visible filter notes and Steve Jobs (157 people), and fixed hub cards.
- **2026-10-15.** Applied the Claude chat app's patch (Atlas/Map tab with the live Black-Owned Index, and the Web/Map tab rename) and pushed it. Then reverted it at the owner's request, to split it into the Black Business Directory project.
- **2026-11-10.**
  - The owner asked about a Prince George's County land-ownership map (Lerner/Landover Mall, Bowie Town Center, the stadium, nearby senior housing).
  - Claude confirmed it was feasible, wrote its handoff, and moved it to `Real Estate Map/HANDOFF.md`.
  - A `.gitignore` for `.DS_Store` was committed the same day (`77bf026`).
