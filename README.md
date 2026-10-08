# Al Murooj Hotel Apartments: website

A static website (HTML, CSS and a little JavaScript) for **Al Murooj Hotel Apartments** (المروج للشقق الفندقية), a licensed 3-star aparthotel in Al Azaiba North, Muscat, Oman.
English (`/`) and Arabic (`/ar/`, right to left) are both written by hand. There is no server code and no database. Bookings go straight to WhatsApp (+968 9194 2758) with a pre-filled message.

Live at **https://jeeveshverma.github.io/al-murooj-hotel-apartments/** (GitHub Pages, branch `main`, root folder).

## Files

```
index.html            English home page
ar/index.html         Arabic home page (right to left)
404.html              "Page not found" page (works under the /al-murooj-hotel-apartments/ path and at a domain root)
assets/css/style.css  All styles; colours, fonts and sizes are set at the top in :root
assets/css/fonts.css  Self-hosted fonts (no Google requests): Fraunces, Plus Jakarta Sans, Tajawal, Noto Kufi Arabic
assets/js/main.js     WhatsApp booking form, price estimate, lightbox, gallery filter, language menu, mobile menu, scroll animations
assets/img/           logo.svg, favicon.svg, apple-touch-icon.png, pattern.svg, og-image.jpg
assets/img/photos/    30 hotel photos (JPEG, max 1600 px) plus WebP variants at 540/800/1080 px made by the build
assets/img/places/    13 Muscat landmark photos from Wikimedia Commons (see Photo credits) plus WebP variants
data/photo-credits.json  Author, licence and source page for every landmark photo
data/i18n/            Translation catalogs for generated languages (none yet; _source.json is written by the build)
tools/                build.py, i18n.py, images.py, check_html.py, check_i18n.py (Python 3; images.py needs cwebp)
og-preview.html       The page the social-preview image (og-image.jpg) was rendered from; not linked from the site
favicon.ico, robots.txt, sitemap.xml, .nojekyll
```

## Build

**After changing `index.html` or `ar/index.html`, run `python3 tools/build.py`.** It:

* adds a WebP `srcset` to every photo (making missing variants with `cwebp`; `brew install webp`),
* regenerates the structured data from the page text: the `Hotel` block gains the four rooms as `HotelRoom` (size, beds, occupancy, "from" price in OMR) and a `FAQPage` block is built from the FAQ,
* rewrites the language menu, the `hreflang` links and `sitemap.xml`,
* joins the digits of phone numbers with non-breaking spaces so they never wrap.

Occupancy and bed types are not written on the page, so they live in `ROOM_FACTS` in `tools/build.py`, keyed by each room's main photo name.

### Adding a language

The build can generate more languages from the English page: add a tuple to `LANGS` in `tools/build.py` (code, hreflang, name, og:locale, Google Maps `hl`) and create `data/i18n/<code>.json` with a translation for each key in `data/i18n/_source.json`. Strings hide numbers and HTML behind placeholders (`<0>…</0>`, `{0}`), so a translation cannot change a price or break a link. `python3 tools/check_i18n.py` lists missing strings and fails on a broken placeholder. The Arabic page stays hand-written.

## Checks

```
python3 tools/check_html.py   # balanced tags, duplicate ids, heading order, anchors, alt text, JSON-LD, file references
python3 tools/check_i18n.py   # every generated language complete, no broken placeholders
npx html-validate@8 index.html ar/index.html 404.html   # optional HTML5 lint (needs Node)
```

## Preview locally

```
python3 -m http.server 8000   # then open http://localhost:8000
```

## Publish

Settings → Pages → Deploy from branch → `main` / root. The site is served under `/al-murooj-hotel-apartments/`; `404.html` detects that prefix.

The hotel does not appear to own a domain (none is listed on Google, Trip.com, Expedia or its Facebook page). If it buys one, set it under Settings → Pages → Custom domain, point the DNS at GitHub Pages (four A records 185.199.108–111.153 and a `www` CNAME to `jeeveshverma.github.io`), then search-and-replace `https://jeeveshverma.github.io/al-murooj-hotel-apartments/` in `tools/build.py` (`BASE`), both pages and `robots.txt`, and rebuild.

