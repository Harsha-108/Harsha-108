# README setup — quick reference

How to wire up the profile README so every live widget actually pulls data.

## 1 · Create the profile repo

GitHub renders the README of a repo whose name matches your username as your **profile page**.

```bash
# create a new public repo named exactly:
Harsha-108/Harsha-108

# drop README.md at its root, then:
git push
```

It'll appear on `github.com/Harsha-108` automatically.

## 2 · Live star / fork / issue badges

The OpenAnalyst project block uses `shields.io/github/...` URLs pointing at `OpenAnalystInc/OpenAnalyst`. If the canonical repo moves, find-and-replace that slug in `README.md`.

## 3 · Snake animation

Drop this workflow at `.github/workflows/snake.yml` in the profile repo. It runs nightly, generates the SVG, and pushes it to the `output` branch — which the README already points at.

```yaml
name: Generate snake
on:
  schedule: [{ cron: "0 0 * * *" }]
  workflow_dispatch:
  push: { branches: [main] }
jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: Platane/snk/svg-only@v3
        with:
          github_user_name: Harsha-108
          outputs: |
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
      - uses: crazy-max/ghaction-github-pages@v3
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Reference: [github.com/Platane/snk](https://github.com/Platane/snk)

## 4 · Latest-activity block (auto-updates)

The `<!-- START_SECTION:activity -->` placeholder in the README is rewritten by this workflow:

```yaml
name: Update activity
on:
  schedule: [{ cron: "0 */6 * * *" }]
  workflow_dispatch:
jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: jamesgeorge007/github-activity-readme@master
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          MAX_LINES: 5
```

Reference: [github.com/jamesgeorge007/github-activity-readme](https://github.com/jamesgeorge007/github-activity-readme)

## 5 · Optional — WakaTime coding hours

Sign up at [wakatime.com](https://wakatime.com), link to GitHub, then add this block under the GitHub stats section:

```html
<img src="https://github-readme-stats.vercel.app/api/wakatime?username=Harsha-108&theme=midnight-purple&hide_border=true&bg_color=0d1117&title_color=9a8cff&text_color=c8c8d4" />
```

## 6 · Profile views counter

The `komarev.com/ghpvc` badge auto-increments every page load — no setup.

---

That's it — push and your profile lights up.
