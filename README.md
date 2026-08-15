# 🍪 Hostel Store

A tablet-first, single-device point-of-sale web app for selling snacks and other products from a hostel room or small kiosk. Customers browse products, build a cart, apply promotional discounts, and pay via a pre-filled UPI QR code. The seller manages inventory, pricing, promotions, orders, profit, and inventory analytics from a PIN-protected dashboard.

The entire application is contained in a single `index.html` file and stores its data locally in the browser using IndexedDB.

> **Important:** This is intentionally a single-device kiosk. There is no shared backend and no synchronization between devices.

---

## What it does

### 🛒 Customer side

- Tablet-friendly, touch-oriented product grid with large product cards and readable text.
- Product photos, names, categories, live selling prices, and stock availability.
- Categories:
  - Snacks
  - Biscuits
  - Drinks
  - Sweets
  - Instant Food
  - Other
- Tap **+** to add products and use the **+ / −** stepper to change quantities.
- Large cart drawer designed for tablet screens.
- Seller-selected **Quick Picks** for one-tap repeat/add-on purchases.
- Cart shows:
  - Item quantities
  - Current selling price
  - Subtotal
  - Promotional discount
  - Final total
- Customer-facing **live promo section**:
  - Shows currently active promo codes.
  - Customers can tap/use a displayed promo or enter a code manually.
  - Only one promo code can be used per order.
  - Shows how much the promotion saves and, for category promotions, which category is affected.
- Checkout generates a **large UPI QR code** with the final amount pre-filled.
- Checkout also shows the subtotal and promotional savings before payment.
- Stock automatically becomes unavailable when quantity reaches zero.

---

## 🏪 Seller dashboard

The seller dashboard is PIN-protected and contains:

### Items

Add and manage inventory listings with:

- Product photo
- Product name
- Category
- **Cost price** — what the seller actually paid
- **MRP** — the price on which the pricing multiplier is applied
- Quantity in stock

Existing listings can be **edited** at any time, including their photo, name, category, cost price, MRP, and quantity.

Stock can also be increased or decreased directly from the item list.

### Pricing model

The app deliberately separates cost price from MRP:

```text
MRP × active pricing multiplier = customer selling price
```

Profit is calculated as:

```text
Selling price − cost price = profit
```

For example:

```text
Cost price = ₹15
MRP = ₹20
Multiplier = 1.25×

Selling price = ₹25
Profit = ₹25 − ₹15 = ₹10
```

The multiplier therefore does **not** operate on cost price.

### Quick Picks

- Mark up to 3 products as seller-selected Quick Picks.
- Quick Picks appear in the customer's cart for easy add-on purchases.

### Promos

Create and manage customer-facing promotional codes.

Supported promotion types:

1. **Percentage discount**
   - Example: `IND` → 15% off
2. **Flat discount**
   - Example: `HOSTEL20` → ₹20 off
3. **Buy X Get Y**
   - Example: `BUY3GET1` → Buy 3, Get 1 Free

Promotions can apply to:

- **Entire cart**
- **One specific category**

Additional controls:

- Minimum cart value
- Inclusive start and end dates
- Usage limit or unlimited usage
- Current usage count
- Enabled/disabled status
- Edit, disable, or delete promotions
- Only one promo can be applied to an order

### Buy X Get Y rules

For Buy X Get Y promotions:

- Every complete qualifying group receives the promotion.
- The cheapest qualifying item(s) are free.
- Category-specific Buy X Get Y promotions only consider products from the selected category.

### Promo usage

A promo usage is counted only after a customer completes the payment flow and the order is recorded. Merely entering or applying a code does not consume a use.

### Promo testing

The Promos tab includes a **Create TEST15 test promo** button.

It creates a temporary:

- `TEST15`
- 15% off the entire cart
- ₹0 minimum cart value
- 20 uses
- Valid today through the next 7 days
- Enabled immediately

This is useful for testing the complete customer checkout and order-recording flow without manually creating a promotion.

