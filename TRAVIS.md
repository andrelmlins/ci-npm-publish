# Travis

[Travis CI](https://travis-ci.com/) is a hosted continuous integration service used to build and test software projects hosted at GitHub.

1. Create file config:

```sh
touch .travis.yml
```

2. Copy the content

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

3. Paste content into .travis.yml
