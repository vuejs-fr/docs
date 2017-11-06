---
title: "API : La classe Builder"
description: La classe `Builder` de Nuxt
---

# Classe Builder

- Source : **[builder/builder.js](https://github.com/nuxt/nuxt.js/blob/dev/lib/builder/builder.js)**


## Plugins Tapable

Nous pouvons enregistrer des points d'ancrage sur certains évènements du cycle de vie.

```js
nuxt.plugin('build', builder => {
    builder.plugin('extendRoutes', async ({routes}) =>  {
        // ...
    })
})
```

Plugin         | Arguments                               | When
---------------|-----------------------------------------|-------------------------------------------------------------------------------
`build`        | builder                                 | Au démarrage du premier build
`built`        | builder                                 | À la fin du premier build
`extendRoutes` | {routes, templateVars, r}               | À la génération des routes
`generate`     | {builder, templatesFiles, templateVars} | À la génération des fichiers template `.nuxt`
`done`         | {builder, stats}                        | Quand les build webpack sont fini
`compile`      | {builder, compiler}                     | Avant la compilation webpack (le compilateur est une instance `MultiCompiler`)
`compiled`     | builder                                 | À la fin du build webpack
