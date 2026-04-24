# `data/antwerp-cycling-cafes` — README

Standalone dataset of cycling-friendly cafes, bars and koffiebars in the **province of Antwerp** (Belgium). Compiled 2026-04-24. Not wired into the React/DatoCMS portfolio app — these files are a self-contained reference asset.

## Files

| File | Format | Purpose |
|---|---|---|
| `antwerp-cycling-cafes.json` | JSON array (40 entries) | Structured machine-readable dataset |
| `antwerp-cycling-cafes.md` | Markdown | Human-readable summary table + per-cafe notes |
| `README.md` | Markdown | This file: schema, sources, methodology, caveats |

## Schema (per JSON entry)

```jsonc
{
  "rank": 1,                            // Integer, 1..N. See ranking method below.
  "name": "Vitesse Coffee & Cycling",
  "address": {
    "street": "Provinciestraat 82/1",   // String or null
    "postal_code": "2018",              // BE postcode
    "city": "Antwerpen",                // Deelgemeente / village
    "municipality": "Antwerpen",        // Gemeente
    "arrondissement": "Antwerpen",      // Antwerpen | Mechelen | Turnhout
    "province": "Antwerpen",            // Always "Antwerpen" in this dataset
    "country": "BE"
  },
  "coords": null,                       // {lat, lon} or null. All null in this dataset (no reliable WebFetch).
  "contact": {
    "website": "https://...",
    "phone": "+32 ...",
    "email": "info@...",
    "instagram": "@handle"
  },
  "opening_hours": {
    "mon": "10:00-17:00" | null,        // 24h "HH:MM-HH:MM" or null=closed/unknown
    "tue": "...",
    "wed": "...",
    "thu": "...",
    "fri": "...",
    "sat": "...",
    "sun": "...",
    "notes": "Free-text caveat (seasonal, popups, etc.)"
  },
  "cycling": {
    "labeled_fietscafe_province": null, // bool or null. True = on Provincie Antwerpen "Label Fietscafe" list.
    "bike_rack_outdoor": null,          // bool or null
    "covered_or_indoor_bike_storage": null,
    "repair_tools_or_floor_pump": null,
    "water_refill": null,
    "ebike_charging": null,
    "changing_area_or_shower": null,
    "terrace": null,
    "wifi": null,
    "near_fietsknooppunt": "30" | null, // Cycle-node number if at/near one
    "nearby_named_routes": ["Kempen", ...],
    "typical_cyclist_crowd": ["road", "gravel", "recreational", "touring", "mountain", "urban", "commuter", "family", "ebike"]
  },
  "popularity": {
    "google_rating": null,              // 1.0-5.0 (all null — see caveats)
    "google_review_count": null,
    "cited_in": ["wielercafes.nl", "kempen.be", ...]   // Domains that mention this cafe
  },
  "sources": ["https://...", "https://..."],   // URLs consulted for this entry
  "last_verified": "2026-04-24",        // ISO date of the snippet/page that fed this entry
  "notes": "Free-text summary."
}
```

**Convention:** `null` = unknown or unverifiable from available sources. **Never `false` as a default.** A `null` for `bike_rack_outdoor` means "we couldn't confirm either way", not "no rack".

## Methodology

