# Evermore Arcade — HTML game shelf

Every game lives in its own folder under `games/`. The home page reads that
folder and builds the list itself, so adding or removing a game is just adding
or removing a folder. There's no list to keep in sync.

```
├── index.html          ← the shelf (auto-lists everything in games/)
├── .nojekyll           ← stops GitHub mangling folders that start with _
└── games/
    ├── _template/      ← copy this to start a new game (leading _ hides it)
    ├── bingo/
    │   ├── index.html  ← required: the entry point
    │   └── meta.json   ← optional: title, description, thumbnail
    └── pool/
        └── index.html
```

## One-time setup

1. Create a new repository on GitHub. **Public** — GitHub Pages needs a paid
   plan to serve from a private repo.
2. Upload these files to it (drag the whole folder onto the repo's upload page).
3. In the repo: **Settings → Pages → Source: Deploy from a branch**, pick
   `main` and `/ (root)`, then **Save**.
4. Wait about a minute. Your shelf is live at
   `https://YOUR-USERNAME.github.io/YOUR-REPO/`

The shelf figures out your username and repo name from that URL. If you later
put it on a custom domain, fill in `owner` and `repo` in the `CONFIG` block at
the top of `index.html`.

## Adding a game

Copy `games/_template/`, rename it to your game's slug (lowercase, hyphens —
`trick-shot-pool`), and put your game's `index.html` inside. Refresh the shelf.

To give it a proper title and blurb, add a `meta.json` next to the `index.html`:

```json
{
  "title": "Trick Shot Pool",
  "desc": "Angle-based pool with a shot clock.",
  "thumb": "cover.png"
}
```

`thumb` is a file inside that same game folder. Leave it out and the shelf draws
a coloured tile from the game's name instead — consistent every time, no asset
needed.

## Editing a game

- **In the browser** — open the file on GitHub, click the pencil icon, edit,
  then **Commit changes**. Best for quick tweaks.
- **GitHub Desktop** — clone the repo once, then edit files in your normal
  editor and hit **Commit** → **Push**. Best for real work.
- **Command line** —
  ```bash
  git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
  cd YOUR-REPO
  # edit files
  git add -A && git commit -m "Update pool physics" && git push
  ```

Changes go live 30–60 seconds after the push.

## Deleting a game

Delete its folder. On the web: open any file in the folder → trash icon →
commit. From the command line: `git rm -r games/pool && git commit -m "Remove
pool" && git push`. The shelf stops showing it on the next refresh.

## Things worth knowing

- Each game folder is fully self-contained. Reference assets with relative paths
  (`./sprites/ball.png`), never absolute ones (`/sprites/ball.png`) — absolute
  paths break under the `/YOUR-REPO/` prefix that Pages adds.
- Anything starting with `_` or `.` is skipped by the shelf, so `_template`
  and any work-in-progress folders stay hidden without being deleted.
- The folder listing uses GitHub's public API, which allows 60 requests an hour
  per IP address. If you ever hit that ceiling, drop a `games.json` at the repo
  root — `["bingo","pool","skeeball"]` — and the shelf reads that instead, with
  no API call at all.
- Games hosted here run client-side only. Nothing here handles real-money play,
  accounts, or payouts.
