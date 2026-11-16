# LWS overview — W3C Member meeting, 17 Nov 2026

Slides on the W3C [Linked Web Storage WG](https://github.com/w3c/lws-protocol) work products,
written in [Marp](https://marp.app/) Markdown with a small W3C-flavoured theme.

**View the slides: <https://lws-contrib.github.io/lws-overview-w3c-memeber-meeting-17nov-2026/>** ([PDF](https://lws-contrib.github.io/lws-overview-w3c-memeber-meeting-17nov-2026/slides.pdf))

- `slides.md` — the deck
- `themes/w3c.css` — Marp theme (`theme: w3c`)

```sh
npm install
# edit slides.md
npm run preview   # live-reloading server
npm run build     # dist/index.html
npm run pdf       # dist/slides.pdf
```

Or open `slides.md` in VS Code with the Marp extension; point its
`markdown.marp.themes` setting at `./themes/w3c.css`.

Pushing to `main` rebuilds the HTML and PDF and publishes them to GitHub Pages
(`.github/workflows/pages.yml`).
