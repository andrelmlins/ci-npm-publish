# Travis

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
        email: $EMAIL_ADDRESS
        api_key: $NPM_TOKEN
        skip_cleanup: true
        on:
          tags: true
```
