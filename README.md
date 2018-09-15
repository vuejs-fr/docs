# Pour traduire la documentation de nuxt

### Workflow de travail

Cette branche de travail `working` est volontairement mise en avant et doit uniquement être mise à jour dans le sens :

`nuxt/docs:master` --> `vuejs-fr/nuxt:working`.

Nous traduisons les fichiers directement dans le dossier `en` sans les renommer. Cela permet lors de la mise à jour de la documentation via l'utilisation des commandes :

Démarrer un serveur de développement sur `localhost:4000` :

```bash
npm install
npm run dev
```

d'obtenir des conflits **sur les pages déjà traduites** et ainsi maintenir la documentation à jour en fonction des modifications à travers **les documents déjà traduits**.

### Traduction

Pour savoir ce qui est [en cours de traduction](https://github.com/vuejs-fr/nuxt/issues/1) ou [comment traduire un fichier](https://github.com/vuejs-fr/nuxt/issues/2), référez vous aux issues correspondantes.

### Reverssement

Quand un fichier traduit est validé par pull request, on le met à jour dans le dossier `fr` de `vuejs-fr/nuxt:master` puis on propose une pull request au site principal :

`vuejs-fr/nuxt:master` --> `nuxt/docs:dev`

ainsi le dossier officiel hébergeant la documentation possède bien le dossier `fr` en français et le dossier `en` en anglais.

<<<<<<< HEAD
Note : il peut être intéressant de faire une pull request par ficher validé et donc de créer une branche dérivée de `vuejs-fr/nuxt:master` pour faire la pull request (`vuejs-fr/nuxt:master` --> `vuejs-fr/nuxt:only_one_changed_file_from_master` --> `vuejs/nuxt:master`)
=======
* Translation Repo - [/o2team/i18n-cn-nuxtjs-docs](https://github.com/o2team/i18n-cn-nuxtjs-docs)
* Primary maintainer - [AOTU Labs](https://aotu.io)
* Primary translator - [Levin Wong](http://faso.me), [Edward Chu](https://github.com/chuyik)

### Japanese

Japanese translation is maintained by [Vue.js Japan User Group](https://github.com/vuejs-jp/home).

* Translation Repo - [https://github.com/vuejs-jp/ja.docs.nuxtjs](https://github.com/vuejs-jp/ja.docs.nuxtjs)
* Primary maintainer - [INOUE Takuya(@inouetakuya)](http://blog.inouetakuya.info/), [HANATANI Takuma(@potato4d)](https://github.com/potato4d), [numa(@aytdm)](https://github.com/aytdm)
* Primary translator - [INOUE Takuya(@inouetakuya)](https://github.com/inouetakuya), [HANATANI Takuma(@potato4d)](https://github.com/potato4d), [numa(@aytdm)](https://github.com/aytdm)

### Korean

Korean translation is maintained by Taewoong La.

* Translation Repo - [/DiyLecko/ko.docs.nuxtjs](https://github.com/DiyLecko/ko.docs.nuxtjs)
* Primary maintainer - [Taewoong La](http://blog.naver.com/diy_lecko)
* Primary translator - [june](http://jicjjang.github.io), [wanybae](https://github.com/wanybae), [rellario](https://github.com/rellario)

### French

French translation is maintained by [Vuejs-FR](https://github.com/vuejs-fr/nuxt/issues/1) Team.

* Translation Repo - [/vuejs-fr/nuxt](https://github.com/vuejs-fr/nuxt)
* Primary maintainer - [Bruno Lesieur](https://www.lesieur.name/) ([Orchard ID](https://www.orchard-id.com/))
* Primary translator - [Julien Grünhagel](https://rspt.io/) ([laruche](https://laruche.io))

### Indonesian

Indonesian translation is maintained by [Nuxt.js Indonesian Community](https://github.com/nuxtjs-id) Team.

* Translation Repo - [https://gitlocalize.com/repo/100/id](https://gitlocalize.com/repo/100/id)
* Primary maintainer - [Achan](http://achan.id/)
* Primary translator - [afrianjunior](https://github.com/afrianjunior), [fikrizufri](https://github.com/fikrizufri), [huiralb](https://github.com/huiralb), [jefrydco](https://github.com/jefrydco), [muhibbudins](https://github.com/muhibbudins), [nusendra](https://github.com/nusendra), [perjakasunda](https://github.com/perjakasunda), [tapitapeh](https://github.com/tapitapeh), [wahwahid](https://github.com/wahwahid)



### Want to help with the translation?

[gl]: https://gitlocalize.com

Much of the translation was done using a tool called [GitLocalize][gl], which is unfortunately reaching its end of service soon. Hence, we're currently exploring and open to suggestions on how to keep this process as streamlined as possible.

For now, **always check the `en/` version** of a document for its latest revision before starting a translation.  

>>>>>>> upstream/master
