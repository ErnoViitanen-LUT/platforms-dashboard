# platforms-dashboard

## Project usage

```
yarn install
yarn serve
yarn build
yarn deploy
```

### Build

- yarn build

### Deploy

- setup package.json with project details

```json
{
   "scripts": {
      "serve": "vue-cli-service serve",
      "build": "vue-cli-service build",
      "lint": "vue-cli-service lint",
      "deploy": "gh-pages -d dist"
   },
   "repository": {
      "type": "git",
      "url": "git+https://github.com/ErnoViitanen-LUT/platforms-dashboard.git"
   },
   "author": "",
   "license": "ISC",
   "bugs": {
      "url": "https://github.com/ErnoViitanen-LUT/platforms-dashboard/issues"
   },
   "homepage": "https://ernoviitanen-lut.github.io/platforms-dashboard/"
}
```

- yarn add gh-pages
- yarn deploy

### Run

<https://ernoviitanen-lut.github.io/platforms-dashboard/>
