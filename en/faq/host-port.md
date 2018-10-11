---
title: Host et Port
description: Comment changer le HOST et le PORT avec Nuxt.js ?
---

# Comment changer le host et le port ?

Vous pouvez configurer les variables de connexion de plusieurs façon différentes, listé **de la plus haute à la plus basse priorité** :


## As direct arguments (En)

```sh
nuxt --hostname myhost --port 3333
```
Or
```js
"scripts": {
  "dev": "nuxt --hostname myhost --port 3333"
}
```

## Configure in `nuxt.config.js` (En):

Inside your `nuxt.config.js`:

```js
export default {
  server: {
    port: 8000, // default: 3000
    host: '0.0.0.0', // default: localhost
  },
  // other configs
}
```


## With NUXT_HOST and NUXT_PORT env variables (En)

Similar to HOST and PORT but more specific in case you need those for something else.

```js
"scripts": {
  "dev": "NUXT_HOST=0.0.0.0 NUXT_PORT=3333 nuxt"
}
```

**Note** : pour un meilleur développement qui fonctionne sur toutes les plateformes vous pouvez utiliser le package [cross-env](https://www.npmjs.com/package/cross-env).

Installation :

```bash
npm install --save-dev cross-env
```

```js
"scripts": {
  "dev": "cross-env NUXT_HOST=0.0.0.0 NUXT_PORT=3333 nuxt"
}
```

## With HOST and PORT env variables (En)

```js
"scripts": {
  "dev": "HOST=0.0.0.0 PORT=3333 nuxt"
}
```


## Via a `nuxt` config in the `package.json` (En):

Inside your `package.json`:

```json
"config": {
  "nuxt": {
    "host": "0.0.0.0",
    "port": "3333"
  }
},
"scripts": {
  "dev": "nuxt"
}
```
