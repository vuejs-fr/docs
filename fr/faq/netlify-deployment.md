---
title: Déploiement sur Netlify
description: Comment déployer une application Nuxt.js sur Netlify ?
---

# Comment déployer sur Netlify?

Le déploiement vers [Netlify](https://www.netlify.com) est rapide grâce à la **génération** de projets **statiques** NuxtJs

Le déploiement fonctionne avec la méthode `nuxt generate` qui permet de générer une version statique de votre projet vers le dossier `/dist`, c'est dans ce dossier que Netlify va déployer votre projet vers le web.

### Débuter la configuration du déploiement

Choisissez l'option _"New site from Git"_ sur le dashboard Netlify. Maintenant il vous suffit de renseigner les sources de votre dépôt en choisissant votre fournisseurs de dépôts (Github, Gitlab, BitBucket), sélectionnez le dépôt à déployer et continuez. Puis vous arriverez vers l'étapes: _"Build options, and deploy!"_

### Configuration :

1. __Branch to deploy:__ `master` (la branche qui contient votre site)
2. __Build command:__ `npm run generate`
3. __Publish directory:__ `dist`

> Vous avez la possibilité de choisir des variables d'environnement grâce au bouton _"Advanced"_. Les variables d'environnements sont utiles pour ne pas dévoiler les identifiants d'une API par exemple. Netlify offre aussi la possibilité de créer des [variables par défauts](https://www.netlify.com/docs/build-settings/#build-environment-variables).

Il ne vous reste plus qu'a cliquer sur _"Deploy site"_ pour immédiatement déployer votre site web avec la méthode `nuxt generate`. Votre site Netlify à une URL aléatoire qui lui est assigné.

Voilà c'est tout ! Votre application Nuxt.js est maintenant héberger avec Netlify !
