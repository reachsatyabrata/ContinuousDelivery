---

copyright:
  years: 2017, 2018
lastupdated: "2018-8-2"

---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Création de modèles de chaîne d'outils personnalisés
{: #toolchains_custom}

Améliorez votre flux de travaux DevOps en créant un modèle de chaîne d'outils personnalisé. Vous pouvez démarrer rapidement avec un modèle de chaîne d'outils existant, ou créer un modèle de chaîne d'outils qui inclut uniquement les intégrations dont vous avez besoin. Vous pouvez ajouter ou retirer des intégrations de votre chaîne d'outils à tout moment.
{:shortdesc}

Il existe différentes façons de [créer une chaîne d'outils](/docs/services/ContinuousDelivery/toolchains_working.html#toolchains_getting_started){: new_window}. Après avoir créé un modèle de chaîne d'outil personnalisé, vous pouvez le partager en [créant un déploiement vers {{site.data.keyword.Bluemix_notm}}](/docs/services/ContinuousDelivery/deploy_button.html#deploy-button){: new_window}.   Vous trouverez des détails sur le kit de développement du modèle de chaîne d'outils sous [Open Toolchain Templates SDK](https://github.com/open-toolchain/sdk/wiki/){:new_window}. Vous trouverez un tutoriel étape par étape sur le [site Garage Method](https://www.ibm.com/cloud/garage/tutorials/create-a-template-for-a-custom-toolchain/){:new_window}.


## Initiation
{: #toolchains_custom_gettingstarted}

Pour créer un modèle de chaîne d'outils personnalisé, commencez par cloner le modèle de chaîne d'outils Cloud Foundry simple. En clonant un modèle existant, vous bénéficiez d'un point de départ pour votre chaîne d'outils personnalisée.

1. En utilisant le client Git de votre choix, entrez la commande suivante pour cloner le modèle [de chaîne d'outils simple](https://github.com/open-toolchain/simple-toolchain){: new_window} dans GitHub.

 ```
 git clone https://github.com/open-toolchain/simple-toolchain.git
 ```
 {: pre}

 Ce modèle déploie une application Hello World de base à partir d'un référentiel GitHub unique et inclut une chaîne d'outils simple qui est préconfigurée pour la distribution continue, le contrôle des sources, le suivi des problèmes et l'édition en ligne.

2. Si vous utilisez un modèle de chaîne d'outils plus complexe, vous pouvez commencer par cloner la [chaîne d'outils native pour le cloud pour les microservices](https://github.com/open-toolchain/toolchain-demo){: new_window}.

 ```
 git clone https://github.com/open-toolchain/toolchain-demo.git
 ```
 {: pre}

Le modèle de microservices déploie un magasin en ligne composé de trois microservices, chacun d'eux étant contenus dans leur propre référentiel GitHub. Il s'agit également d'une chaîne d'outils plus complexe qui est préconfigurée pour :
* La distribution continue
* Le contrôle des sources
* Les déploiements Blue-Green
* Les tests fonctionnels
* Le suivi des problèmes
* L'édition en ligne
* La messagerie

Quel que soit le modèle que vous choisissez, le processus de personnalisation de la chaîne d'outils que vous créez est généralement le même.

Une fois le modèle cloné, vous obtenez un référentiel GitHub de base qui contient un fichier Readme et un répertoire `.bluemix`. Le répertoire contient tous les fichiers de configuration nécessaires au fonctionnement de la chaîne d'outils. Le répertoire `.bluemix` doit au moins contenir les fichiers suivants :

* `toolchain.yml`
* `deploy.json`
* `pipeline.yml`

![Fichiers minimum requis pour définir une chaîne d'outils](images/min_files_for_a_toolchain.png)


Chacun de ces fichiers est décrit dans les sections ci-dessous. Chaque section contient des informations de configuration que vous pouvez consulter à mesure que votre chaîne d'outils évolue.
YAML est un langage de sérialisation de données qui est un sur-ensemble strict de JSON, avec l'ajout de nouvelles lignes et d'indentation syntaxiquement significatives. Cependant, YAML n'autorise pas du tout les caractères de tabulation littéraux.

## Présentation des fichiers de configuration
{: #toolchains_custom_config_files}


Les fichiers de configuration de modèle de chaîne d'outils sont principalement composés de fichiers YAML formatés. Chaque fichier contient des métadonnées décrivant différents aspects de la chaîne d'outils. Les métadonnées incluent notamment :
* Des informations sur les chaînes d'outils et les référentiels.
* Des informations détaillées sur la façon dont le code est généré et déployé
* Des propriétés de configuration relatives aux outils compris dans la chaîne d'outils

Plus votre chaîne d'outils devient complexe, plus la complexité de vos fichiers de configuration est susceptible d'augmenter.

Voici un guide de bonnes pratiques pour la gestion de vos fichiers YAML :

* Utilisez uniquement des espaces. Les tabulations ne sont pas autorisées.
* Toutes les propriétés et toutes les listes doivent présenter un retrait de ligne d'un ou de plusieurs espaces.
* Toutes les clés et toutes les propriétés sont sensibles à la casse.

Prêtez une attention particulière au formatage du fichier YAML afin de réduire les risques d'erreur.
Pour rechercher les éventuelles erreurs, vous pouvez utiliser un valideur simple, tel que [cet analyseur syntaxique](http://wiki.ess3.net/yaml/){: new_window}.
{: tip}

## Section de planification des services
Chaque sous-section de service contient les informations suivantes :

* name - Chaîne générée par l'utilisateur qui est utilisée pour identifier ce service dans le contexte du fichier en cours. Ce nom peut être utilisé pour marquer un service selon les besoins.

* service_id - Chaîne unique qui identifie le service. Cette chaîne vient directement du [catalogue des services](https://github.com/open-toolchain/sdk/wiki/services.md){: new_window}.

* parameters - Zéro paramètre de configuration du service ou plus. Ces paramètres varient entre les services : les utilisateurs doivent consulter le catalogue afin de déterminer les paramètres dont un service particulier a besoin.

### Inclure du texte provenant d'autres fichiers

Toutes les informations de votre chaîne d'outils peuvent se trouver dans le fichier `toolchain.yml`.  Cependant, vous pouvez créer des fichiers séparés pour chaque interface utilisateur d'intégration d'outil en utilisant `$text`. Cela peut faciliter la maintenance de vos chaînes d'outils et minimiser le temps que vous passez à éditer les fichiers de configuration.  Cet exemple extrait d'un fichier `toolchain.yml` montre comment utiliser le contenu du fichier `pipeline.yml` comme valeur pour `content`.

```
  configuration:
    content:
      $text: pipeline.yml
```

### Localisation de votre modèle de chaîne d'outils

Vous pouvez localiser votre chaîne d'outils en externalisant vos chaînes d'interface dans le répertoire `nls` de manière à ce que les chaînes s'affichent dans la langue de l'utilisateur dans la chaîne d'outils.
Votre fichier `toolchain.yml` doit inclure une référence `$i18n`.  
L'exemple suivant montre une référence `$i18n` à un fichier `messages.yml` :

```
messages:
  $i18n: messages.yml
```

  Les chaînes en anglais utilisent le format `messages.yml` et les autres langues utilisent leurs codes de langue respectifs, par exemple `messages_de.yml`.   La liste des codes de langue se trouve sous [Etiquettes d'identification des langues](https://tools.ietf.org/html/rfc5646){: new_window}.

   Pour référencer une chaîne externalisée, utilisez `$ref` pour récupérer la chaîne.  Par exemple,

```
  template:
    name:
      $ref: "#/messages/template.name"
```

  Si vous n'utilisez pas de chaîne externalisée, vous pouvez utiliser :

```
  template:
    name: my_template
```

Pour en savoir plus, voir la [section Messages de Open Toolchain SDK](https://github.com/open-toolchain/sdk/wiki/Template-File-Format#messages-section){: new_window}.

## Configuration du fichier de chaîne d'outils
{: #toolchains_custom_toolchain_yml}

Le fichier `toolchain.yml` est au coeur de votre chaîne d'outils. Ce fichier décrit toutes les caractéristiques de votre chaîne d'outils, y compris les référentiels à rechercher, les services à inclure, ainsi que les détails de génération. Afin de rendre son contenu plus compréhensible, vous pouvez fractionner ce fichier en plusieurs sections.

1\. **Section de présentation de la chaîne d'outils :**

 Cette section du fichier fournit les détails simples relatifs à votre chaîne d'outils que l'utilisateur peut visualiser sur la page de création de la chaîne d'outils. Ajoutez un nom et une description pour votre chaîne d'outils. Vous pouvez également inclure une image, par exemple, un logo ou une représentation visuelle de votre chaîne d'outils.

 En plus de fournir un contenu d'introduction pour votre chaîne d'outils, cette section comprend également une clé nommée `required` qui définit les outils qui font partie de la chaîne d'outils. Le créateur de la chaîne d'outils configure ces outils lorsqu'il la crée à partir de votre modèle. Pour chaque outil configurable sur la page de création de la chaîne d'outils, ajoutez en tant que propriété de la clé `required` la clé parente de l'outil qui est définie dans le fichier `toolchain.yml`.

 Ce fragment est un exemple qui illustre cette section :

 ```
 ---
template
  name: "Simple Toolchain"
  description: "This Hello World application uses Node.js and includes a toolchain that is preconfigured for continuous delivery, source control, issue tracking, and online editing.\n\nTo get started, click **Create**."
  header: '![](toolchain.svg)'
  icon: icon.svg
  required:
   - sample-build
   - sample-repo
  info:
    git url: >-
      [https://github.com/my-toolchain/simple-toolchain](https://github.com/open-toolchain/simple-toolchain)
    git branch: >-
      [master](https://github.com/my-toolchain/simple-toolchain/tree/master)
  ```
 {: codeblock}

Dans cet exemple, l'URL Git et la branche Git s'appliquent à un nouveau modèle de chaîne d'outils.

2\. **Définitions de référentiels :**

 Une chaîne d'outils peut offrir une distribution continue pour n'importe quel nombre de référentiels incluant GitHub, GitHub Enterprise, Git Repos and Issue Tracking et GitLab. Cette section du fichier `toolchain.yml` contient la définition de chaque référentiel.

 Pour chaque référentiel ajouté à la chaîne d'outils, ajoutez une clé parent qui représente le nom de votre référentiel avec les propriétés suivantes :

| Elément | Clé/Propriété | Valeur | Description |
|------|--------------|-------|-------------|
| repo-name | Clé |  | Nom de référentiel. Cette clé correspond au nom (sample-repo) |
| service_id | Propriété | <`githubpublic`, `githubprivate`, `hostedgit`, `gitlab`> | Type de référentiel |
| parameters: | Clé |  |  |
| repo_name | Propriété |  | Modèle pour repo-name. L'exemple qui suit utilise le nom de la chaîne d'outils comme nom de référentiel |
| repo_url | Propriété |  | URL du référentiel |
| type | Propriété | <`new` , `fork` , `clone` , `link`> | Comment créer le nouveau référentiel |
| has_issues | Propriété | <`true` , `false`> | Utilisation de Issues |
| enable_traceability | Propriété |  <`true` , `false`> | Indique s'il y a lieu de suivre le déploiement des modifications de code en créant des étiquettes, des libellés et des commentaires sur les validations, les demandes d'extraction et les problèmes référencés|

 Si vous définissez plusieurs référentiels et les configurez comme `has_issues: true`, une instance unique du dispositif de suivi GitHub Issue est ajoutée à la chaîne d'outils. Le dispositif de suivi trace les problèmes de tous les référentiels pour lesquels cette propriété a pour valeur `true`.
 {: tip}

 Ce fragment est un exemple qui illustre cette section :

 ```
 # Github repos
 services:
  sample-repo:
    service_id: githubpublic
    parameters:
      repo_name: '{{toolchain.name}}'
      repo_url: 'https://github.com/open-toolchain/node-hello-world'
      type: clone
      has_issues: true
      enable_traceability: true
 ```
 {: codeblock}

3\. **Informations sur le pipeline :**

 Vous pouvez fournir en permanence un pipeline à votre projet. Cette section du fichier contient les détails de configuration utilisés pour générer et déployer le code dans chacun des référentiels GitHub et Git Repos and Issue Tracking.

 Pour commencer, pour chaque référentiel défini dans votre chaîne d'outils, ajoutez une clé parente qui représente un nom de son pipeline. Envisagez de dériver cette clé à partir du nom de votre référentiel GitHub ou Git Repos and Issue Tracking. Ajoutez les propriétés suivantes :

| Elément | Clé/Propriété | Valeur | Description |
|------|--------------|-------|-------------|
| pipeline-name | Clé |  | Nom du pipeline (sample-build) |
| service_id | Propriété | <`pipeline`> | Nom du service à utiliser |
| parameters | Clé |  |  |
| name | Propriété | <`repo_name`> | Identique à la définition dans la section repos |
| ui-pipeline | Propriété | <`true` , `false`> |La valeur est true si les applications que ce pipeline déploie s'affichent dans le menu **Afficher l'appli** sur la page de la chaîne d'outils  |
| configuration | Clé |  |  |
| content | Propriété | <`$ref(pipeline.yml)`> | Fichier contenant votre définition de pipeline |
| env | Clé |  |  |
| SAMPLE_REPO | Clé | <`repo-name-key`> | Nom identique à celui de votre clé parente de référentiel |
| CF_APP_NAME |  Propriété | <`'{{form.pipeline.parameters.prod-app-name}}'`> | Nom utilisé par Cloud Foundry. Envisagez d'incorporer votre nom de clé parent de référentiel dans cette propriété. |
| PROD_SPACE_NAME | Propriété | <`'{{form.pipeline.parameters.prod-space}}'`> | Nom de l'espace {{site.data.keyword.Bluemix_notm}} vers lequel effectuer le déploiement |
| PROD_ORG_NAME | Propriété | <`'{{form.pipeline.parameters.prod-organization}}'`> | Nom de l'organisation {{site.data.keyword.Bluemix_notm}} vers laquelle effectuer le déploiement |
| PROD_REGION_ID | Propriété | <`'{{form.pipeline.parameters.prod-region}}'`> | Nom de la région {{site.data.keyword.Bluemix_notm}} vers laquelle effectuer le déploiement |
| execute | Propriété | <`true` , `false`> | Démarrer le pipeline après sa création |

<!--| services | property | <`repo-name-key`> |  GitHub repository parent key |
| hidden | property | <`[form, description]`> |  |
-->

 Des informations sur la création d'un fichier `pipeline.yml` sont fournies [ultérieurement.](#toolchains_custom_pipeline_yml)

 Ce fragment est un exemple qui illustre cette section du fichier :

 ```
 # Pipelines
  sample-build:
    service_id: pipeline
    parameters:
      services:
        - sample-repo
      name: '{{services.sample-repo.parameters.repo_name}}'
      ui-pipeline: true
      configuration:
        content:
          $ref: pipeline.yml
        env:
          SAMPLE_REPO: sample-repo
          CF_APP_NAME: '{{form.pipeline.parameters.prod-app-name}}'
          PROD_SPACE_NAME: '{{form.pipeline.parameters.prod-space}}'
          PROD_ORG_NAME: '{{form.pipeline.parameters.prod-organization}}'
          PROD_REGION_ID: '{{form.pipeline.parameters.prod-region}}'
       execute: true
 ```
 {: codeblock}

4\. **Détails du déploiement :**

 Dans le cadre de la distribution continue, vous pouvez configurer une chaîne d'outils afin de déployer une application sur n'importe quels région, organisation et espace {{site.data.keyword.Bluemix_notm}} auxquels un utilisateur peut accéder. Vous pouvez sélectionner spécifiquement où déployer votre application sur la page de création de la chaîne d'outils.

 ![Paramètres de configuration de Delivery Pipeline](images/deploy_configuration.png)

 Cette section du fichier `toolchain.yml` définit les étapes de pipeline disponibles pour configuration depuis la page de création de la chaîne d'outils.

 Pour commencer, la clé parente `deploy` est utilisée pour identifier les propriétés de configuration du déploiement. Le reste de la section est constitué des propriétés suivantes :

| Elément | Clé/Propriété | Valeur | Description |
|------|--------------|-------|-------------|
| deploy | Clé |  | Nom de la section de déploiement |
| schema | Propriété | <`deploy.json`> | Fichier qui définit l'agencement de l'interface graphique pour configuration des informations de déploiement |
| service-category | Propriété | <`pipeline`> | Service qui utilise les configurations de déploiement |
| parameters | Clé |  |  |
| prod-region | Propriété | <`"{{region}}"`> | Définit la région {{site.data.keyword.Bluemix_notm}} pour la phase de production |
| prod-organization | Propriété | <`"{{organization}}"`> | Définit l'organisation {{site.data.keyword.Bluemix_notm}} pour la phase de production |
| prod-space | Propriété | <`prod`> | Définit l'espace {{site.data.keyword.Bluemix_notm}} pour la phase de production |
| github-repo-name | Propriété | <`"{{repo-name-key.parameters.repo_name}}"`> | Variable servant à transférer le nom de référentiel GitHub à la page de création de la chaîne d'outils |

Pour en savoir plus sur la création d'un fichier `deploy.json`, voir [dans cette section] (#toolchains_custom_deploy_json).

 L'exemple suivant illustre la définition d'une seule étape de déploiement dans un environnement de production :

 ```
 ## Configuring a deployment stage
 deploy:
   schema: deploy.json
   service-category: pipeline
   parameters:
 	prod-region: "{{region}}"
 	prod-organization: "{{organization}}"
 	prod-space: prod
 	hello-world-name: "{{hello-world-repo.parameters.repo_name}}"
 ```
 {: codeblock}

 Cet exemple de code peut être utilisé essentiellement tel quel et ne requiert qu'une modification mineure. Pour personnaliser cette section, définissez `github-repo-name` de telle manière qu'il soit cohérent avec le nom de votre référentiel. Les détails du fichier [`deploy.json`](#toolchains_custom_deploy_json) ont également besoin d'être mis à jour.

 Pour créer un pipeline plus complexe incluant les étapes de développement, de QA et de production, vous pouvez remplacer les propriétés suivantes sous la clé `parameters`.

 ```
   parameters:
 	dev-region: "{{region}}"
 	qa-region: "{{region}}"
 	prod-region: "{{region}}"
 	dev-organization: "{{organization}}"
 	qa-organization: "{{organization}}"
 	prod-organization: "{{organization}}"
 	dev-space: dev
 	qa-space: qa
 	prod-space: prod
 ```
 {: codeblock}

 ## Configuration d'un pipeline
 {: #toolchains_custom_pipeline_yml}

 Le fichier `pipeline.yml` contient toutes les informations de configuration des phases de votre pipeline. Vous pouvez commencer avec un fichier pipeline.yml existant et le personnaliser en fonction de vos besoins.

 Si votre chaîne d'outils contient plusieurs pipelines, attribuez un nom unique à chaque fichier `pipeline.yml`.

 Ci-dessous figure un exemple de fichier `pipeline.yml` :

 ```
 ---
stages:
- name: BUILD
  inputs:
  - type: git
    branch: master
    service: ${SAMPLE_REPO}
  triggers:
  - type: commit
  jobs:
  - name: Build
    type: builder
- name: DEPLOY
  inputs:
  - type: job
    stage: BUILD
    job: Build
  triggers:
  - type: stage
  properties:
  - name: CF_APP_NAME
    value: undefined
    type: text
  - name: APP_URL
    value: undefined
    type: text
  jobs:
  - name: Blue-Green Deploy
    type: deployer
    target:
      region_id: ${PROD_REGION_ID}
      organization: ${PROD_ORG_NAME}
      space: ${PROD_SPACE_NAME}
      application: ${CF_APP_NAME}
    script: |
      #!/bin/bash
      # Push app
      if ! cf app $CF_APP; then  
        cf push $CF_APP
      else
        OLD_CF_APP=${CF_APP}-OLD-$(date +"%s")
        rollback() {
          set +e  
          if cf app $OLD_CF_APP; then
            cf logs $CF_APP --recent
            cf delete $CF_APP -f
            cf rename $OLD_CF_APP $CF_APP
          fi
          exit 1
        }
        set -e
        trap rollback ERR
        cf rename $CF_APP $OLD_CF_APP
        cf push $CF_APP
        cf delete $OLD_CF_APP -f
      fi
      # Export app name and URL for use in later Pipeline jobs
      export CF_APP_NAME="$CF_APP"
      export APP_URL=http://$(cf app $CF_APP_NAME | grep urls: | awk '{print $2}')
      # View logs
      #cf logs "${CF_APP}" --recent
 ```
 {: codeblock}		


 ## Configuration de l'interface de pipeline
 {: #toolchains_custom_deploy_json}

 Sur la page de création de la chaîne d'outils, lorsque Delivery Pipeline est sélectionné dans la section Intégrations configurables, cette section est développée et affiche les éléments suivants :

 	* Nom de l'application
 	* Région, organisation et espace dans lesquels vos phases de pipeline sont déployées.

Vous pouvez configurer ces éléments pour chaque outil.

 ![Paramètres de configuration de Delivery Pipeline](images/deploy_configuration.png)

 L'agencement de cette section dans l'interface utilisateur est défini par le schéma `deploy.json`.

 Au sein du schéma, les propriétés suivantes doivent être mises à jour afin de correspondre aux détails de votre application :

 	* Titre
 	* Description
 	* LongDescription
 	* Toutes les instances de `hello-world-name` et les informations associées doivent être mises à jour afin de concorder avec celles de votre application.

 Le fragment suivant est un exemple de fichier `deploy.json` :

 ```
 {
    "$schema": "http://json-schema.org/draft-04/schema#",
    "messages": {
        "$i18n": "locales.yml"
    },
    "title": {
        "$ref": "#/messages/deploy.title"
    },
    "description": {
        "$ref": "#/messages/deploy.description"
    },
    "longDescription": {
        "$ref": "#/messages/deploy.longDescription"
    },
    "type": "object",
    "properties": {
        "prod-region": {
            "description": "The {{site.data.keyword.Bluemix_notm}} region",
            "type": "string"
        },
        "prod-organization": {
            "description": "The {{site.data.keyword.Bluemix_notm}} org",
            "type": "string"
        },
        "prod-space": {
            "description": "The {{site.data.keyword.Bluemix_notm}} space",
            "type": "string"
        },
        "prod-app-name": {
            "description": {
                "$ref": "#/messages/deploy.appDescription"
            },
            "type": "string",
            "pattern": "\\S"
        }
    },
    "required": [
        "prod-region",
        "prod-organization",
        "prod-space",
        "prod-app-name"
    ],
    "form": [
        {
            "type": "validator",
            "url": "/devops/setup/bm-helper/helper.html"
        },
        {
            "type": "text",
            "readonly": false,
            "title": {
                "$ref": "#/messages/deploy.appName"
            },
            "key": "prod-app-name"
        },
        {
            "type": "table",
            "columnCount": 4,
            "widths": [
                "15%",
                "28%",
                "28%",
                "28%"
            ],
            "items": [
                {
                    "type": "label",
                    "title": ""
                },
                {
                    "type": "label",
                    "title": {
                        "$ref": "#/messages/region"
                    }
                },
                {
                    "type": "label",
                    "title": {
                        "$ref": "#/messages/organization"
                    }
                },
                {
                    "type": "label",
                    "title": {
                        "$ref": "#/messages/space"
                    }
                },
                {
                    "type": "label",
                    "title": {
                        "$ref": "#/messages/prodStage"
                    }
                },
                {
                    "type": "select",
                    "key": "prod-region"
                },
                {
                    "type": "select",
                    "key": "prod-organization"
                },
                {
                    "type": "select",
                    "key": "prod-space",
                    "readonly": false
                }
            ]
        }
    ]
}
 ```
 {: codeblock}


## Autres configurations d'outils

 Après avoir configuré les composants de base de votre chaîne d'outils, vous pouvez inclure d'autres intégrations d'outils qui apportent d'autres fonctions à votre chaîne d'outils. Une entrée spécifique est requise dans le fichier `toolchain.yml` pour chaque outil supplémentaire. Certains outils nécessitent également que vous ajoutiez un fichier de configuration YAML séparé au répertoire `.bluemix`.

 ![Fichiers requis pour définir une chaîne d'outils](images/files_for_toolchain_with_additional_tools.png)

Pour connaître la liste des intégrations d'outils disponibles, voir l'article <a href="https://github.com/open-toolchain/sdk/wiki/services.md" target="_blank">Services disponibles dans un modèle de chaîne d'outils</a>. Les exemples suivants montrent comment formater les ajouts effectués sur un fichier YAML de chaîne d'outils :

### **Slack**

#### toolchain.yml
{: #slack_toolchain_yaml}

```
messaging:
	  service_id: slack
	  $ref: slack.yml
```
{: codeblock}

#### slack.yml
{: #slack_slack_yaml}

```
---YAML
parameters:
  api_token: ""
  channel_name: ""
```
{: codeblock}

### **Sauce Labs**

#### toolchain.yml
{: #sauce_toolchain_yaml}

```YAML
test:
	  service_id: saucelabs
	  $ref: saucelabs.yml
```
{: codeblock}

#### saucelabs.yml
{: #sauce_slack_yaml}

```YAML
---
	parameters:
	  username: ""
  key: ""
```
{: codeblock}

### **Eclipse Orion Web IDE**

####	toolchain.yml
{: #eclipse_toolchain_yaml}

```YAML
webide:
	  service_id: orion
```
{: codeblock}
