---
title: 'The css Property'
description: Nuxt.js lets you define the CSS files/modules/libraries you want to set globally (included in every page).
category: api-configuration
---

> Nuxt.js lets you define the CSS files/modules/libraries you want to set globally (included in every page).

In case you want to use `sass` make sure that you have installed `sass` and `sass-loader` packages. If you didn't just

```sh
npm install --save-dev sass sass-loader fibers
```

- Type: `Array`
  - Items: `string`

```js{}[nuxt.config.js]
export default {
  css: [
    // Load a Node.js module directly (here it's a Sass file)
    'bulma',
    // CSS file in the project
    '@/assets/css/main.css',
    // SCSS file in the project
    '@/assets/css/main.scss'
  ]
}
```

Nuxt.js will automatically guess the file type by its extension and use the appropriate pre-processor loader for webpack. You will still need to install the required loader if you need to use them.

### Style Extensions

You can omit the file extension for CSS/SCSS/Postcss/Less/Stylus/... files listed in the css array in your nuxt config file.

```js{}[nuxt.config.js]
export default {
  css: ['~/assets/css/main', '~/assets/css/animations']
}
```

<alert>

If you have two files with the same name eg. `main.scss` and `main.css`, and don't specify an extension in the css array entry, eg. `css: ['~/assets/css/main']`, then only one file will be loaded depending on the order of `styleExtensions`. In this case only the `css` file will be loaded and the `scss` file will be ignored because `css` comes first in the default `styleExtension` array.

</alert>

Default order: `['css', 'pcss', 'postcss', 'styl', 'stylus', 'scss', 'sass', 'less']`
