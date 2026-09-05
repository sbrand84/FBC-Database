# Scott Brand Ministries — Shirt Order App

A single-page web app for taking shirt orders in person on an iPad. Customers
pick a design, size, and quantity, enter their name/phone/address, and tap
**Send Order**.

**There is no email, server, or third-party account involved.** Every order
is saved directly on the iPad (in the browser's storage) the moment Send is
tapped — it works even with no Wi-Fi at all. Whenever you want to get orders
off the iPad, open **View / Export Orders** and tap **Share / Export All
Orders**: this opens the iPad's normal Share menu, so you can send the whole
list to yourself by text, Mail, AirDrop to a laptop, save to Notes or Files,
etc. — whatever you prefer, whenever you want, no automatic sending required.

## 1. Get the files onto something you can host

The app is fully static — just `index.html` and the `assets/` folder. No
build step, no server code.

**Easiest: GitHub Pages (free)**
1. If this repo isn't already on GitHub, push it there.
2. In the repo: **Settings → Pages → Deploy from branch** → pick this
   branch → `/ (root)` → **Save**.
3. GitHub gives you a URL like `https://yourname.github.io/FBC-Database/`.

Any other static host (Netlify, Vercel, Cloudflare Pages, etc.) works the
same way. You can also skip hosting entirely and just copy `index.html` and
the `assets` folder onto the iPad via the Files app and open `index.html`
directly in Safari — since there's no email/network step anymore, the app
works completely offline.

## 2. Add it to the iPad Home Screen (so it looks/feels like an app)

1. On the iPad, open the hosted page (or local file) in **Safari**.
2. Tap the **Share** icon → **Add to Home Screen**.
3. Name it (e.g. "Order Shirts") → **Add**.
4. Launch it from the Home Screen icon like any other app — it opens full
   screen without Safari's address bar.

For a kiosk-style table setup, you can also enable **Guided Access**
(Settings → Accessibility → Guided Access) so the iPad stays locked into
just this app during an event.

## 3. Getting your orders

Tap **View / Export Orders** at the bottom of the form any time to:
- See every order saved on that iPad (most recent first)
- Tap **Share / Export All Orders** to send the full list off the iPad —
  text it to yourself, email it, AirDrop it to a computer, save it to
  Notes/Files, whatever the Share menu offers
- **Clear Log** once you've exported, to start fresh (this can't be undone,
  so make sure you've shared/exported first)

Right after submitting a single order, there's also a **Share This Order**
button if you want to send just that one order somewhere immediately.

> **Note:** Orders live in that iPad's Safari storage only. They are not
> backed up anywhere else, and could be lost if someone clears Safari's
> website data or uses Private Browsing. Get in the habit of exporting
> orders regularly (e.g., at the end of each event/day).

## Customizing

- **Designs shown:** edit the four `.design-option` blocks in `index.html`
  and the images in `assets/designs/`. Add or remove cards the same way —
  each is just an image + a radio input with a `value`.
- **Sizes:** edit the `<select id="size">` options.
- **Styling/colors:** the CSS custom properties at the top of the
  `<style>` block (`--navy`, `--accent`, etc.) control the color scheme.

## How it works (no code changes needed to understand this)

- The form validates that every field is filled in and a design is
  selected before allowing submission.
- On **Send Order**, the order is written to the iPad's local browser
  storage (`localStorage`) — nothing is sent over the network.
- The **View / Export Orders** panel reads that same storage to list every
  order, and uses the iPad's built-in Share sheet (`navigator.share`) to
  hand the data off to whatever app you choose.
