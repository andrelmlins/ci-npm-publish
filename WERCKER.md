<div align="center">
  <h1>Wercker</div>
</div>

<div align="center">
  <img alt="Wercker logo" width="250px" src="https://res-4.cloudinary.com/crunchbase-production/image/upload/c_lpad,h_256,w_256,f_auto,q_auto:eco/v1481640407/xcmsrvor9cvaqqfh0udh.png" />
  <br />
  <br />
  
  [Site](https://app.wercker.com/) | [Example](examples/wercker.yml)
</div>

<br />
<br />

## Create file config:

```sh
touch wercker.yml
```

## Copy the content and paste in `wercker.yml`

```yaml
box: node

build:
  steps:
    - script:
      name: install dependencies
      code: |
        yarn install

    - script:
      name: build project
      code: |
        yarn build

    - script:
      name: publish
      code: |
        echo "//registry.npmjs.org/:_authToken=\${NPM_TOKEN}" >> $HOME/.npmrc 2> /dev/null
        npm publish
```