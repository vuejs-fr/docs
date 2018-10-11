---
title: "API: The hooks Property (En)"
description: Hooks are listeners to Nuxt events that are typically used in Nuxt modules, but are also available in `nuxt.config.js`.
---

# The hooks Property (En)

- Type: `Object`

> Hooks are [listeners to Nuxt events](/api/internals) that are typically used in Nuxt modules, but are also available in `nuxt.config.js`. [Learn More](/api/internals)

<p style="width: 294px;position: fixed; top : 64px; right: 4px;" class="Alert Alert--orange"><strong>⚠Cette page est actuellement en cours de traduction française. Vous pouvez repasser plus tard ou <a href="https://github.com/vuejs-fr/nuxt" target="_blank">participer à la traduction</a> de celle-ci dès maintenant !</strong></p><p>Example (`nuxt.config.js`):</p>

```js
import fs from 'fs'
import path from 'path'

export default {
  hooks: {
    build: {
      done(builder) {
        const extraFilePath = path.join(builder.nuxt.options.buildDir, 'extra-file')
        fs.writeFileSync(extraFilePath, 'Something extra')
      }
    }
  }
}
```

Internally, hooks follow a naming pattern using colons (e.g., `build:done`). For ease of configuration, you can structure them as an hierarchical object when using `nuxt.config.js` (as exemplifed above) to set your own hooks. See [Nuxt Internals](/api/internals) for more detailed information on how they work.

## List of hooks

- [Nuxt hooks](https://nuxtjs.org/api/internals-nuxt#hooks)
- [Renderer hooks](https://nuxtjs.org/api/internals-renderer#hooks)
- [ModulesContainer hooks](https://nuxtjs.org/api/internals-module-container#hooks)
- [Builder hooks](https://nuxtjs.org/api/internals-builder#hooks)
- [Generator hooks](https://nuxtjs.org/api/internals-generator#hooks)
