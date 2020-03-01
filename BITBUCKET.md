<div align="center">
  <h1 style="margin:0;border:0">Bitbucket Pipelines</h1>
  <hr />
  <img width="250px" src="https://wac-cdn.atlassian.com/dam/jcr:e75ffb0e-b3ee-40ca-8659-ecb93675a379/Bitbucket@2x-blue.png" />
</div>

---

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
