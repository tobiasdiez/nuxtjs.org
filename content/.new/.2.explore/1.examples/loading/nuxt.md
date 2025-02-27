---
title: Customize Nuxt Loading
description: Create a custom loading component to replace the default loader
csb_link: https://codesandbox.io/embed/github/nuxt-academy/examples/tree/master/loading/customize-nuxt-loading?fontsize=14&hidenavigation=1&module=%2Fnuxt.config.js&theme=dark&view=editor
---

<example-intro></example-intro>

`nuxt.config.js` contains the `loading` property which modifies the default loader

`pages/loading.vue` programmatically starts the loader so we force the page to take 2 seconds to load

<alert type="next">

Learn more in the Features book in the [Loading](/docs/features/loading) chapter.

</alert>

<code-sandbox :src="csb_link"></code-sandbox>
