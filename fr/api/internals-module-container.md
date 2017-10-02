---
title: "API: The ModuleContainer Class"
description: Nuxt ModuleContainer Class
---

# ModuleContainer Class (En)

- Source: **[core/module.js](https://github.com/nuxt/nuxt.js/blob/dev/lib/core/module.js)**

<p style="width: 294px;position: fixed; top : 64px; right: 4px;" class="Alert Alert--orange"><strong>⚠Cette page est actuellement en cours de traduction française. Vous pouvez repasser plus tard ou <a href="https://github.com/vuejs-fr/nuxt" target="_blank">participer à la traduction</a> de celle-ci dès maintenant !</strong></p><p>All [modules](/guide/modules) will be called within context of ModuleContainer instance.</p>

## Tapable plugins

We can register hooks on certain life cycle events.

```js
nuxt.moduleContainer.plugin('ready', async moduleContainer => {
    // Do this after all modules where ready
})
```

Inside [modules](/guide/modules) context we can use this instead:

```js
this.plugin('ready', async moduleContainer => {
    // Do this after all modules where ready
})
```

Plugin               | Arguments                 | When
---------------------|---------------------------|--------------------------------------------------------------
`ready`              | moduleContainuer          | All modules in `nuxt.config.js` has been initialized


## Methods

### addVendor (vendor)
Adds to `options.build.vendor` and apply uniq filter.

### addTemplate (template)
- **template**: String Or Object
    - src
    - options
    - fileName

Renders given template using [lodash template](https://lodash.com/docs/4.17.4#template) during build into project `buildDir` (`.nuxt`).

If `fileName` is not provided or template is string, target file name defaults to `[dirName].[fileName].[pathHash].[ext]`

This method returns final `{ dist, src, options }` object.

### addPlugin (template)

Registers a plugin using `addTemplate` and adds it to first of `plugins[]` option.
You can use `template.ssr: false` to disable plugin including in SSR bundle.

### addServerMiddleware (middleware)

Pushes middleware into [options.serverMiddleware](/api/configuration-servermiddleware). 

### extendBuild (fn)

Allows easily extending webpack build config by chaining [options.build.extend](/api/configuration-build#extend) function.

### extendRoutes (fn)

Allows easily extending routes by chaining [options.build.extendRoutes](/api/configuration-router#extendroutes) function.

### addModule (moduleOpts, requireOnce) 

Registers module. moduleOpts can be string or `[src, options]`.
If `requireOnce` is `true` and resolved module exports `meta` prevents registering same module twice.

### requireModule (moduleOpts)

Is shortcut to `addModule(moduleOpts, true)`