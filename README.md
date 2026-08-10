# 🍪 Hostel Store

A tiny point-of-sale web app for reselling snacks out of a hostel room. One HTML file, no server, no database, no internet required once loaded — built to run continuously on a single tablet that customers use to browse, add to cart, and pay you via UPI.

**Live demo:** add your GitHub Pages link here once deployed, e.g. `https://yourusername.github.io/hostel-store/`

---

## What it does

### Customer side
- Blinkit/Zepto-style grid of items — photo, name, live price
- Tap **+** to add an item, then a **+ / −** stepper to adjust quantity
- Category filter chips: Snacks, Biscuits, Drinks, Sweets, Instant Food, Other
- Cart drawer with running total and 2–3 seller-picked **quick add** items for one-tap repeat orders
- Checkout generates a **UPI QR code with the amount pre-filled** — the customer just scans and pays, no typing
- Stock automatically goes out of stock / greys out when quantity hits 0

### Seller side (PIN-protected)
- **Items** — add a photo, name, category, cost price, and starting stock; adjust stock up/down anytime with a tap; delete items
- **Quick Picks** — mark up to 3 items as one-tap shortcuts shown in the customer's cart
- **Pricing & UPI** — set your UPI ID, payee name, shop name, and seller PIN
- **Dynamic pricing tiers** — add as many time-based pricing windows as you want (e.g. "Night Market" 8–11pm ×1.5, "Late Night Market" 11pm–3am ×1.7), each with its own multiplier on top of your standard multiplier. Handles windows that cross midnight.
- **Orders log** — every completed order with timestamp, items, and total, plus a "today's sales" summary, so you can reconcile against your bank SMS

### Under the hood
- Single `index.html` file — HTML, CSS, and JS all inline, nothing to install or build
- Data (items, photos, orders, settings) is stored in the browser's **IndexedDB** — fully local to the device, no backend, no account
- QR codes are generated **entirely client-side** (a small embedded QR library) — works with zero internet connection
- Requests a **screen wake lock** so the tablet doesn't sleep while running as a kiosk
- Photos are auto-resized and compressed on upload to keep storage light

---

## ⚠️ Important: the database is local to each device

There is no shared backend. Every device that opens this link gets its **own separate, empty copy** of the app — items, stock, and orders do **not** sync between devices. This is meant to run on **one single device** (your tablet) that acts as the till. If two different devices both add inventory, you'll end up with two out-of-sync stores.

---

## Getting started

1. Open the app on the tablet you'll actually use as the till.
2. Tap the small **⚙︎** gear icon in the bottom-left corner.
3. Enter the default seller PIN: **`1234`**
4. Go to **Pricing & UPI** and set, at minimum:
   - Your **UPI ID** (e.g. `yourname@upi`)
   - **Payee name** (shown to the customer before they pay)
   - **Shop name**
   - A new **seller PIN** (change this from the default!)
5. Adjust the standard multiplier and pricing tiers if you don't want the defaults (see below).
6. Go to **Items** and add your snacks — photo, name, category, cost price, and quantity in stock.
7. (Optional) Go to **Quick Picks** and mark your 2–3 best sellers.
8. Tap the back arrow to return to the customer view — you're live.

### Default settings out of the box

| Setting | Default |
|---|---|
| Seller PIN | `1234` |
| Standard multiplier | 1.25× |
| Night Market tier | 8:00 PM – 11:00 PM, ×1.5 |
| Late Night Market tier | 11:00 PM – 3:00 AM, ×1.7 |
| Price rounding | nearest ₹1 |
| UPI ID | *(empty — must be set before checkout will work)* |

> Base price = your cost price. The multiplier is your markup — whatever tier is active for the current time gets used automatically, with the standard multiplier as the fallback when no tier is active. You can add, edit, or remove tiers freely; if two tiers' time windows overlap, the one lower in the list wins.

---

## How pricing tiers work

- Every item has one **base price** — what you paid for it.
- At checkout, the customer sees `base price × current multiplier`, rounded to your chosen rounding.
- The active multiplier is whichever pricing tier's time window matches right now; if none match, the standard multiplier applies.
- Tiers can wrap past midnight (e.g. 11:00 PM → 3:00 AM works correctly).

---

## Deployment (GitHub Pages)

1. Create a **public** repository (GitHub Pages on the free plan requires public repos).
2. Upload `index.html` (rename it to `index.html` if it isn't already, so it loads at the root URL).
3. Go to **Settings → Pages**, set Source to your main branch, folder `/(root)`.
4. Your live link will appear at `https://yourusername.github.io/reponame/`.
5. Open that link on the tablet, then use the browser's **"Add to Home Screen"** option for a full-screen, app-like experience.

Since the compiled page's source is publicly viewable once deployed (this is how all GitHub Pages sites work), **make sure you've changed the seller PIN** in-app before relying on it — don't leave it at `1234`.

---

## Notes & limitations

- No payment confirmation system — you manually confirm payment against your bank app/SMS. Tapping "I've Paid — Done" simply logs the order and deducts stock; it doesn't verify anything.
- Clearing browser data/cache on the tablet will wipe the local database. Don't clear site data for this page.
- Built for one device. See the section above if you're thinking about multiple sellers/devices — that needs a different architecture with a real backend.

---

## License

The embedded QR code generator is [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) by Kazuhiko Arase, MIT licensed. Everything else is free to use, modify, and redeploy for your own hostel/dorm/office snack stand.
