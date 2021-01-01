---
layout: default
date: 2020-11-27 10:44:00
---

Create a file `main.yml` in `.github/workflows/` with following code.

```
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
    env:
      master_branch: master
      gh_pages_branch: gh-pages

    steps:
      - uses: actions/checkout@v2

      - name: Install Dependancies
        run: npm ci

      - name: Run Build Script
        run: npm start

      - name: Setup Github Account
        run: |
          git config user.name "$GITHUB_ACTOR"
          git config user.email "${GITHUB_ACTOR}@bots.github.com"

      - name: Select GH-Pages Branch
        run: |
          git pull --rebase=true
          set +e
          if git branch -a | grep "remotes/origin/${gh_pages_branch}"
          then
            set -e
            echo "Exists: ${gh_pages_branch} Branch"
            git checkout "$gh_pages_branch"
          else
            set-e
            echo "Created: ${gh_pages_branch} Branch"
            git checkout -b "$gh_pages_branch"
            git rm -rf .
            git checkout "$master_branch" -- .gitignore
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
              git push origin "$gh_pages_branch"
          else
              set -e
              echo "No changes to commit"
          fi
          echo "finish"
```
