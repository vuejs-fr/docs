---
title: Plugins
description: Nuxt.js vous permet de définir les plugins JavaScript à exécuter avant d'instancier l'application Vue.js racine. Cela est particulièrement pratique quand vous utilisez vos propres bibliothèques ou modules externes.
---

> Nuxt.js vous permet de définir les plugins JavaScript à exécuter avant d'instancier l'application Vue.js racine. Cela est particulièrement pratique quand vous utilisez vos propres bibliothèques ou modules externes.

<div class="Alert">Il est important de savoir que, dans le [cycle de vie d'une instance de Vue](https://fr.vuejs.org/v2/guide/instance.html#Diagramme-du-cycle-de-vie), les hooks `beforeCreate` et `created` sont appelés **à la fois du côté client et du côté serveur**. Tous les autres *hooks* ne sont appelés que depuis le client.</div>

## Modules externes

Nous souhaitons utiliser des packages / modules externes dans notre application, un excellent exemple est [axios](https://github.com/mzabriskie/axios) pour les requêtes HTTP depuis le serveur et le client.

Nous l'installons via npm :

```bash
npm install --save axios
```

Puis nous pouvons l'utiliser directement dans nos pages :

```html
<template>
  <h1>{{ title }}</h1>
</template>

<script>
import axios from 'axios'

export default {
  async data ({ params }) {
    let { data } = await axios.get(`https://my-api/posts/${params.id}`)
    return { title: data.title }
  }
}
</script>
```

## Plugins Vue

Si nous voulons utiliser [vue-notifications](https://github.com/se-panfilov/vue-notifications) pour afficher des notifications dans notre application, nous devons configurer le plugin avant de lancer l'application.

Dans `plugins/vue-notifications.js` :

```js
import Vue from 'vue'
import VueNotifications from 'vue-notifications'

Vue.use(VueNotifications)
```

Puis nous ajoutons le fichier dans l'attribut `plugins` de `nuxt.config.js` :

```js
export default {
  plugins: ['~/plugins/vue-notifications']
}
```

Pour en savoir plus sur l'attribut `plugins`, consultez [La propriété `plugins`](/api/configuration-plugins) de l'API.

## Inject in $root & context

Sometimes you want to make functions or values available across the app.
You can inject those variables into Vue instances (client side), the context (server side) and even in the Vuex store.
It is a convention to prefix those functions with a `$`.

### Inject into Vue instances

Injecting content into Vue instances works similar to when doing this in standard Vue apps.

`plugins/vue-inject.js`:

```js
import Vue from 'vue'

Vue.prototype.$myInjectedFunction = (string) => console.log("This is an example", string)
```

`nuxt.config.js`:

```js
export default {
  plugins: ['~/plugins/vue-inject.js']
}
```

You can now use the function in all your Vue components.

`example-component.vue`:

```js
export default {
  mounted(){
    this.$myInjectedFunction('test')
  }
}
```


### Inject into context

Injecting content into Vue instances works similar to when doing this in standard Vue apps.

`plugins/ctx-inject.js`:

```js
export default ({ app }, inject) => {
  // Set the function directly on the context.app object
  app.myInjectedFunction = (string) => console.log('Okay, another function', string)
}
```

`nuxt.config.js`:

```js
export default {
  plugins: ['~/plugins/ctx-inject.js']
}
```

The function is now available whenever you have access to the `context` (for example in `asyncData` and `fetch`).

`ctx-example-component.vue`:

```js
export default {
  asyncData(context){
    context.app.myInjectedFunction('ctx!')
  }
}
```

### Combined inject

If you need the function in the `context`, Vue instances and maybe even in the Vuex store, you can use the `inject` function, which is the second parameter of the plugins exported function.

Injecting content into Vue instances works similar to when doing this in standard Vue apps. The `$` will be prepended automatically to the function.

`plugins/combined-inject.js`:

```js
export default ({ app }, inject) => {
  inject('myInjectedFunction', (string) => console.log('That was easy!', string))
}
```

`nuxt.config.js`:

```js
export default {
  plugins: ['~/plugins/combined-inject.js']
}
```

Now the function can be used from `context`, via `this` in Vue instances and via `this` in store `actions`/`mutations`.

`ctx-example-component.vue`:

```js
export default {
  mounted(){
    this.$myInjectedFunction('works in mounted')
  },
  asyncData(context){
    context.app.$myInjectedFunction('works with context')
  }
}
```

`store/index.js`:

```js
export const state = () => ({
  someValue: ''
})

export const mutations = {
  changeSomeValue(state, newValue) {
    this.$myInjectedFunction('accessible in mutations')
    state.someValue = newValue
  }
}

export const actions = {
  setSomeValueToWhatever ({ commit }) {
    this.$myInjectedFunction('accessible in actions')
    const newValue = "whatever"
    commit('changeSomeValue', newValue)
  }
}

```



## Côté client uniquement

Certains plugins fonctionnent **uniquement dans un navigateur**. Vous pouvez utiliser l'option `ssr: false` dans `plugins` pour exécuter le fichier uniquement côté client.

Exemple :

`nuxt.config.js`:

```js
export default {
  plugins: [
    { src: '~/plugins/vue-notifications', ssr: false }
  ]
}
```

`plugins/vue-notifications.js`:

```js
import Vue from 'vue'
import VueNotifications from 'vue-notifications'

Vue.use(VueNotifications)
```

Dans le cas où vous devez importer certaines bibliothèques uniquement pour le serveur, vous pouvez utiliser la variable `process.server` définie sur `true` lorsque le serveur web crée le fichier `server.bundle.js`.

Si vous avez besoin également de savoir si vous êtes dans une application générée (via `nuxt generate`), vous pouvez vérifier la propriété `process.static` mise à `true` pendant la génération et après. Pour connaitre dans quel état est une page qui est en train d'être rendue par `nuxt generate` avant d'être sauvée, vous pouvez utilisez `process.static && process.server`.
