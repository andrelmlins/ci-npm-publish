# Continuous Integrations for NPM Publish

Examples of continuous integration for publishing npm packages.

## Contents

- [Bitbucket CI](#bitbucker-ci)
- [CircleCI](#circleci)
- [Github Actions](#github-actions)
- [Gitlab CI](#gitlab-ci)
- [Travis](#travis)

## Platforms

### **Bitbucket CI**

1. Create file config:

```sh
touch bitbucket-pipelines.yml
```

2. Copy the content

```yaml
image: node:latest

pipelines:
  tags:
    - step:
      name: Build
      script:
        - yarn install
        - yarn build
    - step:
      name: Publish
      deployment: production
      script:
        - pipe: atlassian/npm-publish:0.2.0
          variables:
            NPM_TOKEN: $NPM_TOKEN
```

### **CircleCI**

1. Create file config:

```sh
mkdir .circleci
cd .circleci
touch config.yml
```

2. Copy the content

```yaml
version: 2.1

defaults: &defaults
  working_directory: ~/repo
  docker:
    - image: circleci/node:10.16.3

jobs:
  build:
    <<: *defaults
    steps:
      - checkout

      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "package.json" }}
            - v1-dependencies-

      - run: yarn install

      - save_cache:
          paths:
            - node_modules
          key: v1-dependencies-{{ checksum "package.json" }}

      - persist_to_workspace:
          root: ~/repo
          paths: .
  deploy:
    <<: *defaults
    steps:
      - attach_workspace:
          at: ~/repo
      - run:
          name: Authenticate with registry
          command: echo "//registry.npmjs.org/:_authToken=$NPM_TOKEN" > ~/repo/.npmrc
      - run:
          name: Publish package
          command: npm publish

workflows:
  version: 2
  build-deploy:
    jobs:
      - build:
          filters:
            tags:
              only: /^.*/
      - deploy:
          requires:
            - build
          filters:
            tags:
              only: /^.*/
```

### **Github Actions**

1. Create file config:

```sh
mkdir .github
cd .github
mkdir workflows
cd workflows
touch release.yml
```

2. Copy the content

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

### **Gitlab CI**

1. Create file config:

```sh
touch .gitlab-ci.yml
```

2. Copy the content

```yaml
image: node:latest

stages:
  - build
  - deploy

build:
  stage: build
  script:
    - yarn install
    - yarn build
  artifacts:
    expire_in: 1 hour
    paths:
      - dist/
      - node_modules/
  only:
    - tags

deploy:
  stage: deploy
  script:
    - echo "//registry.npmjs.org/:_authToken=\${NPM_TOKEN}" >> $HOME/.npmrc 2> /dev/null
    - npm publish
  only:
    - tags
  dependencies:
    - build
```

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