### Sources consulted
- **Provincie Antwerpen — Label Fietscafé/Fietsresto/Fietslogies** ([provincieantwerpen.be](https://www.provincieantwerpen.be/aanbod/dvt/toerisme-provincie-antwerpen/routes/label-fietscafe-fietsresto-fietslogies.html)) — official curation
- **Kempen.be — "Fietsers Welkom!"** ([kempen.be/fietsers-welkom](https://www.kempen.be/fietsers-welkom))
- **Visit Antwerpen — 5 leuke wielercafés** ([visit.antwerpen.be](https://visit.antwerpen.be/leuke-wielercafes-in-antwerpen))
- **Vlaamse Wielrijdersbond — Fietsvriendelijke cafés** ([vwb.be](https://www.vwb.be/125/Fietsvriendelijke-caf%C3%A9s-en-B&B-s))
- **Wielercafes.nl** ([wielercafes.nl](https://wielercafes.nl/)) — NL+BE aggregator
- **Cycling in Flanders — Food & Drink** ([cyclinginflanders.cc](https://www.cyclinginflanders.cc/spots/food-drink))
- **Cycling blogs:** coiscycling.com, cyclis.be, routen.be, cyclingdestination.cc, granfondo-cycling.com, belgiansmaak.com, grinta.be, wielerverhaal.com
- **Address/hours aggregators:** openingsuren.vlaanderen, openingsurengids.be, alle-openingstijden.be, restaurantguru.com, opcafegaan.be, handelsgids.be, goudengids.be, foursquare.com, tripadvisor.com
- **Each cafe's own website** when extractable from search snippets

### Ranking method
1. Citation count across cycling-community sources (each named source = 1 point)
2. +0.5 if listed on Provincie Antwerpen or Kempen.be official "Fietsers Welkom" curation
3. Brand/recognition tiebreaker for iconic spots (Vitesse, Peloton de Paris, Cafe Welkom)
4. Geographic spread tiebreaker — no single arrondissement dominates the top 20

This is **subjective**: a different weighting would produce a different ordering. Use the JSON `popularity.cited_in` field if you want to compute your own ranking.

### Verification process
- Each entry was researched via WebSearch (multiple targeted queries per cafe)
- Address, phone, opening hours pulled from search snippets when present
- Cycling amenities (bike rack, indoor storage, repair, etc.) only marked `true` when explicitly confirmed in a source — otherwise `null`
- `last_verified` stamped 2026-04-24 for every entry

## Caveats / known limitations

1. **WebFetch was blocked (HTTP 403)** by every Belgian site tested (kempen.be, provincieantwerpen.be, wielercafes.nl, visit.antwerpen.be, individual cafe sites, goudengids.be, etc.). All data was extracted from **WebSearch snippets only**. The originally planned "deep verification by fetching each cafe's site directly" was not achievable. As a result:
    - **Coordinates are all `null`** — no reliable way to extract lat/lon without fetching map pages
    - **`labeled_fietscafe_province` is mostly `null`** — couldn't enumerate the official label list
    - **`google_rating` and `google_review_count` are `null`** — Google Maps pages couldn't be fetched
    - Some opening hours are partial ("opens 10:00 today" without full week)
2. **Hours go stale fast.** Always re-check the cafe's own site/socials before riding.
3. **Cycling amenities aren't always advertised.** Many cafes have bike racks/pumps that aren't mentioned online. `null` ≠ "not present".
4. **"Top N" is subjective.** Ranking captures cycling-community visibility, not a definitive quality ordering.
5. **40 entries, not 50.** The user accepted "more or less than 50 is fine". Beyond 40, additional candidates surfaced in searches (Fietscafé l'Amusette in Bornem, Taverne Kattevenia in Bornem, Campinastrand in Dessel, several more in Wuustwezel/Heist-op-den-Berg/Putte/Bonheiden) but lacked enough verifiable data (street address + at least one secondary signal) to clear the inclusion bar without fabricating fields.
6. **Excluded after research:**
    - **Koningsbos (Kasterlee)** — permanently closed June 2024
    - **Taverne Aan De Molen (Kasterlee)** — under renovation; status unclear
    - **Marcel Bike Café** — located in Vorst (Brussels), not Antwerp province
    - **Ciclo Morello (Hoogstraten)** — bike shop, not a cafe
    - **Pavé (Antwerpen)** — couldn't confirm operating address as a cafe
7. **Two lower-confidence entries** (rank 39 Wijnegemse Pijl, rank 40 Visitor Centre De Liereman) included for geographic balance — both flagged in their `notes` field.

## Refresh guidance

When refreshing the dataset:
1. Re-run searches for each cafe by name + city, prioritizing `openingsuren.vlaanderen` and the cafe's own site for hours
2. Update `last_verified` for every modified entry
3. If WebFetch becomes available, fetch each cafe site for `coords` (from contact page maps) + Google Maps for `google_rating` / `google_review_count`
4. Cross-reference the Provincie Antwerpen "Label Fietscafé" page when accessible to fill `cycling.labeled_fietscafe_province`
5. Validate JSON: `python3 -c "import json; json.load(open('data/antwerp-cycling-cafes.json'))"`
