# Contributing to These Docs

This page explains how to propose changes to the documentation. If you are fixing a typo or a short paragraph, use **“Edit this page”** in the header. For substantial edits (new pages, images, navigation changes), follow the local workflow below.

> We **do not** use versioned deployments (no `mike`). Maintainers publish a single latest site with `mkdocs gh-deploy`.

---

## Small edits in the browser (fastest)

1. Click **Edit this page** on the top-right.
2. GitHub will fork the repository to your account if needed.
3. Make your Markdown changes.
4. Click **Propose changes** → **Create pull request (PR)**.
5. A maintainer will review and merge.

!!! tip
    Use this path for quick fixes. For images, new pages, or previewing layout/MathJax, use the local workflow.

---

## Full local workflow (≈5 minutes)

### 1) Fork and clone

- Repository: <https://github.com/cermas/cermas-mkdocs>
- Click **Fork**, then clone **your fork**:
  ```bash
  git clone https://github.com/<your-username>/cermas-mkdocs.git
  cd cermas-mkdocs
  git remote add upstream https://github.com/cermas/cermas-mkdocs.git
  ```

### 2) Create a virtual environment (Python 3.12.3)

=== "macOS / Linux"
    ```bash
    python3 -m venv .venv
    source .venv/bin/activate
    python --version          # expect 3.12.3
    pip install --upgrade pip
    pip install mkdocs-material
    ```

=== "Windows (PowerShell)"
    ```powershell
    py -3.12 -m venv .venv
    .\.venv\Scripts\Activate.ps1
    python --version          # expect 3.12.3
    python -m pip install --upgrade pip
    pip install mkdocs-material
    ```

!!! note
    Dependencies are intentionally minimal. Any extra plugins required by the site are declared in `mkdocs.yml`.

### 3) Run a live preview

```bash
mkdocs serve
```

Open <http://127.0.0.1:8000>. The site reloads automatically when you save files.

### 4) Add or edit content

- **Where to put files**
  - Pages: place Markdown under `docs/` (use folders to organize).
  - Images/figures: `docs/assets/` and reference as `![caption](assets/my-figure.png)`.

- **Math (MathJax via Arithmatex)**
  - Inline: `$\omega_c t$`
  - Display:
    ```latex
    $$
    \mathcal{Z}(\beta) = \mathrm{Tr}\, e^{-\beta \hat H}
    $$
    ```

- **Admonitions**
  ```markdown
  !!! tip "Quick tip"
      Use `mkdocs serve` while writing so you can see live changes.
  ```

- **Tabbed content**
  ```markdown
  === "Python"
  ```python
  print("hello")
  ```
  === "Bash"
  ```bash
  echo hello
  ```
  ```

- **Code blocks**
  Use language hints for syntax highlighting: ```python, ```bash, ```yaml, etc.

### 5) Update the navigation

Add your new page to `mkdocs.yml` under `nav:` so it appears in the sidebar:

```yaml
nav:
  - Home: index.md
  - DFT:
      - DFT Overview: DFT/DFT.md
      - New Tutorial: DFT/new-tutorial.md
```

!!! warning
    YAML is whitespace-sensitive. Use two spaces for indentation. Ensure file paths match exactly.

### 6) Validate locally (recommended)

```bash
mkdocs build --strict
```

This flags broken links, missing images, and undefined nav entries.

### 7) Commit and open a Pull Request

```bash
git checkout -b docs/<short-topic>
git add -A
git commit -m "docs: add <short-topic> tutorial"
git push origin docs/<short-topic>
```

Then open a PR on GitHub from your branch into `cermas:main`.

---

## Writing guidelines

- **Headings:** one `#` title per page, then `##`, `###`, …
- **Tone:** clear, concise, instructional. Prefer task-oriented sections (“Install…”, “Run…”, “Interpret…”).
- **Filenames:** lowercase with hyphens, e.g. `installation-aocl.md`. Avoid spaces in filenames.
- **Links:** prefer relative links, e.g. `[CASTEP runner](../DFT/Python%20Runner.md)`.
- **Images:** keep ≤ 1600 px wide, compressed; place under `docs/assets/`.
- **Code:** runnable, minimal, and consistent with current instructions.
- **References:** when citing literature, include a short context sentence and a link.

---

## How we deploy (maintainers)

We publish the **latest** site with **GitHub Pages** using MkDocs’ built-in deploy command.

### First-time setup (one-time in the repo)

1. Ensure Pages is enabled: **Settings → Pages**.
2. Source: **Deploy from a branch**, pick the `gh-pages` branch after first deploy.

### Regular deployment (after PRs are merged)

```bash
mkdocs gh-deploy --force
```

- This builds the site and pushes to the `gh-pages` branch.
- The site is served at the URL configured in `site_url` (e.g., `https://cermas.github.io/cermas-mkdocs/`).
- If the site doesn’t update, clear the browser cache or re-run with `--clean`:
  ```bash
  mkdocs gh-deploy --force --clean
  ```

!!! info
    We do not keep multiple hosted versions. If you want to reference historical states, use **git tags** and a `CHANGELOG.md` page.

---

## Troubleshooting

??? note "The site does not update when I add a page"
    - Ensure you **added the file to `nav:`** in `mkdocs.yml` and that the path is correct.
    - Run `mkdocs build --strict` to surface errors.

??? note "`mkdocs: command not found`"
    - Activate the virtual environment first.
    - Verify installation: `pip show mkdocs-material`.

??? note "Port 8000 already in use"
    - Stop the previous server or run `mkdocs serve -a 127.0.0.1:8001`.

??? note "Math is not rendering"
    - Use `$$ ... $$` for display math and `$ ... $` inline.
    - Do not indent math blocks unless they are inside a fenced code block.

---

## Thank you

Every contribution helps. If you are unsure about scope or placement, open a **Draft PR** and ask for feedback.
