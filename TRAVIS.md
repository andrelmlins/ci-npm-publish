<div align="center">
  <h1 style="margin:0;border:0">Travis CI</h1>
  <hr />
  <img width="250px" src="https://miro.medium.com/max/600/1*VXdK53mBfr27iT8LiHNAbg.png" />
</div>

---

1. Create file config:

```sh
touch .travis.yml
```

2. Copy the content and paste in `.travis.yml`

```yaml
language: node_js
node_js:
  - "10"
cache: yarn

install: yarn
script:
  - echo "Not found tests"

jobs:
  include:
    - stage: npm release
      if: tag IS present
      node_js: "10"
      script: yarn build
      deploy:
        provider: npm
        api_key: $NPM_TOKEN
        skip_cleanup: true
        on:
          tags: true
```
