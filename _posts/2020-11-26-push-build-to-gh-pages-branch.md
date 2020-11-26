## Git Checkout

git checkout gh-pages
git checkout master --  dist/easyprep/*
mv dist/easyprep/* .
rm -r dist
git add .
git commit -m 'update'
git push origin gh-pages
