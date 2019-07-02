---
title: window ou document non défini
description: window ou document non défini avec Nuxt.js ?
---

# window ou document non défini ?

Cette erreur est due au rendu côté serveur. Si vous devez spécifier que vous souhaitez importer une ressource uniquement côté client, vous devez utiliser la variable `process.browser`.

Par exemple, dans votre fichier `.vue` :

```js
if (process.browser) {
  require('external_library')
}
```
