---
title: API d'authentification externe (JWT)
description: Exemple d'authentification avec le service d'API externe (jsonwebtoken) avec Nuxt.js
github: auth-jwt
code: https://github.com/ahadyekta/nuxt-auth-external-jwt
---

# Documentation

Dans l'exemple auth-routes l'API et le site Nuxt se lancent ensemble et utilisent la même instance serveur Node.js. Cependant, il est parfois mieux de travailler avec une API externe avec jsonWebToken. Cela sera expliqué simplement à travers cet exemple.

## Structure

Puisque Nuxt.js fournit à la fois le rendu client et serveur ainsi qu'un cookie différent entre le navigateur et le serveur Node.js, nous devons fournir un jeton de donnée qui puisse être accessible par les deux parties.

### Pour le rendu serveur

Nous devons sauvegarder le jeton dans un cookie de session après connexion pour qu'il puisse être récupéré via `req.headers.cookie` par les fichiers middlewares, la fonction `nuxtServerInit` ou tout ce qui a accès à `req`.

### Pour le rendu client

Nous actons directement le jeton dans le store, aussi longtemps que la page n'est pas fermée ou rechargée, nous avons le jeton.

Tout d'abord, installons les dépendances :

```bash
npm install js-cookie --save
npm install cookieparser --save
```

## Page de connexion

À l'intérieur du dossier des pages, créez un fichier `login.vue` et dans ce fichier, dans la partie script, ajoutez :

```js
const Cookie = process.client ? require('js-cookie') : undefined

export default {
  middleware: 'notAuthenticated',
  methods: {
    postLogin() {
      setTimeout(() => { // we simulate the async request with timeout.
        const auth = {
          accessToken: 'someStringGotFromApiServiceWithAjax'
        }
        this.$store.commit('setAuth', auth) // muter `auth` dans le store pour le rendu client
        Cookie.set('auth', auth) // sauver le jeton dans un cookie pour le rendu serveur
        this.$router.push('/')
      }, 1000)
    }
  }
}
```

> Note : nous simulerons la requête asynchrone avec un délai.

## Utiliser le store

Après cela modifiez `index.js` dans le dossier `store` comme ci-dessous :

```javascript
import Vuex from 'vuex'

const cookieparser = process.server ? require('cookieparser') : undefined

const createStore = () => {
  return new Vuex.Store({
    state: () => ({
      auth: null
    }),
    mutations: {
      setAuth(state, auth) {
        state.auth = auth
      }
    },
    actions: {
      nuxtServerInit({ commit }, { req }) {
        let auth = null
        if (req.headers.cookie) {
          const parsed = cookieparser.parse(req.headers.cookie)
          try {
            auth = JSON.parse(parsed.auth)
          } catch (err) {
            // No valid cookie found
          }
        }
        commit('setAuth', auth)
      }
    }
  })
}

export default createStore
```

> Note : la fonction `nuxtServerInit` s'exécute seulement dans chaque rendu côté serveur. Nous l'utilisons pour muter le cookie de session du navigateur dans le store. Nous pouvons récupérer le cookie de session du navigateur avec `req.headers.cookie` et l'analyser en utilisant `cookieparser`.

## Authentification vérifiée via middlewares

Nous pouvons vérifier le store pour obtenir un accès au jeton sur toutes les pages qui demandent un accès limité. Dans le dossier des middlewares nous créons un fichier `authenticated.js` :

```javascript
export default function ({ store, redirect }) {
  // Si l'utilisateur n'est pas authentifié
  if (!store.state.auth) {
    return redirect('/login')
  }
}
```

et dans le dossier des middlewares créez un fichier `notAuthenticated.js` pour la page de connexion :

```javascript
export default function ({ store, redirect }) {
  // Si l'utilisateur est authentifié, aller à la page d'accueil
  if (store.state.auth) {
    return redirect('/')
  }
}
```

> Note: use `authenticated` middleware for pages which need authentication and use `notAuthenticated` middleware inside the login/register and similar pages.

## Logging out the User
Finally to allow the user to logout of the system, we can remove the cookie: 

```javascript
const Cookie = process.client ? require('js-cookie') : undefined

export default {
  methods: {
    logout() {
      // Code will also be required to invalidate the JWT Cookie on external API
      Cookie.remove('auth')
      this.$store.commit('setAuth', null)
    }
  }
}
```

> Note: refer to the method using @click="logout"

