# The Kitchen Cabinet

A Standing Committee on Global Cuisine — a website companion to the original spreadsheet, backed by a real-time shared database. Anyone with the link can browse the record, propose new restaurants, update statuses, and edit or remove entries from any device; every change appears on every member's screen instantly.

## Files

- `index.html` — the website (single file, no build step)
- `firestore.rules` — Firestore security rules (must be pasted into the Firebase console; see below)
- `The_Kitchen_Cabinet.xlsx` — the original spreadsheet (kept for posterity)

## Deploy to GitHub Pages

1. Create a new repo on GitHub (e.g. `kitchen-cabinet`).
2. Push `index.html` (and these other files) to the `main` branch.
3. In the repo, go to **Settings → Pages**, set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`, and save.
4. After about a minute, your site is live at `https://<your-username>.github.io/kitchen-cabinet/`. Share that link.

The site uses Firestore only (no Firebase Authentication), so no authorized-domain setup is required.

## How the record works

The site uses **Firebase Firestore** for shared state. There is no approval step and no login; the database *is* the record.

- **Propose** — fill out the form. If a restaurant with the same name is already listed, you'll be asked to confirm before a second entry is added.
- **Change status** — use the dropdown at the bottom of any card (Want to Try → Planned → Visited → Loved It). No need to open the edit form.
- **Edit** — click *Edit* on a card. The form fills with the entry; *Save Changes* updates it in place. Clearing a field removes it from the entry.
- **Delete** — click *Delete* and confirm. An *Undo* button appears in the toast for eight seconds; restoring re-stamps the entry's submission time, so it moves to the top of the "recent" sort.
- The connection indicator at bottom-right shows `● live` when connected, `● offline` if not.

Members work the same way on the Membership page: sign the register, edit, or remove (with Undo).

## Security rules

Firestore starts in **test mode**, which auto-locks after 30 days. Publish the rules in `firestore.rules` before then, and re-publish whenever that file changes:

1. Firebase Console → **Firestore Database → Rules**
2. Paste the full contents of `firestore.rules` and click **Publish**

The rules are deliberately open — anyone with the link can read, create, edit, and delete — but every write is validated:

- Restaurants need a name, a submitter (`added_by`), and a valid continent; members need a name and both dishes.
- Field counts and string lengths are capped.
- New documents must carry a server timestamp (`submitted_at` / `joined_at`), and that timestamp cannot be changed afterwards.
- Only http(s) website links are rendered as clickable on the site; anything else is ignored.

## Free tier — capacity for your group

The Firebase Spark (free) plan covers this site comfortably:

- 50,000 reads/day, 20,000 writes/day, 1 GiB storage
- For 20-30 members: expect a few hundred reads/day and a handful of writes
- **No credit card required.** You cannot be billed accidentally on Spark.

## Managing data directly

Everything can be done from the site, but the Firebase console also works:

1. Firebase Console → **Firestore Database → Data → `restaurants`** (or `members`)
2. Click an entry to edit fields, or use the menu to delete

Keep `name` and `continent` (restaurants) or `name`, `master`, and `favorite` (members) intact when editing in the console, or later edits from the site will be rejected by the rules.

## Local preview

Serve over HTTP so ES modules and Firebase work properly:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`. Local preview talks to the same live database as the deployed site.

✦ In Cuisine We Convene ✦
