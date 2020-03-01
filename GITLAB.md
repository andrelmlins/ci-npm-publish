<div align="center">
  <h1 style="margin:0;border:0">Gitlab CI</h1>
  <hr />
  <img width="250px" src="https://about.gitlab.com/images/press/logo/png/gitlab-logo-gray-rgb.png" />
</div>

---

1. Create file config:

```sh
touch .gitlab-ci.yml
```

2. Copy the content and paste in `.gitlab.yml`

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
