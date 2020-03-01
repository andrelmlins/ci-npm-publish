# CircleCI

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
