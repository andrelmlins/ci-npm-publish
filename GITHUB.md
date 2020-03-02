<div align="center">
  <h1>Github Actions</h1>
</div>

<div align="center">
  <img alt="Github Actions logo" width="250px" src="https://miro.medium.com/max/300/0*EOBenMCWMDaPdeJL.png" />
  <br />
  <br />
  
  [Site](https://github.com/features/actions) | [Example](examples/.github)
</div>

<br />
<br />

## Create file config:

```sh
mkdir .github
cd .github
mkdir workflows
cd workflows
touch release.yml
```

## Copy the content and paste in `release.yml`

```yaml
name: Publish

on:
  release:
    types: [published]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://registry.npmjs.org/
      - run: yarn install
      - run: npm publish --access public
        env:
          NODE_AUTH_TOKEN: ${{secrets.NPM_TOKEN}}
```
