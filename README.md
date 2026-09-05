# Scott Brand Ministries — Shirt Order App

A single-page web app for taking shirt orders on an iPad. Customers pick a
design, size, and quantity, enter their name/phone/address, and tap **Send
Order** — the order is emailed straight to `scott@scottbrandministries.com`
using [EmailJS](https://www.emailjs.com/), so there's no server to run or pay
for.

If the iPad ever loses Wi-Fi and the email can't go out, the app doesn't lose
the order: it's saved on the iPad (tap **View recent orders on this iPad**),
and the customer is offered a **Mail app** link or a **Copy Order Details**
button as a backup.

## 1. One-time setup: EmailJS (free)

EmailJS lets a plain web page send email without a backend server.

1. Go to https://www.emailjs.com/ and create a free account.
2. **Add an email service** (Email Services → Add New Service). The easiest
   option is "Gmail" — connect whichever inbox you want the *sending*
   account to be (it can be any Gmail account; the order still gets
   delivered *to* `scott@scottbrandministries.com` because that's set as the
   recipient in the template, step 4 below). Note the **Service ID**.
3. Go to **Account → General** and copy your **Public Key**.
4. Go to **Email Templates → Create New Template** and build the template
   customers' orders will be formatted with. Set:
   - **To email:** `scott@scottbrandministries.com` (or use `{{to_email}}`
     and it will already be filled in by the app)
   - **Subject:** e.g. `New Shirt Order from {{customer_name}}`
   - **Content:**
     ```
     New shirt order:

     Name: {{customer_name}}
     Phone: {{phone}}
     Address: {{address}}
     Design: {{design}}
     Size: {{size}}
     Quantity: {{quantity}}
     Submitted: {{order_date}}
     ```
   Save it and note the **Template ID**.
5. Free EmailJS accounts include 200 emails/month, which is plenty for
   in-person order-taking. You can upgrade later if needed.

## 2. Plug your keys into the app

Open `index.html` and find this block near the bottom of the file:

```js
const EMAILJS_CONFIG = {
  publicKey:  "YOUR_PUBLIC_KEY",
  serviceId:  "YOUR_SERVICE_ID",
  templateId: "YOUR_TEMPLATE_ID"
};
const ORDER_EMAIL_TO = "scott@scottbrandministries.com";
```

Replace the three `YOUR_...` placeholders with the values from step 1.
Until this is done, the app shows a friendly error and falls back to the
Mail-app/copy option instead of silently failing.

## 3. Host the page

The page is fully static (just `index.html` + the `assets/` folder), so any
static host works. The simplest free option:

**GitHub Pages**
1. Push this repo to GitHub (already set up if you're reading this from the
   repo).
2. Repo → Settings → Pages → Deploy from branch → pick this branch → `/`
   (root) → Save.
3. GitHub gives you a URL like `https://yourname.github.io/FBC-Database/`.

Any other static host (Netlify, Vercel, Cloudflare Pages, etc.) works the
same way — just point it at this folder.

## 4. Add it to the iPad Home Screen (so it looks/feels like an app)

1. On the iPad, open the hosted URL in **Safari**.
2. Tap the **Share** icon → **Add to Home Screen**.
3. Name it (e.g. "Order Shirts") → **Add**.
4. Launch it from the Home Screen icon like any other app — it opens full
   screen without Safari's address bar.

For a kiosk-style table setup, you can also enable **Guided Access**
(Settings → Accessibility → Guided Access) so the iPad stays locked into
just this app during an event.

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
- On **Send Order**, it calls EmailJS directly from the browser — no
  backend server, no database to maintain.
- Every submitted order is also saved to the iPad's local browser storage
  as a backup log, viewable via the "View recent orders on this iPad" link
  at the bottom of the form. This is local to that iPad/browser only — it's
  a backup, not a synced database.
- If the EmailJS send fails (e.g., no internet), the customer/staff can use
  the **Open in Mail App** or **Copy Order Details** fallback so the order
  is never lost.
