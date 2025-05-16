name: Newman extra-html-report deploy to github page 

on:
  push:
  workflow_dispatch:
  schedule:
    - cron: '00 18 * * *'

jobs:
  test-api:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Create dir
        run: |
          mkdir -p testResults

      - name: Install Node
        uses: actions/setup-node@v4
        with:
          node-version: '16.x'
      
      - run: npm i npm@latest
      - run: npm i
      - name: Install newman
        run: |
          npm install -g newman
          npm install -g newman-reporter-htmlextra
      
      - name: Run POSTMAN collection
        run: |
          newman run "petStore.postman_collection.json" -e "petStore.postman_environment.json" -r cli,htmlextra --reporter-htmlextra-export testResults/index.html || true

      - name: Output the results
        uses: actions/upload-artifact@v4
        with:
          name: Reporter
          path: testResults

      - name: Test marketplace action
        uses: PavanMudigonda/html-reporter-github-pages@v1.0
        id: test-report
        with:
          test_results: testResults
          gh_pages: gh-pages
          results_history: results-history

      - name: Publish Github Pages
        if: ${{ always() }}
        uses: peaceiris/actions-gh-pages@v3.8.0
        with:
          github_token: ${{ secrets.ci_token }}
          publish_branch: gh-pages
          publish_dir: results-history
          keep_files: true 
