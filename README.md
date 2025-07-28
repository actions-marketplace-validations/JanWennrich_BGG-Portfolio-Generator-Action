# 🎲 BGG Portfolio Generator Action

**GitHub Action to generate a static portfolio website from a [BoardGameGeek](https://boardgamegeek.com) account.**

This action installs and runs [`jan-wennrich/bgg-portfolio-generator`](https://github.com/JanWennrich/BGG-Portfolio-Generator) to build a personal board game portfolio site directly from your BGG collection.

## 📦 Inputs

| Name           | Description                                               | Required |
|----------------|-----------------------------------------------------------|----------|
| `bgg_username` | The BoardGameGeek username to generate the portfolio for. | ✅ Yes    |


## 🚀 Usage

Here's how to use this action in your workflow:

```yaml
name: Generate BGG Portfolio

on:
  # Run on push to "main"
  push:
    branches: [main]
  # Run every night at 04:00
  schedule:
    - cron: "0 4 * * *" 
  # Allow manual execution
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: JanWennrich/BGG-Portfolio-Generator-Action@main
        with:
          bgg_username: Klabauterjan
      - name: Upload Pages Artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "public"

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## ⚙️ Installation Details

Under the hood, this action:

1. Sets up PHP 8.3.
2. Installs [`jan-wennrich/bgg-portfolio-generator`](https://packagist.org/packages/jan-wennrich/bgg-portfolio-generator) via Composer.
3. Runs the `composer generate` command for the given BGG username every night at 04:00 or when triggered manually.
4. Publishes the generated portfolio to GitHub pages


## 📝 Example Output

The output is a static HTML site listing your BGG games with cover images, ratings, and more.

See a live example at:
📍 [https://janwennrich.github.io/boardgames/](https://janwennrich.github.io/boardgames/)


## 🛠️ Requirements

* Your repository must have GitHub Pages enabled: https://docs.github.com/en/pages/quickstart
* Your workflow must grant `pages: write` and `id-token: write` permissions to deploy.


## 📄 License

GPL-3 or later — use it freely and build awesome board game portfolios!
