---
title: "API: La propriété buildDir"
description: Définissez le dossier dist de votre application nuxt.js
---

# The buildDir Property (EN)
# La propriété buildDir

- Type: `String`
- Par défaut: `.nuxt`

> Définissez le répertoire dist de votre application nuxt.js

<p style="width: 294px;position: fixed; top : 64px; right: 4px;" class="Alert Alert--orange"><strong>⚠Cette page est actuellement en cours de traduction française. Vous pouvez repasser plus tard ou <a href="https://github.com/vuejs-fr/nuxt" target="_blank">participer à la traduction</a> de celle-ci dès maintenant !</strong></p><p>Example (`nuxt.config.js`):</p>

```js
module.exports = {
  buildDir: 'nuxt-dist'
}
```

Par défaut, de nombreux outils supposent que `.nuxt` est un répertoire caché, car son nom commence par un point. Vous pouvez utiliser cette option pour rendre le dossier dist non masqué.