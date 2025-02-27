---
title: Preview Mode
description: Live Preview for target static with the preview mode
---

With Nuxt.js and full static you can now use live preview out of the box which will call your API or your CMS so you can see the changes live before deploying.

<alert> Only available when using [`target:static`](/docs/features/deployment-targets#static-hosting) </alert>

The preview mode will automatically refresh the page data as it uses `$nuxt.refresh` under the hood and therefore calls nuxtServerInit, asyncData and fetch on the client side.

In order to activate live preview you will need to add the following plugin:

```js{}[plugins/preview.client.js]
export default function ({ query, enablePreview }) {
  if (query.preview) {
    enablePreview()
  }
}
```

<alert>
EnablePreview is only available in the context object of plugins. Previews are handled client-side and
thus the plugin should be run on the client: preview.client.js
</alert>

```js{}[nuxt.config.js]
export default {
  plugins: ['plugins/preview.client.js']
}
```

Once you have added the plugin you can now generate your site and serve it.

<code-group>
<code-block label="npx" active>

```bash
npx nuxt generate
npx nuxt start
```

</code-block>
<code-block label="Yarn" >

```bash
yarn generate
yarn start
```

  </code-block>
</code-group>

Then you can see your preview page by adding the query param to the end of the page you want to see once:

```js
?preview=true
```

<alert>
enablePreview should be tested locally with yarn start and not yarn
dev
</alert>

### Previewing pages that are not yet generated

For pages that are not yet generated, SPA fallback will still call the API before showing the 404 page as these pages exist on the API but are not generated yet.

If you have set a validate hook, you will probably need to modify it so that it doesn't redirect to the 404 page in preview mode.

```js
validate({ params, query }) {
  if (query.preview) {
    return true
}
```

### Passing data to enablePreview

You can pass data to the `enablePreview` function. That data will then be available on the `$preview` context helper and on `this.$preview`.

### What's next

<alert type="next">

Check out the [Directory Structure book](/docs/directory-structure/nuxt)

</alert>
