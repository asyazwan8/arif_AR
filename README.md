# AR Viewer

A minimal, cross-platform AR web app for viewing a single 3D model on iPhone
and Android. Built with [Vite](https://vitejs.dev/) (vanilla JS/HTML/CSS) and
Google's [`<model-viewer>`](https://modelviewer.dev/) web component loaded from
a CDN. No framework, no backend, no API keys.

---

## ⚠️ Three files are missing on purpose

The viewer references three model assets that are **not** included in this repo.
You must add them to `public/models/` yourself before the model or the AR button
will show anything:

| File path                  | What it is                                              |
| -------------------------- | ------------------------------------------------------- |
| `public/models/model.glb`  | The 3D model (used on Android + desktop / WebXR)        |
| `public/models/model.usdz` | iOS Quick Look version (used on iPhone / iPad)          |
| `public/models/poster.webp`| Placeholder image shown before the model loads          |

Until these exist, `<model-viewer>` will render its poster/loading state and the
**AR button won't have anything to place**. The file _names_ above are exact —
they must match what's referenced in `index.html`.

> Tip: you can generate a `.usdz` from a `.glb` with Reality Converter (macOS)
> or `usdzconvert`. The `poster.webp` is just a still preview image of the model.

---

## Run locally

```bash
npm install
npm run dev
```

Then open the printed `http://localhost:5173` in a desktop browser to confirm
the model loads and rotates.

```bash
npm run build     # production build into dist/
npm run preview   # preview the production build
```

---

## Testing AR on a real phone (HTTPS required)

WebXR and camera access **only work over HTTPS** (plain `http://localhost` will
not trigger AR on a phone). To test AR on a real device locally, use one of:

- **`vercel dev`** — runs the project with a Vercel-style local server, or
- **ngrok** — `ngrok http 5173` to expose your local dev server over HTTPS, then
  open the HTTPS URL on your phone.

Plain `localhost` over your LAN is **not** enough for AR.

---

## Deploying

Final hosting flow is **GitHub → Vercel**:

1. Push this repo to GitHub.
2. Import the repo into Vercel and deploy.

HTTPS is automatic on Vercel, so AR works out of the box once deployed.

`vercel.json` adds a header rule that serves any `.usdz` file with
`Content-Type: model/vnd.usdz+zip`, which iOS Quick Look requires.

---

## A note on model file size

GitHub flags files over ~50 MB and **blocks** files over 100 MB. If your
`model.glb` is large (roughly 40–50 MB or more), compress it with **Draco** or
**Meshopt** geometry compression before committing — that's far simpler than
setting up Git LFS for a single asset.
