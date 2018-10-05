---
title: Installation
description: Débuter avec Nuxt.js est vraiment facile. Un projet simple n'a besoin que d'une dépendance à `nuxt`.
---

> Débuter avec Nuxt.js est vraiment facile. Un projet simple n'a besoin que d'une dépendance à `nuxt`.

## Utiliser `create-nuxt-app`

Afin de démarrer rapidement, l'équipe Nuxt.js a créé un outil de démarrage [create-nuxt-app](https://github.com/nuxt/create-nuxt-app).

Assurez-vous que [npx](https://www.npmjs.com/package/npx) est installé (`npx` est livré par défaut depuis NPM `5.2.0`)

```bash
$ npx create-nuxt-app <project-name>
```

Or with [yarn](https://yarnpkg.com/en/):

```bash
yarn create nuxt-app <my-project>
```

It will ask you some questions:

1. Choose between integrated server-side frameworks:
  - None (Nuxt default server)
  - [Express](https://github.com/expressjs/express)
  - [Koa](https://github.com/koajs/koa)
  - [Hapi](https://github.com/hapijs/hapi)
  - [Feathers](https://github.com/feathersjs/feathers)
  - [Micro](https://github.com/zeit/micro)
  - [Adonis](https://github.com/adonisjs/adonis-framework) (WIP)
2. Choose your favorite UI framework:
  - None (feel free to add one later)
  - [Bootstrap](https://github.com/bootstrap-vue/bootstrap-vue)
  - [Vuetify](https://github.com/vuetifyjs/vuetify)
  - [Bulma](https://github.com/jgthms/bulma)
  - [Tailwind](https://github.com/tailwindcss/tailwindcss)
  - [Element UI](https://github.com/ElemeFE/element)
  - [Buefy](https://buefy.github.io)
3. The Nuxt mode you want (`Universal` or `SPA`)
4. Add [axios module](https://github.com/nuxt-community/axios-module) to make HTTP request easily into your application.
5. Add [EsLint](https://eslint.org/) to Lint your code on save.
5. Add [Prettier](https://prettier.io/) to prettify your code on save.

When answered, it will install all the dependencies so the next step is to launch the project with:

```bash
$ npm run dev
```
L'application est désormais accessible à l'adresse http://localhost:3000.

<p class="Alert">Nuxt.js va surveiller les modifications faites sur les fichiers du répertoire <code>pages</code> aussi il n'y a pas besoin de redémarrer le serveur quand vous ajoutez de nouvelles pages.</p>

Pour en savoir plus sur l'organisation des répertoires dans un projet, consultez la documentation de l'[Architecture des répertoires](/guide/directory-structure).

## Commencer depuis zéro

La création d'une application Nuxt.js à partir de zéro est également très simple, elle ne nécessite qu'*1 fichier et 1 répertoire*. Créez un répertoire vide pour commencer à travailler sur l'application :

```bash
$ mkdir <nom-du-projet>
$ cd <nom-du-projet>
```

<p class="Alert Alert--nuxt-green"><b>Info :</b> remplacez <code>&lt;nom-du-projet&gt;</nom-du-projet></code> par le nom du projet.</p>

### Le package.json

Le projet a besoin d'un fichier `package.json` avec un script permettant de lancer `nuxt` :

```json
{
  "name": "mon-application",
  "scripts": {
    "dev": "nuxt"
  }
}
```

`scripts` lancera Nuxt.js via `npm run dev`.


### Installation de `nuxt`

Une fois que le `package.json` est créé, ajoutez `nuxt` au projet via npm :

```bash
npm install --save nuxt
```

### Le dossier `pages`

Nuxt.js va transformer chaque fichier `*.vue` se trouvant dans le dossier `pages` en une route pour l'application.

Créez le dossier `pages` :

```bash
$ mkdir pages
```

puis créez la première page `pages/index.vue`:

```html
<template>
  <h1>Hello world !</h1>
</template>
```

et lancez le projet avec :

```bash
$ npm run dev
```

L'application est désormais accessible à l'adresse http://localhost:3000.

<p class="Alert">Nuxt.js va surveiller les modifications faites sur les fichiers du répertoire <code>pages</code> aussi il n'y a pas besoin de redémarrer le serveur quand vous ajoutez de nouvelles pages</p>

Pour en savoir plus sur la structure des dossiers du projet, consultez la documentation de l'[Architecture des répertoires](/guide/directory-structure).
