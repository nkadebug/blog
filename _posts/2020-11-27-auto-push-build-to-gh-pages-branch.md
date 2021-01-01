---
layout: default
date: 2020-11-27 10:44:00
---

Create a file `main.yml` in `.github/workflows/` with following code.

```YML
name: Build & Deploy
on:
  push:
    branches:
      - master
  schedule:
    - cron: '*/10 * * * *'
jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    timeout-minutes: 9

    steps:
      - uses: actions/checkout@v2

      - name: Install Dependancies
        run: npm ci

      - name: Run Build Script
        run: npm start

      - name: Setup Github Account
        run: git config user.name `${{ github.actor }}`

      - name: Select GH-Pages Branch
        run: |
          git pull --rebase=true
          set +e
          if git branch -a | grep 'remotes/origin/gh-pages'
          then
            set -e
            echo 'Exists: gh-pages Branch'
            git checkout "gh-pages"
          else
            set-e
            echo 'Created: gh-pages Branch'
            git checkout -b "gh-pages"
            git rm -rf .
            git checkout master -- .gitignore
          fi

      - name: Push To GH-Pages Branch
        run: |
          cp -r dist/* .
          git add .
          set +e
          if git status | grep 'new file\|modified'
          then
              set -e
              git commit -m "Auto Updated $(date)"
              git push origin gh-pages
          else
              set -e
              echo "No changes since last run"
          fi
          echo "finish"
```
