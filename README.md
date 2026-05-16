# The Kitchen Cabinet

A Standing Committee on Global Cuisine — a website companion to the original spreadsheet, backed by a real-time shared database. Anyone with the link can browse the record and propose new restaurants from any device; submissions appear on every member's screen instantly.

## Files

- `index.html` — the website (single file, no build step)
- `firestore.rules` — production security rules for Firestore (paste into Firebase console before day 30)
- `The_Kitchen_Cabinet.xlsx` — the original spreadsheet (kept for posterity)
- `restaurants.json` — legacy starter file, no longer used by the site

## Deploy to GitHub Pages

1. Create a new repo on GitHub (e.g. `kitchen-cabinet`).
2. Push `index.html` (and these other files) to the `main` branch.
3. In the repo, go to **Settings → Pages**, set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`, and save.
4. After about a minute, your site is live at `https://<your-username>.github.io/kitchen-cabinet/`. Share that link.

## Authorize the live domain in Firebase

Once you know the Pages URL:

1. Firebase Console → **Authentication → Settings → Authorized domains**
2. Add `<your-username>.github.io`

(`localhost` already works, so local testing needs no setup.)

## How submissions work

The site uses **Firebase Firestore** for shared state. When a member fills out the form:

1. The entry is written directly to the `restaurants` collection.
2. Firestore pushes the change to every connected device in real time — no refresh needed.
3. The connection indicator at bottom-right shows `● live` when connected, `● offline` if not.

No maintainer approval step, no `restaurants.json` merging. The database *is* the record.

## Free tier — capacity for your group

The Firebase Spark (free) plan covers this site comfortably:

- 50,000 reads/day, 20,000 writes/day, 1 GiB storage
- For 20-30 members: expect a few hundred reads/day and a handful of writes
- **No credit card required.** You cannot be billed accidentally on Spark.

## Harden security before day 30

Firestore was started in **test mode**, which auto-locks after 30 days. Before that:

1. Open `firestore.rules` in this repo.
2. In Firebase Console → **Firestore Database → Rules**, paste the contents and click **Publish**.

These rules:
- Allow anyone to read (public list)
- Allow new submissions if well-formed (name + added_by + valid continent)
- Block all edits and deletes from the client (you can still manage entries in the Firebase console)

## Editing or removing an entry

Use the Firebase console:

1. Firebase Console → **Firestore Database → Data → `restaurants`**
2. Click an entry to edit fields, or use the menu to delete

Changes appear on the live site within a second.

## Local preview

Serve over HTTP so ES modules and Firebase work properly:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`.

✦ In Cuisine We Convene ✦
