<div align="center">
  <h1>Bitbucket Pipelines</h1>
</div>

<div align="center">
  <img alt="Bitbucket logo" width="250px" src="https://wac-cdn.atlassian.com/dam/jcr:e75ffb0e-b3ee-40ca-8659-ecb93675a379/Bitbucket@2x-blue.png" />
  <br />
  <br />
  
  [Site](https://bitbucket.org/product/br/features/pipelines) | [Example](examples/bitbucket-pipelines.yml)
</div>

<br />
<br />

## Create file config:

```sh
touch bitbucket-pipelines.yml
```

## Copy the content and paste in `bitbucket-pipelines.yml`

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

## Example Projects

If you use this CI format, enter your project here.
