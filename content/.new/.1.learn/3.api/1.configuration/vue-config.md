---
title: 'The vue.config Property'
description: A config object for Vue.config
category: api-configuration
---

- Type: `Object`
- Default: `{ silent: !isDev, performance: isDev }`

> The vue.config property provides a direct configuration bridge for the `Vue.config`

**Example**

```js{}[nuxt.config.js]
export default {
  vue: {
    config: {
      productionTip: true,
      devtools: false
    }
  }
}
```

This configuration will lead to the following Vue.config:

```js
Vue.config.productionTip // true
Vue.config.devtools // false
Vue.config.silent // !isDev [default value]
Vue.config.performance // isDev [default value]
```

To learn more about the `Vue.config` API, check out the [official Vue documentation](https://vuejs.org/v2/api/#Global-Config)
