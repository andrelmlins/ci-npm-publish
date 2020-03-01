# Continuous Integrations for NPM Publish

Examples of continuous integration for publishing npm packages.

## Contents

- [Bitbucket CI](#bitbucker-ci)
- [CircleCI](#circleci)
- [Gitlab CI](#gitlab-ci)
- [Github Actions](#github-actions)
- [Jenkins](#jenkins)
- [Travis](#travis)

## Platforms

### **Bitbucket CI**

### **CircleCI**

### **Gitlab CI**

### **Github Actions**

### **Jenkins**

### **Travis**

[Travis CI](https://travis-ci.com/) is a hosted continuous integration service used to build and test software projects hosted at GitHub.

1. Create `.travis.yml`file:

```sh
nano .travis.yml
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
      script: yarn build # Insert in react|vue|svelte|angular|stencil packages
      deploy:
        provider: npm
        email: $EMAIL_ADDRESS
        api_key: $AUTH_TOKEN
        skip_cleanup: true
        on:
          tags: true
```

3. Paste content into .travis.yml

## How to Contribute

## License

Continuous Integrations for NPM Publish is open source software [licensed as MIT](https://github.com/andrelmlins/ci-npm-publish/blob/master/LICENSE).
