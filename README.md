## Storing Custom Icons as SVGs in Quasar Framework

The Quasar UI framework's official documentation suggests two methods for storing custom SVG icons:

## Method 1: Storing SVG Files as Static Assets

Using this method, you can store SVG files as static assets and reference them. Example in the [documentation](https://quasar.dev/vue-components/icon#svg-use-way/)

## Method 2: Storing SVG Icons as Strings

Alternatively, you may want to store SVG icons as strings, similar to how the framework handles standard library icon sets

```
<script setup>
import { matMenu } from '@quasar/extras/material-icons'
// OR
import { appGitHub } from "./icons"; // custom icons for your app
</script>

<template>
  <div>
    <q-icon :name="matMenu" />
    <q-icon :name="appGitHub" />
  </div>
</template>
```

Quasar uses its string format in the case of q-btn and q-icon (:icon and :name), and you can get the correct format for these components using the conversion from SVG in [my solution](https://antoninik.github.io/svg-encoder-for-q-icon-quasar/)

This code is based on the official [parser](https://github.com/quasarframework/quasar/blob/dev/extras/build/utils/index.js/) of the Quasar

## Technology stack

- **UI**: [`Quasar`](https://quasar.dev/)
- **Lint**: [`eslint`](https://eslint.org/), [`prettier`](https://prettier.io/)

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run dev
```

### Compiles and minifies for production

```
npm run build
```
