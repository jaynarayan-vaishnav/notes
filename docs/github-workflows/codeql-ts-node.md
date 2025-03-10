# CodeQL Scanning

This is a workflow to run a CodeQL scan. It is designed for a TypeScript based Node.js application, and works in tandem with the [LGTM integration](https://lgtm.com).

```yaml
name: "CodeQL"

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  analyse:
    name: Analyse
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        language: ["javascript"]
        node-version: [16.x]
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      - name: Use Node.js v${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - name: Install dependencies
        run: npm ci
      - name: Build files
        run: npm run build
      - name: Setup CodeQL
        uses: github/codeql-action/init@v1
        with:
          languages: ${{ matrix.language }}
      - name: Perform Analysis
        uses: github/codeql-action/analyze@v1
```