### Live promo poster

The Promos tab has a separate marketing-poster generator for promotions.

It can:

- Download a PNG
- Share the PNG
- Print / save as PDF

The poster includes only promotions that are **live right now** and shows important details such as:

- Promo code
- Discount type and value
- Whether it applies to the whole cart or a category
- Minimum cart value
- Validity dates
- Usage limit / remaining uses

This is separate from the product/inventory marketing poster.

### Pricing & UPI

Configure:

- Shop name
- UPI ID
- Payee name
- Marketing footer text
- Standard pricing multiplier
- Price rounding
- Time-based pricing tiers
- Seller PIN

The marketing footer is used by the product marketing PNG, allowing text such as:

```text
Room 214 · DM to order
```

### Dynamic pricing tiers

Add as many time-based pricing windows as needed.

Example:

```text
Standard pricing      ×1.25
Night Market 8–11 PM ×1.50
Late Night 11 PM–3 AM ×1.70
```

Pricing windows can cross midnight.

If multiple tiers overlap, the lower tier in the list wins.

---

## 📦 Inventory analytics

The Seller → **Inventory** tab provides a visual inventory dashboard.

It shows:

- **Current inventory cost**
  - Sum of `cost price × current stock`
- **Current selling value**
  - Value of remaining inventory at the current live selling price
- **Total units in stock**
- **Number of products currently in stock**

### Fastest-moving products

The Inventory tab analyzes completed orders and ranks products by how quickly they are selling.

This helps identify products that should be restocked more aggressively.

### Stock levels

A visual stock overview shows:

- Product photo
- Product name
- Category
- Current quantity
- Cost price
- Stock-level indicator
- Low-stock / in-stock / sold-out status

---

## 📊 Sales, profit & orders

The Seller → **Orders** tab provides both order history and financial analytics.

### Period summary

Toggle between:

- **Today**
- **This week**

The dashboard shows:

- **Sales** — actual recorded customer revenue after applicable discounts
- **Cost** — cost of the products sold
- **Profit** — sales minus cost
- **Orders** — number of completed orders

### Charts

The Orders tab includes:

- **Daily Sales** chart
- **Daily Profit** chart

The charts are constrained to the available screen width and stack vertically on smaller/tablet displays so they do not overflow the screen.

### Individual orders

Each order shows:

- Date and time
- Items and quantities
- Final sales amount
- Cost
- Profit
- Promo code and promotional saving, when applicable

### Removing test orders

Every order has a **× Remove** control.

Removing an order deletes it from the local sales history and causes the sales, cost, profit, order count, and charts to recalculate.

This is intended primarily for removing test transactions.

> Removing an order does not automatically restore a promotional usage. Promo usage is treated separately from sales-history cleanup.

---

## 📣 Product marketing poster

The Items tab contains a product marketing-poster generator.

It can:

- Download a PNG
- Share the PNG
- Print / save as PDF

The product poster:

- Includes all categories that currently contain in-stock products.
- Uses the real uploaded product photos.
- Uses the current live selling prices.
- Automatically skips out-of-stock products.
- Shows a small remaining-stock indicator.
- Uses the configured marketing footer.
- Adds pricing-tier urgency information when applicable.

Example footer behavior:

```text
Prices rise to ×1.5 at 8 PM
```

If there are multiple pricing tiers, the poster adapts the message automatically.

---

## 💾 Under the hood

- Single `index.html` file.
- HTML, CSS, and JavaScript are inline.
- No build system or installation is required.
- Data is stored locally in the browser's **IndexedDB**.
- Separate local data includes:
  - Items
  - Product photos
  - Orders
  - Settings
  - Promotions
- QR codes are generated client-side using the embedded QR library.
- Product photos are resized/compressed when uploaded to reduce storage usage.
- The app requests a screen wake lock so the kiosk tablet can remain awake where supported.

---

## ⚠️ Important: local database / single-device architecture

There is **no shared backend**.

Every browser/device that opens the application has its own local database.

