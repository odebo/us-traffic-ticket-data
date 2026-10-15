# US traffic ticket data — courts, payment portals, points and traffic school

A machine-readable reference for **what to do after a US traffic ticket**: 153 city records across 21 states, plus one record per state.

This is a hand-checked snapshot. Every city record names the court or agency that actually handles the citation and links to that agency's own page — no third-party "pay your ticket here" sites. Every state record covers the point system, whether a course can clear the ticket, where the fine comes from, and which agency decides the case.

Maintained as the open-data layer behind **https://ticketsimplified.com** — the human-readable version, with the same facts written out as plain-English guides.

## Mirrors

| Platform | Link | Format |
|---|---|---|
| **TicketSimplified** (human-readable + dataset page) | [ticketsimplified.com/open-data/](https://ticketsimplified.com/open-data/) | HTML |
| Kaggle | [US Traffic Ticket Courts and Payment Portals](https://www.kaggle.com/datasets/pz2tools/us-traffic-ticket-courts-and-payment-portals) | CSV |
| Hugging Face | [us-traffic-ticket-data](https://huggingface.co/datasets/Asrocky/us-traffic-ticket-data) | CSV, JSONL |
| GitHub (this repository) | [odebo/us-traffic-ticket-data](https://github.com/odebo/us-traffic-ticket-data) | JSON, CSV, JSONL |
| **TicketSimplified API** (read-only JSON, no key) | [api.ticketsimplified.com](https://api.ticketsimplified.com/index.json) | JSON |

## API

The same records are served as a read-only JSON API - no key, no registration, no rate limit:

```sh
curl -s "https://api.ticketsimplified.com/cities.json?state=CA&limit=3"
curl -s "https://api.ticketsimplified.com/states.json?state=TX"
curl -s "https://api.ticketsimplified.com/index.json"     # endpoint index + licence
```

`/cities.json` accepts `?state=`, `?q=` and `?limit=`; `/states.json` accepts `?state=`.
CORS is open (`Access-Control-Allow-Origin: *`), so the API is callable from a browser.

## Files

| File | Rows | What it is |
|---|---|---|
| `data/cities.json` | 153 | One object per city: court, payment portal, fine note, traffic-school eligibility, points note, typical lawyer cost, and the items still unverified |
| `data/states.json` | 21 | One object per state: point system, course / record-protection rule, fine source, who handles tickets, the state licensing agency, and FAQs |
| `data/cities.csv` / `data/states.csv` | 153 / 21 | Flat versions, for spreadsheets and anything that will not read nested JSON |
| `data/cities.jsonl` / `data/states.jsonl` | 153 / 21 | One record per line, for streaming and for loaders that prefer JSONL |

## City record

```json
{
  "city": "Philadelphia", "state": "PA", "state_name": "Pennsylvania",
  "slug": "philadelphia-pa", "county": "Philadelphia County",
  "pay_link": "https://...", "pay_label": "...",
  "court_link": "https://...", "court_label": "...",
  "fine_note": "...", "school_eligible": "...", "school_link": "https://...",
  "points_note": "...", "lawyer_cost": "...", "contest_online": true,
  "local": { "h": "...", "bullets": ["..."] },
  "verify": ["the current fine and costs for your specific violation"]
}
```

`verify[]` is deliberate, not a bug: it lists the facts that could **not** be confirmed against an official source at `last_verified`. Treat unlisted items as checked, listed items as open.

## State record

```json
{
  "points": "...", "school": "...", "cost": "...", "courts": "...",
  "school_tag": "points", "school_short": "...",
  "dmv_label": "...", "dmv_url": "https://...",
  "faq": [{ "q": "...", "a": "..." }]
}
```

`school_tag` is one of `dismiss` (a course ends the case), `withhold` (adjudication withheld), `points` (point reduction only), `alternative` (deferral or supervision), `none` (no course exists).

## Coverage

**Arizona** (Phoenix, Scottsdale, Mesa, Tucson) · **California** (Los Angeles, San Diego, San Jose, San Francisco, Irvine, Sacramento, Fresno, Long Beach, Oakland, Anaheim, Bakersfield, Santa Ana, Huntington Beach, San Bernardino, Fontana, Riverside, Moreno Valley, Stockton, Chula Vista, Fremont, Modesto, Oxnard, Simi Valley, Salinas, Santa Rosa, Fairfield, Vallejo, Visalia, Concord, Chico, Redding, Merced, San Rafael, Woodland) · **Colorado** (Denver, Colorado Springs) · **Florida** (Orlando, Miami, Tampa, Jacksonville, Fort Lauderdale, Tallahassee, St. Petersburg, Naples, Daytona Beach, Lakeland, Melbourne, Gainesville, Ocala, Kissimmee, Boca Raton, Clearwater, Fort Myers, Pensacola, Sarasota, Hialeah) · **Georgia** (Atlanta, Savannah, Augusta, Columbus, Athens, Macon, Marietta) · **Illinois** (Chicago, Aurora, Naperville, Joliet, Rockford, Peoria, Springfield) · **Indiana** (Indianapolis, Fort Wayne, Evansville, South Bend, Bloomington) · **Massachusetts** (Boston, Worcester, Springfield, Lowell, Cambridge) · **Michigan** (Detroit, Grand Rapids) · **Minnesota** (Minneapolis, St. Paul, Duluth, Rochester, St. Cloud) · **Missouri** (Kansas City, St. Louis) · **Nevada** (Las Vegas, Reno) · **New York** (New York, Buffalo) · **North Carolina** (Charlotte, Raleigh, Greensboro, Durham, Wilmington, Asheville, Fayetteville) · **Ohio** (Columbus, Cleveland, Cincinnati, Akron, Dayton, Toledo) · **Oregon** (Portland, Eugene, Salem, Bend, Medford) · **Pennsylvania** (Philadelphia, Pittsburgh, Allentown) · **Tennessee** (Nashville, Knoxville, Chattanooga, Memphis) · **Texas** (Houston, San Antonio, Dallas, Austin, Fort Worth, Arlington, Plano, Corpus Christi, El Paso, Denton, Amarillo, Lubbock, McKinney, Grand Prairie, Brownsville, Killeen, Midland, Waco, McAllen, Richardson, Sugar Land, Round Rock, Tyler) · **Utah** (Salt Lake City, Provo, Ogden, West Valley City, Orem) · **Washington** (Seattle, Spokane, Tacoma)

## Caveats — read before using this

- **Not legal advice.** This is procedural reference information.
- **Rules change.** Fines, deadlines, portal URLs and course rules change constantly. Always confirm with the court or agency named on the citation.
- **A URL is a snapshot, not a guarantee.** Payment portals get replaced (Philadelphia changed payment vendors in January 2025). If a link is dead, the agency name in the same record is still the right starting point.
- **Nothing here is personalised.** Applying any of these rules depends on the specific violation and the specific court.

## Using it

Public domain (CC0). No attribution required, though a link back to https://ticketsimplified.com is appreciated.

Spotted an error, or want a city added? Open an issue with the official page that shows the correct value.

Dataset landing page with field notes, coverage and a suggested citation: <https://ticketsimplified.com/open-data/>
