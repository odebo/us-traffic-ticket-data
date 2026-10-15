# US traffic ticket data — courts, payment portals, points and traffic school

A machine-readable reference for **what to do after a US traffic ticket**, one record per city plus one per state.

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
| `data/cities.json` | 54 | One object per city: court, payment portal, fine note, traffic-school eligibility, points note, typical lawyer cost, and the items still unverified |
| `data/states.json` | 21 | One object per state: point system, course / record-protection rule, fine source, who handles tickets, the state licensing agency, and FAQs |

## City record

```json
{
  "city": "Philadelphia", "state": "PA", "state_name": "Pennsylvania",
  "slug": "philadelphia-pa", "county": "Philadelphia County",
  "pay_link": "https://...", "pay_label": "...",
  "court_link": "https://...", "court_label": "...",
  "fine_note": "...", "school_eligible": "...", "school_link": "https://...",
  "points_note": "...", "lawyer_cost": "...", "contest_online": true,
  "local": { "h": "...", "paras": ["..."], "bullets": ["..."] },
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

**Arizona** (Phoenix) · **California** (Los Angeles, San Diego, San Jose, San Francisco, Irvine, Sacramento, Fresno, Long Beach, Oakland, Anaheim, Bakersfield) · **Colorado** (Denver, Colorado Springs) · **Florida** (Orlando, Miami, Tampa, Jacksonville, Fort Lauderdale) · **Georgia** (Atlanta) · **Illinois** (Chicago) · **Indiana** (Indianapolis) · **Massachusetts** (Boston) · **Michigan** (Detroit) · **Minnesota** (Minneapolis) · **Missouri** (Kansas City) · **Nevada** (Las Vegas, Reno) · **New York** (New York) · **North Carolina** (Charlotte) · **Ohio** (Columbus, Cleveland, Cincinnati, Akron, Dayton) · **Oregon** (Portland) · **Pennsylvania** (Philadelphia, Pittsburgh) · **Tennessee** (Nashville, Knoxville) · **Texas** (Houston, San Antonio, Dallas, Austin, Fort Worth, Arlington, Plano, Corpus Christi, El Paso, Denton) · **Utah** (Salt Lake City) · **Washington** (Seattle, Spokane, Tacoma)

## Caveats — read before using this

- **Not legal advice.** This is procedural reference information.
- **Rules change.** Fines, deadlines, portal URLs and course rules change constantly. Always confirm with the court or agency named on the citation.
- **A URL is a snapshot, not a guarantee.** Payment portals get replaced (Philadelphia changed payment vendors in January 2025). If a link is dead, the agency name in the same record is still the right starting point.
- **Nothing here is personalised.** Applying any of these rules depends on the specific violation and the specific court.

## Using it

Public domain (CC0). No attribution required, though a link back to https://ticketsimplified.com is appreciated.

Spotted an error, or want a city added? Open an issue with the official page that shows the correct value.

Dataset landing page with field notes, coverage and a suggested citation: <https://ticketsimplified.com/open-data/>
