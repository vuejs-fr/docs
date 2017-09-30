---
title: Modules
description: Les modules sont des extensions de Nuxt.js qui augmente ses fonctionnalités et permet de l'intégration continue.
---

> Les modules sont des extensions de Nuxt.js qui augmente ses fonctionnalités et permet de l'intégration continue.

## Introduction

Pendant votre phase de préparation du développement pour la production avec Nuxt, vous allez découvrir que les fonctionnalités offertes par Nuxt ne sont pas suffisantes et que faire la configuration et ajouter les plugins de chaque projet est répétitif, ennuyant et prend du temps. Ajouter chaque nouvelle fonctionnalité dans Nuxt est également impossible sans rendre le framework lourd.

C'est pourquoi Nuxt introduit un système modulaire d'ordre supérieur pour facilement étendre ses fonctionnalités de base. Les modules sont en fait des **fonctions** qui sont appelées de manière séquentielle lors de la phase de démarrage de Nuxt. Le cœur va attendre que chaqun d'entre eux soit chargé avant de continuer son travail. Ainsi vous avez la possibilité de personnaliser le moindre aspet de Nuxt et grâce à sa conception modulaire ainsi que webpack [Tapable](https://github.com/webpack/tapable) il peut également abonner des points d'ancrage (« hooks ») pour certaines étapes comme l'initialisation de la phase se build.

Un autre point à propos des modules est qu'ils peuvent être refactorisés et packagés en dehors du projet de manière à être versionné en tant que packages npm. Ainsi vous pouvez partager et utiliser des intégrations et solutions de qualités auprès de la communauté Nuxt sans effort ! Vous pourriez être intéressé par les modules si :

- Êtes un membre d'une **équipe agile** qui souhaite mettre en place son projet instantanément et éviter de **réinventer** la roue pour les tâches habituelles comme des mécanismes Google Analytics pour vos nouveaux projets.
- Êtes une **société** qui accorde de l'importance à la **qualité** et la **réutilisabilité** de ses projets.
- Vous êtes un membre super enthousiaste de la communauté *Open Source* et que vous souhaitez *partager* avec la communauté d'une façon simple.
- Vous êtes un développeur flaimard et vous n'aimez pas vous encombrer avec des détails comme le paramètrage de chaque nouvelle bibliothèque ou intégration (Quelqu'un l'aura surement déjà fait pour vous, ou vous pourriez demander à quelqu'un de la communauté de le faire).
- Vous êtes fatigué de l'utilisation des API bas niveau et de leur changement continue et vous souhaiter **simplement des choses fonctionnelles**.

## Écrire un module basique

Comme précédemment mentionné, les modules sont juste de simples fonctions. Ils peuvent être packagé en tant que modules npm ou directement inclus dans le code source du projet.

**modules/simple.js**

```js
module.exports = function SimpleModule (moduleOptions) {
  // Écrivez votre code ici
}

// Requis en cas de publication en tant que package npm
// module.exports.meta = require('./package.json')
```

**`moduleOptions`**

Ceci est un objet passé en utilisant le tableau `modules` par les utilisateurs qui souhaite personnaliser son comportement.

**`this.options`**

Vous pouvez accéder directement aux options de Nuxt en utilisant cette référence. C'est la configuration _nuxt.config.js_ avec ses options par défaut assignés qui peut être utilisé en tant qu'options partagées à travers les modules.

**`this.nuxt`**

C'est une référence à l'instance courante de Nuxt. Consultez la documentation sur la [Classe Nuxt](/api/internals-nuxt) pour obtenir les méthodes disponibles.

**`this`**

Le contexte des modules. Consultez la documentation sur la [Classe ModuleContainer](/api/internals-module-container) pour obtenir les méthodes disponibles.

**`module.exports.meta`**

Cette ligne est **obligatoire** si vous publiez un module en tant que package npm. Nuxt utilise en interne les meta pour mieux fonctionner avec votre package.

**nuxt.config.js**

```js
module.exports = {
  modules: [
    // Utilisation simple
    '~/modules/simple'

    // Passage des options
    ['~/modules/simple', { token: '123' }]
  ]
}
```

Nous pouvons dire à Nuxt de charger des modules spécifiques pour un projet avec des paramètres optionnels en tant qu'options. Consultez la documentation sur la [configuration des modules](/api/configuration-modules) pour plus d'informations !

## Async Modules

Not all modules will do everything synchronous.
For example you may want to develop a module which needs fetching some API or doing async IO.
For this, Nuxt supports async modules which can return a Promise or call a callback.

### Use async/await

<p class="Alert Alert--orange">
  Be aware that async/await is only supported in Node.js > 7.2
  So if you are a module developer at least warn users about that if using them.
  For heavily async modules or better legacy support you can use either a bundler to transform it for older node comparability or using promise method.
</p>

```js
const fse = require('fs-extra')

module.exports = async function asyncModule() {
  // You can do async works here using async/await
  let pages = await fse.readJson('./pages.json')
}
```

### Return a Promise

