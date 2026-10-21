# ColoradoDreaming-info

The Colofornia Dreaming wiki ([coloforniadreaming.info](https://coloforniadreaming.info)) is a static site. Pages are Markdown files in `docs/`. The menu and theme are set in `mkdocs.yml`.

## Edit a page

1. Open the page under `docs/` (for example `docs/history/russia-1917-bread-to-cheka.md`).
2. Edit the Markdown. To add a page to the menu, list it in `mkdocs.yml` under `nav`.
3. Commit and push to the `main` branch. That push builds the site and publishes it to GitHub Pages. You can also start the **Deploy wiki to GitHub Pages** workflow by hand from the Actions tab.

To preview locally:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Then open http://127.0.0.1:8000.

One-time hosting setup (GitHub Pages source, custom domain, and DNS) is in `HOSTING.md`.
