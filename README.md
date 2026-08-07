# .github/workflows/snake.yml
# Gera a animação da cobra devorando seus commits.
# Coloque este arquivo no repositório especial SEU-USUARIO/SEU-USUARIO

name: Generate Snake Animation

on:
  schedule:
    - cron: "0 */12 * * *"   # a cada 12 horas
  workflow_dispatch:          # permite rodar manualmente
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg?color_snake=orange&color_dots=#1F2937,#8B2FE8,#00FFF7,#FF2E97,#E6F7FF
            dist/snake-dark.svg?palette=github-dark&color_snake=#00FFF7&color_dots=#161B22,#1F6FEB,#8B2FE8,#FF2E97,#00FFF7

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
