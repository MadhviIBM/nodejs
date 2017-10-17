---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}


# Tutoriel Initiation

* {: download} Félicitations, vous avez déployé une application exemple Hello World sur {{site.data.keyword.Bluemix}} !  Pour commencer, suivez ce guide pas à pas. Ou <a class="xref" href="http://bluemix.net" target="_blank" title="(Télécharger l'exemple de code)"><img class="hidden" src="../../images/btn_starter-code.svg" alt="Télécharger le code de l'application" />téléchargez l'exemple de code</a> et découvrez-le par vous-même.

En suivant ce tutoriel d'initiation Node.js, vous allez configurer un environnement de développement, déployer une application en local et sur {{site.data.keyword.Bluemix}}, puis intégrer un service de base de données {{site.data.keyword.Bluemix}} dans votre application.

## Avant de commencer
{: #prereqs}

Vous aurez besoin des comptes et outils suivants :
* [Compte {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/registration/)
* [Cloud Foundry CLI (client de ligne de commande pour Cloud Foundry) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window}
* [Git ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git-scm.com/downloads){: new_window}
* [Node ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://nodejs.org/en/){: new_window}


## Etape 1 : Cloner l'application exemple
{: #clone}

Commencez par cloner le référentiel GitHub contenant l'application exemple Node.js *hello world*.
  ```
git clone https://github.com/IBM-Bluemix/get-started-node
  ```
  {: pre}

## Etape 2 : Exécuter l'application localement
{: #run_locally}

Utilisez le gestionnaire de package npm pour installer les dépendances et lancer votre application.

1. Sur la ligne de commande, placez-vous dans le répertoire où se trouve l'application exemple.
  ```
cd get-started-node
  ```
  {: pre}

1. Installez les dépendances listées dans le fichier [package.json ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.npmjs.com/files/package.json) afin de pouvoir exécuter l'application localement.  
  ```
npm install
  ```
  {: pre}

1. Exécutez l'application.
  ```
npm start  
  ```
  {: pre}

Vous pouvez visualiser votre application sur : http://localhost:3000.

Utilisez [nodemon](https://nodemon.io/) pour que l'application redémarre automatiquement en cas de changement de fichier.
{: tip}


## Etape 3 : Préparer l'application pour le déploiement
{: #prepare}

Pour déployer sur {{site.data.keyword.Bluemix_notm}}, il peut être utile de créer un fichier manifest.yml. Ce fichier inclut des informations essentielles sur votre application, telles que son nom, la quantité de mémoire à allouer à chaque instance et la route. Pour vous éviter d'avoir à le créer, nous avons prévu un exemple de fichier manifest.yml dans le répertoire `get-started-node`.

Ouvrez le fichier manifest.yml et, dans le champ `name`, remplacez le nom `GetStartedNode` par celui de votre application, <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

```
applications:
- name: GetStartedNode
  random-route: true
  memory: 128M
```
{: codeblock}

Dans ce fichier manifest.yml, l'effet de **random-route: true** est de générer une route aléatoire pour votre application afin d'éviter qu'elle n'entre en conflit avec d'autres.  Au besoin, vous pouvez remplacer **random-route: true** par **host: myChosenHostName** et fournir un nom d'hôte de votre choix. [En savoir plus...](/docs/manageapps/depapps.html#appmanifest)
{: tip}

## Etape 4 : Déployer l'application
{: #deploy}

Pour déployer des applications sur {{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser le client de ligne de commande pour Cloud Foundry.

Exécutez la commande suivante pour spécifier votre point d'extrémité d'API. Remplacez la valeur symbolique _API-endpoint_ par le point d'extrémité d'API de votre région.
   ```
cf api <API-endpoint>
   ```
   {: pre}

   |Région          |Point d'extrémité d'API                             |
   |:---------------|:-------------------------------|
   | Sud des Etats-Unis|https://api.ng.bluemix.net   |
   | Royaume-Uni    | https://api.eu-gb.bluemix.net  |
   | Sydney         | https://api.au-syd.bluemix.net |
   | Francfort      | https://api.eu-de.bluemix.net  | 

Connectez-vous à votre compte {{site.data.keyword.Bluemix_notm}}.

  ```
cf login
  ```
  {: pre}

Si vous ne pouvez pas vous connecter à l'aide des commandes `cf login` ou `bx login` car vous disposez d'un ID utilisateur fédéré, utilisez la commande `cf login --sso` ou la commande `bx login --sso` pour vous connecter avec votre ID de connexion unique (SSO). Voir [Connexion à l'aide d'un ID fédéré](https://console.bluemix.net/docs/cli/login_federated_id.html#federated_id) pour en savoir plus.

Depuis le répertoire *get-started-node*, poussez votre application vers {{site.data.keyword.Bluemix_notm}}.
  ```
cf push
  ```
  {: pre}

Le déploiement de votre application peut prendre quelques minutes. Une fois le déploiement achevé, vous verrez un message indiquant que votre application est lancée. Vous pouvez visualiser l'application en accédant à l'URL qui figure dans la sortie de la commande push. Vous pouvez aussi voir à la fois son statut de déploiement et son URL en exécutant la commande suivante :
  ```
cf apps
  ```
  {: pre}

Vous pouvez identifier et résoudre les erreurs du processus de déploiement en utilisant la commande `cf logs <Your-App-Name> --recent`.
{: tip}

## Etape 5 : Ajouter une base de données
{: #add_database}

Nous allons à présent ajouter une base de données NoSQL à l'application et configurer cette dernière pour qu'elle puisse être exécutée localement et sur {{site.data.keyword.Bluemix_notm}}.

1. Dans votre navigateur, connectez-vous à {{site.data.keyword.Bluemix_notm}} et allez au Tableau de bord. Sélectionnez votre application en cliquant sur son nom dans la colonne **Nom**.
2. Cliquez sur **Connexions**, puis sur **Connecter un nouveau**.
2. Dans la section **Données & analyse**, sélectionnez `Cloudant NoSQL DB`, puis créez le service.
3. Sélectionnez **Reconstituer** lorsque vous y êtes invité. {{site.data.keyword.Bluemix_notm}} redémarre votre application et lui fournit les données d'identification pour l'accès à la base de données en utilisant la variable d'environnement `VCAP_SERVICES`. L'application n'a accès à cette variable d'environnement que lorsqu'elle fonctionne sur {{site.data.keyword.Bluemix_notm}}.

Les variables d'environnement vous permettent de séparer les paramètres de déploiement de votre code source. Par exemple, plutôt que de coder en dur le mot de passe d'accès à la base de données, vous pouvez le stocker dans une variable d'environnement et inclure une référence à celle-ci dans votre code source. [En savoir plus...](/docs/manageapps/depapps.html#app_env)
{: tip}

## Etape 6 : Utiliser la base de données
{: #use_database}
Nous allons à présent mettre à jour votre code local pour le faire pointer sur cette base de données. Créons à cet effet un fichier JSON pour y stocker les identifiants d'accès aux services que l'application utilisera. Ce fichier ne sera utilisé QUE lorsque l'application est exécutée localement. Lors de son fonctionnement sur {{site.data.keyword.Bluemix_notm}}, elle obtiendra ces identifiants en lisant la variable d'environnement `VCAP_SERVICES`.

1. Dans le répertoire `get-started-node`, créez un fichier nommé `vcap-local.json` et ajoutez-y le contenu suivant :
  ```
  {
    "services": {
      "cloudantNoSQLDB": [
        {
          "credentials": {
            "url":"CLOUDANT_DATABASE_URL"
          },
          "label": "cloudantNoSQLDB"
        }
      ]
    }
  }
  ```
  {: codeblock}

2. Dans votre navigateur, accédez à {{site.data.keyword.Bluemix_notm}} et sélectionnez **Applications > _votre application_ > Connexions > Cloudant > Afficher les données d'identification**.

3. Copiez seulement la partie `url` des données d'identification et collez-la dans le champ `url` du fichier `vcap-local.json` pour remplacer **CLOUDANT_DATABASE_URL**.

4. Lancez votre application localement.
  ```
npm start  
  ```
  {: pre}

  Affichez votre application locale sur : http://localhost:3000. Chaque nom que vous entrez dans l'application sera ajouté à la base de données.

  Votre application locale et son instance {{site.data.keyword.Bluemix_notm}} partagent la même base de données. Les noms que vous ajoutez depuis l'une ou l'autre version de l'application doivent apparaître dans les deux lorsque vous actualisez les navigateurs.

Si vous n'avez pas besoin de votre application live sur {{site.data.keyword.Bluemix_notm}}, arrêtez-la. Vous éviterez des frais imprévus.
{: tip}
