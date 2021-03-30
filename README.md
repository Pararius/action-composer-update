# Example usage

```yaml
---
name: Update Symfony

on:
  workflow_dispatch:
  schedule:
  - cron:  '0 6 * * *'

jobs:
  update:
    timeout-minutes: 15
    runs-on: ubuntu-latest
    steps:
    - uses: pararius/action-composer-update@v0.0.1
      with:
        working-dir: ./
        composer-token: ${{ secrets.COMPOSER_TOKEN }}
        base-branch: master
```
