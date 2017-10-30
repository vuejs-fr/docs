---
title: Composants avec cache
description: Comment mettre en cache des composants ?
---

# Comment mettre en cache des composants Vue ?

> Bien que le rendu côté serveur de Vue soit rapide, il ne rivalise pas avec les performances d'un template basé sur une chaine de caractère pure, et ceux, à cause du coût de création des instances de composant et des nœuds du DOM virtuel. Dans le cas où les performances du rendu côté serveur est critique, mettre en place une bonne stratégie de mise en cache peut grandement améliorer le temps de réponse et réduire la charge serveur.

Vous pouvez utiliser le module [Component Cache](https://github.com/nuxt-community/modules/tree/master/packages/component-cache) pour Nuxt.js.
Ce module utilise `vue-server-renderer` pour ajouter le support d'un cache [LRU](https://fr.wikipedia.org/wiki/Algorithmes_de_remplacement_des_lignes_de_cache#LRU_.28Least_Recently_Used.29) pour les composants Vue.

## Utilisation

- Ajoutez la dépendance `@nuxtjs/component-cache` en utilisant Yarn ou npm pour votre projet.
- Ajoutez `@nuxtjs/component-cache` to `modules` section of `nuxt.config.js`.

```js
{
  modules: [
    // Utilisation simple
    '@nuxtjs/component-cache',

    // Avec des options
    ['@nuxtjs/component-cache', {
      max: 10000,
      maxAge: 1000 * 60 * 60
    }],
  ]
}
```

Voir [la mise en cache au niveau composant](http://ssr.vuejs.org/en/caching.html#mise-en-cache-au-niveau-du-composant) pour plus d'informations.

## N'oubliez pas

- Les composants à mettre en cache **doivent définir une option `name` unique**.
- Vous **NE** devez ***PAS*** mettre en cache un composant si
  - ses composants enfants sont liés à ur état global ou si
  - ses composants enfants produisent des effets de bord sur le rendu de `context`.
