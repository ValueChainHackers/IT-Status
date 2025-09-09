# VCH Status Board

A lightweight, auto-updating **status dashboard** for the Value Chain Hackers infrastructure.  
Built with [Quarto Dashboards](https://quarto.org/docs/dashboards/) and published automatically via **GitHub Actions** + **GitHub Pages**.


Task is bulletpoint list; 



In october we'll have that.
In september we'll have that; 

We should would on what we look like.


---

## ✨ Features

- **Value box tiles** for quick at-a-glance health indicators (🟢/🟠/🔴).
- **Service checks** for OpenWebUI, n8n, Supabase/Postgres, Qdrant, etc.
- **Recent incidents table** (optional feed from JSON or CSV).
- **Kiosk/TV friendly** design (dark theme, auto-refresh via scheduled builds).
- **Static hosting**: no servers required — runs fully on GitHub Pages.

---

## 🏗 Project Structure

```
.
├── _quarto.yml         # Quarto project config (site + theme)
├── status.qmd          # Main dashboard file (tiles + table)
├── .github/
│   └── workflows/
│       └── publish.yml # GitHub Actions workflow (build + deploy)
```

- `_quarto.yml` sets up the site config (theme, layout, output directory).
- `status.qmd` contains the Quarto dashboard (Python chunks probe services).
- `publish.yml` handles scheduled builds and pushes the site to GitHub Pages.

---

## 🚀 Getting Started

### 1. Install Quarto locally (optional)
If you want to preview before deploying:

```bash
# Install Quarto (see https://quarto.org/docs/get-started/)
quarto check
quarto render
```

Preview locally:

```bash
quarto preview
```

Open `http://localhost:4200/` in your browser.

### 2. Configure status endpoints
The dashboard pulls JSON from your infrastructure.  
Make sure your Traefik setup exposes simple read-only endpoints like:

```json
# Example: https://demo.valuechainhackers.xyz/status/openwebui
{ "ok": true, "status": 200, "ms": 41 }
```

You can adapt [`checkTraefic.sh`](./checkTraefic.sh) for automated health probes.

### 3. GitHub Pages deployment
- Go to **Settings → Pages** in your repo, set branch = `gh-pages` (root).
- GitHub Actions workflow (`.github/workflows/publish.yml`) will:
  - Build the site on push to `main`.
  - Rebuild every 15 minutes via cron.
  - Publish to `gh-pages`.

Your live status board will be available at:

```
https://<username>.github.io/<repo>/
```

---

## ⚙️ GitHub Actions

The workflow:
- Installs Quarto + Python.
- Installs `requests`, `pandas`, `shiny` (for tiles).
- Renders `status.qmd` → `_site/`.
- Publishes `_site/` to `gh-pages`.

Adjust the cron schedule in `publish.yml`:

```yaml
schedule:
  - cron: "*/15 * * * *"   # every 15 minutes
```

---

## 📊 Example Tiles

- **OpenWebUI** latency (ms).
- **n8n** HTTP status.
- **Qdrant** health endpoint.
- **Supabase** status.
- **Last build time** (from GitHub Actions run).

---

## 🔒 Notes

- GitHub Actions can only fetch **public endpoints**.  
  If services are private, expose a minimal read-only status page via Traefik.
- For near real-time dashboards (<10s refresh), consider Quarto + Shiny Server.  
  GitHub Pages is best for **static 5–15 min refresh cycles**.

---

## 📚 References

- [Quarto Dashboards](https://quarto.org/docs/dashboards/)
- [Quarto + GitHub Actions](https://quarto.org/docs/publishing/github-pages.html)
- [Value Chain Hackers Architecture](./architecture.md)
- [Traefik Health Check Script](./checkTraefic.sh)