## Sections

Hero with arch-masked photos and the "from OMR 12" badge · WhatsApp booking card with a price estimate · included-with-every-stay strip · welcome story ("the meadows of Azaiba") · numbers strip · rooms and apartments (four cards, each opens a photo set) · filterable masonry gallery (24 photos) · weekly and monthly stays · services · dining · location with an illustrated sketch map, distances and "getting here" · explore Muscat (8 landmarks) and day trips (4) · reviews · good to know and FAQ · call to action · footer with photo credits.

## Design

Palette: deep teal `#0F4C5C` (the bedding and the sea), sand `#F6F0E6`, copper `#C4712F`. Display type Fraunces, body Plus Jakarta Sans; Arabic in Noto Kufi Arabic (headings) and Tajawal (body). The recurring pointed arch (`clip-path: url(#arch)`, an inline SVG `clipPath` in `objectBoundingBox` units) echoes the arched windows on the facade and the lobby doorways. The logo mark is an arch over three meadow blades, for the name. `pattern.svg` is a faint eight-point star lattice used on the hero, the long-stay panel and the call to action.

## Where the facts came from

| Fact | Source |
|---|---|
| Licence L1041083 (Ministry of Heritage and Tourism, "Operation of Hotel Apartments, Ordinary", first registered 9 Mar 2015, valid to 16 Mar 2030), 30 apartments, 74 beds, trademark 108015 (12 Dec 2017), commercial registration 1011337 (13 Jan 2007), location "North Aludhaybah / Bousher" | Licence certificate photographed on the hotel's own listing (topomanhotels.com gallery, images 16 and 17), printed 19 Mar 2025 |
| Phones +968 2449 9786, +968 2449 9780, +968 9194 2758; email sales.almurooj@gmail.com; address "Al Azaiba, PO Box 24, Muscat 105"; room types and bed setups; check-in 14:00 / check-out 12:00; children all ages, extra bed OMR 5, no cots; pets no; cash and card; 8.3/10 from 36 reviews with category scores; review quotes | Trip.com listing, 8 Oct 2026 |
| Facebook page "Al Murooj HOTEL Apartments 24499786/780" | facebook.com |
| Google rating 3.7 (485 reviews), coordinates 23.59284, 58.373959, place id ChIJU9MBYeH_kT4RIyEcpoCbiDM, Turkish restaurant and rooftop pool mentioned by reviewers | Google Maps via Wanderlog |
| Tripadvisor quotes (Al Harthe Dec 2025, LM Sep 2023), price band $30–38 | Tripadvisor, 8 Oct 2026 |
| One-bedroom ≈ 60 m², two-bedroom ≈ 80 m² | Expedia (search snippet; the page itself rate-limited) |
| Breakfast OMR 2, airport shuttle for a surcharge, free parking, 24-hour desk, lift, step-free entrance, laundry, car hire, luggage storage, wake-up calls, CCTV, keycards, smoke detectors | Trip.com and Expedia amenity lists |
| Microwave, cookware, tea and coffee facilities, Amex/Visa/Mastercard, Azaiba bus stop nearby, Athaiba Public Park 550 m, Azaiba Beach 19 min on foot, 38 rooms (conflicts with the licence's 30) | topomanhotels.com |
| Distances and drive times | Trip.com (airport 9.8 km, Grand Mosque 4.1 km), Expedia (Avenues Mall 5 min) and the map for the rest, rounded |
| Ground-floor restaurants (Turkish grill, rotisserie chicken) | Visible on the listing photos; reviewers mention a Turkish restaurant downstairs |

## Before going live: confirm with the hotel

| Item | Current value on the site | Why it needs checking |
|---|---|---|
| WhatsApp number | +968 9194 2758 (`wa.me/96891942758`) | The mobile number from the Trip.com listing. Confirm it is on WhatsApp and who answers it; the landlines are 2449 9786 / 9780 |
| Prices | Double and Twin from OMR 12, One-Bedroom from OMR 18, Two-Bedroom from OMR 25 | Indicative, derived from OTA price bands ($31–65) on 8 Oct 2026. The hotel should set the real rack rates; prices are in the `<option data-price>` values and the room cards |
| Breakfast price | OMR 2 per person | Trip.com child/breakfast notes |
| Extra bed | OMR 5 per night, no cots | Trip.com. topomanhotels says no extra beds at all |
| Room sizes | 60 m² and 80 m² for the apartments; none shown for the standard rooms | Expedia estimates |
| Room count | 54 rooms | The owner's figure (WhatsApp, 8 Oct 2026). The licence says 30 apartments and 74 beds, so 54 is probably the number of bedrooms across the apartments; listings say 30, 32, 38 or 48 |
| Kitchen contents | Gas hob, microwave, fridge, kettle; "tell us if you plan to cook" | Some reviews complain of missing utensils, hence the wording |
| Restaurants downstairs | "Turkish grill and rotisserie chicken house", unnamed | Signs on the facade read فروج أبو العبد, قصر المشاوي and أناتوليا. Confirm which are open and whether the hotel serves breakfast itself |
| Rooftop pool | Not mentioned on the site | Google reviewers and a topoman FAQ mention a pool; the only pool photos online look like a different hotel. Add it only if it exists |
| Airport transfer | "For a small charge" | A 2023 listing quoted OMR 8 per person return |
| Postal code | 105 (P.O. Box 24) | Trip.com. Google shows 192; Azaiba's code is usually 130 |
| Address line | "Al Azaiba North, Bausher" | From the licence; Google shows "48th St". Add the street and building number |
| Payment | Cash, Visa, Mastercard, Amex; no prepayment | Trip.com (cash and card) and topoman (card brands) |
| Cancellation | "We confirm the terms with your quote" | Not published for direct bookings |
| Languages spoken | Arabic and English | Not verified |
| Lift and step-free entrance | Stated | Trip.com amenity list |
| Bus | "Mwasalat buses stop at Azaiba, five minutes' walk" | topoman names the "Al Azaiba - A" stop; check the route |
| Opening year | "Since 2015" | First licence registration; the building may be older |
| Instagram | Not linked | No account found; add one if the hotel has it |
| Google rating | Not shown (only a link) | 3.7/5 on Google versus 8.3/10 on Trip.com; the Trip.com score is shown with its date |

## Editing

* **Prices:** change the `data-price` values in the booking form and the `OMR` figures in the room cards and the hero badge, in both `index.html` and `ar/index.html`, then run the build (the structured data picks them up).
* **WhatsApp number:** change `PHONE` in `assets/js/main.js` and the `wa.me/96891942758` links in both pages.
* **Photos:** replace files in `assets/img/photos/` keeping the names, delete that photo's `-540/-800/-1080.webp` variants and run the build. Photos are at most 1600 px; larger originals from the hotel will look better, especially `exterior-facade.jpg` in the hero.
* **Reviews:** verbatim quotes from Trip.com and Tripadvisor (light punctuation fixes only); the Arabic page carries translations marked (مترجم). Keep the attribution if you change them.
* **Social preview:** `og-preview.html` is the 1200×630 page the preview image was screenshotted from. Edit it, open it in a browser at that size and save the screenshot as `assets/img/og-image.jpg`.

## Photo credits

Hotel photos are the hotel's own, taken from its public listings. The landmark photos in `assets/img/places/` are from Wikimedia Commons under Creative Commons licences and are credited in the page footer and in `data/photo-credits.json` (author, licence, source page). Each was cropped to 3:2 and resized to 1200 px. CC BY-SA photos require the same credit if reused elsewhere.

Fonts: Fraunces, Plus Jakarta Sans, Tajawal and Noto Kufi Arabic, all under the SIL Open Font License, served from `assets/fonts/`.
