name: Generate Snake Animation

on:
  workflow_dispatch:

  schedule:
    - cron: "17 0 * * *"

  push:
    branches:
      - main
    paths:
      - ".github/workflows/snake.yml"

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    permissions:
      contents: write

    steps:
      - name: Generate contribution animations
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg
            dist/github-snake-dark.svg?palette=github-dark&color_snake=cyan

      - name: Publish animations
        uses: crazy-max/ghaction-github-pages@v5
        with:
          target_branch: profile-animations
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
