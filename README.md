# site-gpsa_coaches
GPSA Coach Listings — static site hosted on GitHub Pages at [coaches.gpsaswimming.org](https://coaches.gpsaswimming.org)

## About
Single-page site listing open coaching positions for GPSA member teams. The page is embedded via iframe in the SwimTopia CMS site.

Content is managed through a single YAML file. Pushing to `main` automatically builds and deploys the site — no server access or Docker required.

---

## How to Update Coach Postings

All posting content lives in **`data/postings.yaml`**. That is the only file you need to edit for routine updates.

### Add a new posting
Add a new entry at the bottom of the `postings:` list:

```yaml
  - id: your-team-name          # URL-safe, no spaces (used as HTML anchor)
    team: Your Team Name        # Display name shown on the page
    active: true
    description:
      - First paragraph of text.
      - Second paragraph of text.
    positions:
      - title: Head Coach
        duties:
          - Duty one
          - Duty two
    qualifications:
      - Qualification one
      - Qualification two
    contact:
      intro: "To apply, please contact"
      email: coach@example.com
      link_text: Coach Name      # optional — defaults to the email address
      suffix: "."
      closing: Optional closing paragraph.
```

### Remove or hide a posting
Set `active: false`. The content is preserved for future reuse — nothing is lost.

```yaml
    active: false
```

### Update an existing posting
Find the team's entry by its `id` or `team` name and edit the relevant fields.

### Image-only posting
Some teams (e.g. Poquoson) use a flyer image instead of structured text:

```yaml
  - id: team-name
    team: Team Name
    active: true
    image: images/your-flyer.jpg
```

Upload the image to `static/images/` and reference it as shown. All other fields are optional.

---

## Deployment

Pushing any change to `main` triggers the GitHub Actions workflow (`.github/workflows/deploy.yml`), which:

1. Installs Python dependencies (`pyyaml`, `jinja2`)
2. Runs `build.py` — reads `data/postings.yaml`, renders `templates/index.html.j2`, copies `static/` assets and `CNAME` into `dist/`
3. Deploys `dist/` to GitHub Pages via the native Pages deployment pipeline

The live site updates within ~1 minute of a push. Deployment status is visible under the **Actions** tab and the **Environments → github-pages** sidebar in the repo.

---

## Local Development

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python build.py
# open dist/index.html in a browser to preview
```

---

## Repository Structure

```
├── data/
│   └── postings.yaml          # ← edit this to manage postings
├── static/
│   ├── styles.css
│   └── images/
├── templates/
│   └── index.html.j2          # Jinja2 template (rarely needs editing)
├── build.py                   # build script
├── requirements.txt
├── CNAME                      # custom domain: coaches.gpsaswimming.org
└── .github/
    └── workflows/
        └── deploy.yml         # CI/CD pipeline
```
