# Example usage

```yaml
---
name: Update PHP

on:
  pull_request:

env:
  DOCKER_BUILDKIT: 1
  COMPOSER_CACHE_DIR: /tmp/composer-cache

jobs:
  update:
    timeout-minutes: 15
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2.3.4

    - name: Setup cache
      uses: actions/cache@v2.1.4
      with:
        path: ${{ COMPOSER_CACHE_DIR }}
        key: ${{ runner.os }}-${{ hashFiles('**/composer.lock') }}

    - uses: Pararius/action-composer-update@v0.0.2
      id: composer-update
      with:
        working-dir: ./
        composer-token: ${{ secrets.COMPOSER_TOKEN }}
        base-branch: master

    - name: Current date
      run: echo "DATE=$(date +%Y%m%d)" >> $GITHUB_ENV

    - name: Create pull request
      uses: peter-evans/create-pull-request@v3.8.2
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
        branch: php-update-${{ env.DATE }}
        branch-suffix: random
        commit-message: '[ci] updated PHP dependencies'
        title: Updated PHP dependencies
        body: ${{ steps.composer-update.outputs.diff }}
        labels: dependencies
        base: master
```
