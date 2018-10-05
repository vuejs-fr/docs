---
title: Plugins PostCSS
description: Comment utiliser des plugins PostCSS ?
---

# Comment utiliser des plugins PostCSS ?

Dans votre fichier `nuxt.config.js` :

```js

import postcssNested from 'postcss-nested',
import postcssResponsiveType from 'postcss-responsive-type',
import postcssHexrgba from 'postcss-hexrgba',

export default {
  build: {
    postcss: [
      postcssNested()
      postcssResponsiveType()
      postcssHexrgba()
    ]
  }
}
```
