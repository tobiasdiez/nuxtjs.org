---
title: External Packages Plugin
description: In this example we show how to use a plugin with an external package - axios
csb_link: https://codesandbox.io/embed/github/nuxt-academy/examples/tree/master/plugins/external-packages-plugin?fontsize=14&hidenavigation=1&module=%2Fplugins%2Faxios.js&theme=dark&view=editor
---

<example-intro></example-intro>

`plugins/axios.js` intercepts the `$axios` call using the `onError()` function.

`pages/index.vue` uses `$axios` to fetch our data from an API.

`pages/mountains/_slug.vue` uses `$axios` to fetch our data from an API with the id coming from route params.

`pages/404.vue` is the page that is called when there is an error.

`nuxt.config.js` contains the `module` property and `plugin` property to register our module and plugin.

`package.json` shows our module `@nuxtjs/axios` has been installed.

<alert type="next">

Learn more in the Directory Structure book in the [plugins](/docs/directory-structure/plugins#external-packages) chapter.

</alert>

<code-sandbox :src="csb_link"></code-sandbox>
