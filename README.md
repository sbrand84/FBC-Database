# Scott Brand Ministries — Shirt Order App

A single-page web app for taking shirt orders in person on an iPad. Customers
pick a design, size, and quantity, enter their name/phone/address, and tap
**Send Order**.

**There is no server or third-party account involved.** Every order is saved
directly on the iPad (in the browser's storage) the moment Send is tapped —
it works even with no Wi-Fi at all. On the confirmation screen, tapping
**Email Packing Slip to Scott** opens the iPad's Mail app, pre-filled and
addressed to `scott@scottbrandministries.com`, with the order formatted as a
packing slip (checkboxes next to each item) — just tap Send in Mail. Every
order is also kept in an **Order History** log on the iPad, where each order
can be marked **Unfulfilled**/**Fulfilled** and saved, and the whole log can
be exported at once via the Share menu.

## 1. Get the files onto something you can host

The app is fully static — just `index.html` and the `assets/` folder. No
build step, no server code.

**Easiest: GitHub Pages (free)**
1. If this repo isn't already on GitHub, push it there.
2. In the repo: **Settings → Pages → Deploy from branch** → pick this
   branch → `/ (root)` → **Save**.
3. GitHub gives you a URL like `https://yourname.github.io/FBC-Database/`.
   (The very first deploy is kicked off by a push to the branch — if the
   site 404s right after enabling Pages, push any small change and it'll
   trigger the build.)

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

Right after an order is submitted, tap **Email Packing Slip to Scott** on the
confirmation screen — it opens Mail, addressed to
`scott@scottbrandministries.com`, with the order already written up as a
packing slip. Tap Send in Mail and it's on its way. (This needs a Mail
account already signed in on the iPad — any account works, since the
*recipient* is always `scott@scottbrandministries.com` regardless of which
account sends it.)

Tap **View / Export Orders** at the bottom of the form any time to open
**Order History**, where you can:
- See every order saved on that iPad (most recent first), with a status pill
- Change an order's dropdown to **Unfulfilled**/**Fulfilled** and tap **Save**
  next to it to track which orders still need to be packed
- Tap **Share / Export All Orders** to send the full list off the iPad —
  text it to yourself, email it, AirDrop it to a computer, save it to
  Notes/Files, whatever the Share menu offers
- **Clear Log** once you've exported, to start fresh (this can't be undone,
  so make sure you've shared/exported first)

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
- On **Send Order**, the order (with a generated ID and "Unfulfilled"
  status) is written to the iPad's local browser storage (`localStorage`).
- **Email Packing Slip to Scott** builds a `mailto:` link with the order
  formatted as a packing slip and navigates to it, which hands off to Mail —
  no network request happens on this page itself.
- The **Order History** panel reads that same storage to list every order.
  Changing an order's status and tapping **Save** updates that one order's
  record in storage. **Share / Export All Orders** uses the iPad's built-in
  Share sheet (`navigator.share`) to hand the whole log off to whatever app
  you choose.
