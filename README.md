# Zotero → GitHub Pages: bibliography template

A website that shows a Zotero library – formatted citations, tags and your notes – and fetches it live
from the [Zotero Web API](https://www.zotero.org/support/dev/web_api/v3/start) every time someone opens the page.
No build step, no installation: plain HTML, CSS and JavaScript, published with GitHub Pages.

DH Lab · University of Bern · Autumn semester 2026 (Session 6, Introduction to APIs)

## Get it online in five steps

1. **Use this template** (green button, top right) → create a repository in your own account, e.g. `dh-bibliography`.
   _Already have a repository from the Intro course?_ Upload the five files (`index.html`, `about.html`,
   `config.js`, `app.js`, `style.css`) into it instead.
2. Open `config.js` and edit it (pencil icon): `title`, `intro`, and which library to show (see below).
   Commit.
3. **Settings → Pages** → Source: **Deploy from a branch** → `main` / `(root)` → **Save**.
4. Wait 1–2 minutes, then open `https://USERNAME.github.io/REPOSITORY/`.
5. Fill in `about.html`: topic, workflow, what failed, where you used AI.

## Which library?

| You want | `libraryType` | `libraryId` | Notes |
|---|---|---|---|
| The open group **dh_unibe** | `"groups"` | `"2386895"` | Works right away (this is the default) |
| Another public group | `"groups"` | number from `zotero.org/groups/<number>/…` | Group must be public |
| Your own library | `"users"` | your **userID** – shown at [zotero.org/settings/keys](https://www.zotero.org/settings/keys) | Library must be public: [zotero.org/settings/privacy](https://www.zotero.org/settings/privacy) → *Publish Entire Library* |

`collectionKey` limits the page to one collection: open the collection on zotero.org, the 8 characters
after `/collections/` in the URL are the key. Leave it `""` for the whole library.

Other settings: `style` (any [CSL style](https://www.zotero.org/styles) id, e.g. `apa`),
`groupBy` (`"none"`, `"year"`, `"type"`, `"tag"`), `showNotes` (child notes as annotations), `maxItems`.

## Annotations

Write your comments as **child notes** in Zotero (right-click an item → *Add Note*). With
`showNotes: true` they appear below the entry, as plain text. Sync Zotero, reload the page – done.

## Never put an API key here

Everything in this repository, and everything the browser loads, is public. This template therefore only
works with **public** libraries and sends no key. If your library must stay private, fetch it with a script
on your computer (see the Python example in the Lab slides), save the result as a JSON file and publish that
– or ask us about GitHub Actions with a secret.

## Good to know

- The API sends at most 100 items per request; the page asks again until it has everything (*pagination*).
  Large libraries (more than a few hundred items) load slowly – use a collection.
- If the page says *Could not load the library*: check `libraryType`, `libraryId`, `collectionKey`, and that the
  library is public. The message shows the HTTP status code (403 = not public, 404 = wrong ID or key).
- Links must be relative (`about.html`, not `/about.html`), otherwise they break under the repository path.
- Changes take a minute or two; hard-reload with `Cmd/Ctrl + Shift + R`.

## How it works (for the curious)

`app.js` requests
`https://api.zotero.org/{groups|users}/{id}[/collections/{key}]/items/top?include=bib,data&style=…&limit=100&start=…`,
reads the `Total-Results` header to know when to stop, and renders the `bib` field (the citation Zotero formats
for you) plus tags. Notes come from a second request with `itemType=note`, matched by `parentItem`.

Licence: CC0 – take it, change it, no attribution needed.
