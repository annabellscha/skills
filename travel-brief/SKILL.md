---
name: travel-brief
description: Generate a concise, actionable travel brief for a destination. Trigger this skill whenever the user mentions traveling to a country or city, asks for travel tips, trip prep, or says things like "I'm going to X", "help me prepare for my trip to X", "what do I need to know about visiting X". The skill covers transit cards, SIM/eSIM, ATMs and cash, free city views, rooftop bars, local vibe and food, currency, and entry requirements — all in a tight bullet-point format with map links for places.
---

# Travel Brief Skill

Generate a structured, tight travel brief for any destination. Based on the conversation, the user is traveling somewhere and wants a quick but complete prep sheet.

## Step 0 — Read user profile (always do this first)

Read `{base_directory}/reference/USER_PROFILE.md`. For every non-blank field, override the corresponding default below. Blank fields fall back to skill defaults.

| Field | Effect |
|---|---|
| `nationality` / `passport_country` | Use for visa/entry section instead of defaulting to EU/German |
| `home_currency` | Show exchange rate for this currency (default: EUR) |
| `maps_app: apple` | Generate Apple Maps links: `[Apple Maps](https://maps.apple.com/?q=VENUE+NAME+CITY)` |
| `maps_app: google` or blank | Generate Google Maps links via `places_search` (default) |
| `bank_card` | Tailor ATM advice — e.g. Schwab/Wise/Revolut users have no ATM fees |
| `accommodation_style` | Adjust bar/restaurant tier: budget → local spots, luxury → hotel rooftops |
| `dietary` | Filter food recommendations accordingly |
| `interests` | Weight sections — e.g. "food" → expand food section; "hiking" → add day trip note |
| `languages_spoken` | Note if local language knowledge changes navigation or social tips |
| `past_experiences` | Use as a taste profile — find similar things at the new destination (e.g. loved High Line → look for elevated park walks; hated tourist crowds → steer toward quieter alternatives) |
| `notes` | Factor in any free-text context (kids, business trip, etc.) |

## What to cover (always in this order)

1. **🚇 Transit Card** — local public transport card, how to get it, iPhone/Android wallet support
2. **📱 SIM** — best eSIM provider (default: Airalo), coverage notes
3. **🏧 ATMs & Cash** — lowest/no-fee ATM option, rough cash estimate needed, card acceptance level
4. **🏙️ Free City Views** — at least one free observation deck or high point; note the "free hack" if applicable
5. **🍸 Rooftop Bars** — 2–3 top picks with vibe, price range, opening hours
6. **🍜 Food & Vibe** — must-eat dishes, neighborhood breakdown (3–4 neighborhoods with one-word descriptor each)
7. **💱 Currency** — current rate for user's home currency vs local, simple mental math shortcut
8. **✈️ Entry Requirements** — visa needed (yes/no + conditions), any pre-registration system (link + time needed), documents to bring

## Format rules

- **Bullet points only** — no prose paragraphs
- Each section has a bold emoji header
- Places always get a map link inline (format depends on `maps_app` field — see Step 0)
- Keep each bullet to one line where possible
- No intros, no outros, no "here's your brief!" — just the brief

## Map link formats

**Google Maps** (default):
- Use `places_search` to get the place_id; format: `[Google Maps](https://www.google.com/maps/place/?q=place_id:PLACE_ID_HERE)`
- Never guess or fabricate a place_id

**Apple Maps** (when `maps_app: apple`):
- Format: `[Apple Maps](https://maps.apple.com/?q=VENUE+NAME+CITY)` — encode spaces as `+`
- No external lookup needed

## Research process

Use `web_search` for each section in parallel where possible:

```
1. "[destination] IC card transit card iPhone wallet [year]"
2. "[destination] eSIM tourist [year] Airalo"
3. "[destination] ATM no fee tourists [year]"
4. "is [destination] cashless [year] can you travel without cash"
5. "[destination] free city view observation deck"
6. "best rooftop bars [destination] [year]"
7. "[destination] entry requirements tourists [year] visa [passport_country or EU/German]"
8. "[home_currency] [local currency] exchange rate today"
```

## Example output structure

```
## 🚇 Transit Card
- Add **Suica** to Apple Wallet (iPhone 8+) — no physical card needed
- Works on trains, buses, vending machines, convenience stores
- Top up via Apple Pay; tap top of iPhone to turnstiles

## 📱 SIM
- **Airalo eSIM** — install before flying, activates on landing
- Connects to [carrier], good city + rural coverage

## 🏧 ATMs & Cash
- **[ATM name]** — no local fee; your home bank fee still applies
- Bring ~[amount] cash for [specific cash-only situations]
- Cards accepted at hotels, chains, convenience stores

## 🏙️ Free Views
- **[Place name]** — [height/floor], free, [tip] → [map link]

## 🍸 Rooftop Bars
- **[Bar name]** — [floor], [vibe], from [price] → [map link]
- **[Bar name]** — [floor], [vibe], open [hours] → [map link]

## 🍜 Food & Vibe
- Must-eat: [dish 1], [dish 2], [dish 3]
- Neighborhoods: [Name] (vibe), [Name] (vibe), [Name] (vibe)

## 💱 Currency
- **€1 ≈ [X] [currency]** — [context e.g. "favorable for Europeans"]
- Mental math: [unit] ≈ €[X]

## ✈️ Entry
- **[Visa status]** for [nationality] (up to X days)
- Register on **[System name]** before flying — saves X min at immigration → [link]
- Bring: passport, flight number, hotel address
```

## Notes

- If `passport_country` is blank, default to EU/German but note the assumption
- If a section has no data (e.g., no free views found), skip it rather than guessing
- Currency rate: always search for current rate, never use training data
- Entry requirements: always search, visa rules change frequently
