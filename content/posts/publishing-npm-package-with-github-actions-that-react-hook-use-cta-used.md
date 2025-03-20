---
title: "Publishing NPM package with Github Actions that react-hook-use-cta used"
date: 2025-01-03
categories: 
  - "general"
---

# Overview

I will be covering the following:

- A little about NPM configuration
- Bare minimum package.json
- Github Action workflow

## NPM

Now that you have your bundle build system, start setting NPM to receive your package.  
If you haven't already, please go through the sign-up process via  
https://www.npmjs.com/signup  
before going any further.

Make sure you enable Multi-factor authentication with TOPT for increase security. This tutorial won't work without it. If you missed this part, you can enable it from your profile page.

`https://www.npmjs.com/settings/your-account-name/profile`

## Init

If you haven't created your `package.json` already, you can go through the process through npm init  

```
npm init
```

will ask you questionnaire to fill out before generating the file.  

```
npm init -y
```

create the file without the questionnaire.

## package.json

The following is the bare minimum.  
Please replace references to:

- `your-account/your-package-name` with your github repo location
- `author.name`
- description
- name
- keywords with words that help match your package when searched

```
{
  "author": {
    "name": "Your name"
  },
  "bugs": {
    "url": "https://github.com/your-account/your-package-name/issues"
  },
  "dependencies": {
  },
  "description": "Sweet package",
  "devDependencies": {
    "@parcel/bundler-library": "2.13.3",
    "@parcel/packager-ts": "2.13.3",
    "@parcel/transformer-typescript-types": "2.13.3",
    "@types/node": "22.10.3",
    "@types/react": "19.0.2",
    "parcel": "2.13.3",
    "react": "19.0.0",
    "rimraf": "6.0.1",
    "typescript": "5.7.2"
  },
  "exports": {
    "import": "./dist/esm/index.mjs",
    "require": "./dist/cjs/index.js",
    "types": "./dist/types.d.ts"
  },
  "files": [
    "dist",
    "src"
  ],
  "homepage": "https://github.com/your-account/your-package-name",
  "keywords": [
    "react",
    "typescript"
  ],
  "license": "MIT",
  "name": "your-package-name",
  "main": "dist/cjs/index.js",
  "module": "dist/esm/index.mjs",
  "peerDependencies": {
    "@types/react": ">= 16.8.0 || >= 17.0.0 || >= 18.0.0 || >= 19.0.0",
    "react": ">= 16.8.0 || >= 17.0.0 || >= 18.0.0 || >= 19.0.0"
  },
  "peerDependenciesMeta": {
    "@types/react": {
      "optional": true
    }
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/your-account/your-package-name.git"
  },
  "sideEffects": false,
  "source": "src/index.ts",
  "types": "dist/types.d.ts",
  "scripts": {
    "build": "rimraf dist && parcel build --log-level verbose",
    "postversion": "git push && git push --follow-tags",
    "version": "npm run build && git add -A dist",
    "version:major": "npm version major",
    "version:minor": "npm version minor",
    "version:patch": "npm version patch",
    "version:prerelease": "npm version prerelease --preid=pre"
  },
  "version": "0.0.0",
}
```

Most of this is standard, but I want to emphasize  

```
  "files": [
    "dist",
    "src"
  ],
```

This prevents publishing extra files your package doesn't need include on installation.

In my case, the only files being published are  

```
dist/
src/
LICENSE
README.md
package.json
```

Note:  
`src/` probably isn't needed, but I like to include it for reference.

Note Note:

I like to tag my prereleases with `--preid=pre`, but you can pick something else or not include it.

### Setup Notes

There are extras you can include in your setup, such as eslint, husky, test etc., but I don't want to overwhelm this with info. I can post more info separate from here.

## Github Actions

There are github actions available from their marketplace, but I want to create a workflow you can manage.

`.github/workflows/publish-package.yml`  

```
name: Publish to package registry

on:
  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write 

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "lts/*"
          registry-url: 'https://registry.npmjs.org'
          cache: npm

      - name: Restore cache
        uses: actions/cache@v4
        with:
          path: |
            ~/.npm
          key: ${{ runner.os }}-publish-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-publish-

      - name: Install npm dependencies
        run: npm ci

      - name: Publish to npm
        run: npm publish --provenance --access public
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_AUTH_TOKEN }}
```

A key takeaway is `--provenance --access public` flags.

They are important flags for increase supply-chain security. Without them, this package will not publish from Github.

For more info read Generating provenance statements

### NPM\_AUTH\_TOKEN

https://dev.to/astagi/publish-to-npm-using-github-actions-23fn has a fair explanation on setting up the token. I named the token `NPM_AUTH_TOKEN` to match `env`

### Finishing up

Commit and push changes to your repo. Now you can see your workflow,

![List of workflows](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fe5f8svwca7m2uk5bksq4.png)

select what branch or tag to publish,

![List of tags](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Foqos4oqjfeh0cn5jn2dv.png)

and click Run Workflow

![Run Workflow Button](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fjaw6zv97rf2zdtu91kah.png)

Next, I will include how to publish to Javascript Registry.

Go to Source
