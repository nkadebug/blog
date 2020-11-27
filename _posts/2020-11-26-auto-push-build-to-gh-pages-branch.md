```YML
name: Build & Deploy to GH-Pages Branch
on:
  push:
    branches:
      - master
  schedule:
    - cron: "*/30 * * * *"
jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout master
        uses: actions/checkout@v2
      
      - name: Setup NodeJS
        uses: actions/setup-node@v1
      
      - name: Cache node modules
        uses: actions/cache@v2
        env:
          cache-name: cache-node-modules
        with:
          path: ~/.npm
          key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-build-${{ env.cache-name }}-
            ${{ runner.os }}-build-
            ${{ runner.os }}-

      - name: Install Dependencies 
        run: npm i
      
      - name: Run Build Script
        run: npm start
      
      - name: Setup Github Account
        run: |
          git config --global user.email "nka.easyprep@gmail.com"
          git config --global user.name "easyprep"
      
      - name: Git Pull
        run: git pull
      
      - name: Select GH-Pages Branch
        run: |
          if git branch --list "gh-pages"
          then
            echo "Exists - gh-pages branch"
            git checkout gh-pages
          else
            echo "Creating - gh-pages branch"
            git checkout -b gh-pages
            git rm -rf .
            git checkout master -- .gitignore
          fi

      - name: Push To GH-Pages
        run: |
          mv docs/* .
          git add .
          git commit -m "Auto Updated $(date)"
          git push origin gh-pages
          
      - name: Completed
        run: echo "Job Done Successfully"

```
