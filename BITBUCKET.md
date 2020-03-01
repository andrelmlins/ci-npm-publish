# Bitbucket Pipelines

1. Create file config:

```sh
touch bitbucket-pipelines.yml
```

2. Copy the content and paste in `bitbucket-pipelines.yml`

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
