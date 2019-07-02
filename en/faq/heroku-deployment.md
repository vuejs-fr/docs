---
title: Déployer sur Heroku
description: Comment déployer Nuxt.js sur Heroku ?
---

# Comment déployer sur Heroku ?

Nous vous recommandons de lire la [documentation Heroku pour Node.js](https://devcenter.heroku.com/articles/nodejs-support).

Premièrement, nous devons demander à Heroku d'installer les `devDependencies` du projet (afin de pouvoir exécuter `npm run build`) :

```bash
heroku config:set NPM_CONFIG_PRODUCTION=false
```

Nous voulons également que notre application écoute le port `0.0.0.0` et s'exécute en mode production :

```bash
heroku config:set HOST=0.0.0.0
heroku config:set NODE_ENV=production
```

Vous devriez voir cela dans votre tableau de bord Heroku (section Settings) :

![variables de configuration de nuxt pour Heroku](https://i.imgur.com/EEKl6aS.png)

Puis nous demandons à Heroku d'exécuter `npm run build` via le script `heroku-postbuild` de notre `package.json` :

```js
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "start": "nuxt start",
  "heroku-postbuild": "npm run build"
}
```

Heroku utilise un [Procfile](https://devcenter.heroku.com/articles/procfile) (nommer le fichier `Procfile` sans extension de nom de fichier) qui indique les commandes à exécuter par les apps dynos. Pour commencer le Procfile sera très simple, et doit contenir les lignes suivantes :

```
web: npm run start
```

Cela indique qu'il faut lancer la commande `npm run start` et dit à heroku de lui rediriger le trafic HTTP externe.

Pour finir, nous pouvons déployer notre application sur Heroku :

```bash
git push heroku master
```

Pour déployer une branche non "master" sur Heroku, utiliser :
```bash
git push heroku develop:master
```
où `develop` est le nom de votre branche.

Voilà ! Votre application Nuxt.js est hébergée sur Heroku !
