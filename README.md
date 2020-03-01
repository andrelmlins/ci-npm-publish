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

1. Create file config:

```sh
mkdir .circleci
cd .circleci
touch config.yml
```

2. Copy the content

```yaml
version: 2

jobs:
  deploy:
    working_directory: ~/repo
    docker:
      - image: circleci/node:8.9.1
    steps:
      - attach_workspace:
          at: ~/repo
      - run: yarn install
      - run: yarn build
      - run:
          name: Authenticate with registry
          command: echo "//registry.npmjs.org/:_authToken=$NPM_TOKEN" > ~/repo/.npmrc
      - run:
          name: Publish package
          command: npm publish

workflows:
  version: 2
  test-deploy:
    jobs:
      - deploy:
          filters:
            tags:
              only: /.*/
```

### **Gitlab CI**

1. Create file config:

```sh
touch .gitlab-ci.yml
```

2. Copy the content

```yaml
image: node:latest

stages:
  - deploy

deploy:
  stage: deploy
  script:
    - yarn install
    - yarn build
    - echo "//registry.npmjs.org/:_authToken=\${NPM_TOKEN}" >> $HOME/.npmrc 2> /dev/null
    - npm publish
```

### **Github Actions**

### **Jenkins**

### **Travis**

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

## How to Contribute

## License

Continuous Integrations for NPM Publish is open source software [licensed as MIT](https://github.com/andrelmlins/ci-npm-publish/blob/master/LICENSE).