```js
const axios = require('axios')

module.exports = function asyncModule() {
  return axios.get('https://jsonplaceholder.typicode.com/users')
    .then(res => res.data.map(user => '/users/' + user.username))
    .then(routes => {
      // Do something by extending nuxt routes
    })
}
```

### Use callbacks

```js
const axios = require('axios')

module.exports = function asyncModule(callback) {
  axios.get('https://jsonplaceholder.typicode.com/users')
    .then(res => res.data.map(user => '/users/' + user.username))
    .then(routes => {
      callback()
    })
}
```


## Common Snippets

### Top level options
Sometimes it is more convenient if we can use top level options while register modules in `nuxt.config.js`.
So we can combine multiply option sources.

**nuxt.config.js**

```js
module.exports = {
  modules: [
    '@nuxtjs/axios'
  ],

  // axios module is aware of this by using this.options.axios
  axios: {
    option1,
    option2
  }
}
```

**module.js**

```js
module.exports = function (moduleOptions) {
  const options = Object.assign({}, this.options.axios, moduleOptions)
  // ...
}
```

### Provide plugins
It is common that modules provide one or more plugins when added.
For example [bootstrap-vue](https://bootstrap-vue.js.org) module would require to register itself into Vue.
For this we can use `this.addPlugin` helper.

**plugin.js**
```js
import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue/dist/bootstrap-vue.esm'

Vue.use(BootstrapVue)
```

**module.js**
```js
const path = require('path')

module.exports = function nuxtBootstrapVue (moduleOptions) {
  // Register plugin.js template
  this.addPlugin(path.resolve(__dirname, 'plugin.js'))
}
```

### Template plugins
Registered templates and plugins can leverage [lodash templates](https://lodash.com/docs/4.17.4#template)
to conditionally change registered plugins output.

**plugin.js**
```js
// Set Google Analytics UA
ga('create', '<%= options.ua %>', 'auto')

<% if (options.debug) { %>
// Dev only code
<% } %>
```

**module.js**
```js
const path = require('path')

module.exports = function nuxtBootstrapVue (moduleOptions) {
  // Register plugin.js template
  this.addPlugin({
    src: path.resolve(__dirname, 'plugin.js'),
    options: {
      // Nuxt will replace options.ua with 123 when copying plugin to project
      ua: 123,

      // conditional parts with dev will be stripped from plugin code on production builds
      debug: this.options.dev
    }
  })
}
```

### Add a CSS library
It is recommended checking if user already not provided same library to avoid adding duplicates.
Also always consider having **an option to disable** adding css files by module.

**module.js**

```js
module.exports = function (moduleOptions) {
  if (moduleOptions.fontAwesome !== false) {
    // Add font-awesome
    this.options.css.push('font-awesome/css/font-awesome.css')
  }
}
```

### Emit assets
We can register webpack plugins to emit assets during build.

**module.js**

```js
module.exports = function (moduleOptions) {
  const info = 'Built by awesome module - 1.3 alpha on ' + Date.now()

  this.options.build.plugins.push({
    apply (compiler) {
      compiler.plugin('emit', (compilation, cb) => {

        // This will generate `.nuxt/dist/info.txt' with contents of info variable.
        // Source can be buffer too
        compilation.assets['info.txt'] = { source: () => info, size: () => info.length }

        cb()
      })
    }
  })
}
```

### Register custom loaders

We can do the same as `build.extend` in `nuxt.config.js` using `this.extendBuild`

**module.js**

```js
module.exports = function (moduleOptions) {
    this.extendBuild((config, { isClient, isServer }) => {
      // .foo Loader
      config.module.rules.push({
        test: /\.foo$/,
        use: [...]
      })

      // Customize existing loaders
      // Refer to source code for Nuxt internals:
      // https://github.com/nuxt/nuxt.js/blob/dev/lib/builder/webpack/base.config.js
      const barLoader = config.module.rules.find(rule => rule.loader === 'bar-loader')
  })
}
```

## Run Tasks on Specific hooks
Your module may need to do things only on specific conditions not just during Nuxt initialization.
We can use powerful [tapable](https://github.com/webpack/tapable) plugin system to do tasks on specific events.
Nuxt will await for us if hooks return a Promise or are defined as `async`.

```js
module.exports = function () {
  // Add hook for modules
  this.nuxt.plugin('module', moduleContainer => {
    // This will be called when all modules finished loading
  })

  // Add hook for renderer
  this.nuxt.plugin('renderer', renderer => {
    // This will be called when renderer was created
  })

  // Add hook for build
  this.nuxt.plugin('build', async builder => {
    // This Will be called once when builder created

    // We can even register internal hooks here
    builder.plugin('compile', ({compiler}) => {
        // This will be run just before webpack compiler starts
    })
  })

  // Add hook for generate
  this.nuxt.plugin('generate', async generator => {
    // This Will be called when a nuxt generate starts
  })
}
```

<p class="Alert">
  There are many many more hooks and possibilities for modules.
  Please refer to [Nuxt Internals](/api/internals) to learn more about Nuxt internal API.
</p>