Therefore:

- Inventory does not sync between devices.
- Orders do not sync between devices.
- Promotions do not sync between devices.
- Settings do not sync between devices.

The intended setup is:

```text
One tablet
    ↓
Hostel Store kiosk
    ↓
Customers browse → order → pay
    ↓
Seller manages everything on the same tablet
```

If multiple devices need to share the same inventory and orders, the application would need a real backend/database architecture.

---

## 🚀 Getting started

1. Open `index.html` on the tablet that will act as the kiosk.
2. Tap the **⚙︎** seller/settings button.
3. Enter the seller PIN.
4. Go to **Pricing & UPI** and configure:
   - UPI ID
   - Payee name
   - Shop name
   - Seller PIN
5. Go to **Items** and add products:
   - Photo
   - Name
   - Category
   - Cost price
   - MRP
   - Quantity
6. Optionally configure:
   - Quick Picks
   - Pricing tiers
   - Promo codes
   - Marketing footer
7. Use **Inventory** to monitor stock and fast-moving products.
8. Use **Orders** to monitor sales, profit, and daily trends.
9. Return to the customer view.

### Default settings

| Setting | Default |
|---|---|
| Seller PIN | `1234` |
| Standard multiplier | `1.25×` |
| Night Market | `8:00 PM – 11:00 PM, ×1.5` |
| Late Night Market | `11:00 PM – 3:00 AM, ×1.7` |
| Price rounding | Nearest ₹1 |
| UPI ID | Empty — must be configured |

**Change the default seller PIN before using the app for real sales.**

---

## 💰 How pricing and profit work

The app maintains two separate prices for each product:

| Value | Meaning |
|---|---|
| Cost price | What the seller paid for the product |
| MRP | Reference price used by the pricing multiplier |

The customer price is:

```text
MRP × active multiplier
```

The profit is:

```text
customer selling price − cost price
```

Promotional discounts are applied **after** the current live selling price has been calculated.

For example:

```text
Cost price = ₹15
MRP = ₹20
Multiplier = ×1.5

Live selling price = ₹30

15% promo discount = ₹4.50

Customer pays = ₹25.50

Profit = ₹25.50 − ₹15
       = ₹10.50
```

Historical orders store their financial information so later changes to an item's current cost/MRP do not change the stored economics of a completed sale.

---

## 💳 Payment flow

The app does not automatically verify UPI payments.

The customer:

1. Opens checkout.
2. Scans the generated UPI QR.
3. Pays the displayed amount.
4. Seller/customer confirms payment.
5. **I've Paid — Done** records the order and deducts stock.

The QR contains the final payable amount, including applicable promotional discounts.

---

## ⚠️ Notes & limitations

- Payment is **not automatically verified**. The seller must verify the payment independently.
- Clearing browser/site data can erase the local IndexedDB data.
- Keep regular backups if the inventory/order history is important.
- The seller PIN protects the seller dashboard UI, but this is still a client-side web application.
- GitHub Pages and similar static hosting expose the page source to users. Do not treat the client-side PIN as a secure server-side authentication mechanism.
- The application is designed for one kiosk device.
- Historical profit requires stored cost information. Older orders created before cost-price tracking may not have complete historical cost data.
- Removing an order is an administrative/testing cleanup operation and should not be used as a substitute for proper accounting records.

---

## 🌐 Deployment with GitHub Pages

1. Create a repository.
2. Upload `index.html`.
3. Go to **Settings → Pages**.
4. Set the source to the main branch and `/(root)`.
5. Open the generated GitHub Pages URL on the kiosk tablet.
6. Use the browser's **Add to Home Screen** option for a more app-like experience.

Because this is a local-first application, the tablet's browser storage is the actual data store.

---

## 📜 License

The embedded QR code generator is based on **qrcode-generator** by Kazuhiko Arase and is MIT licensed.

Everything else is free to use, modify, and redeploy for a hostel, dorm, office snack stand, or similar small self-service kiosk.
