# [AWS] Infrastructure As Code: Pourquoi et Comment?

> Dans cet article je vais aborder l'une des principales bonnes pratiques en termes de Cloud Computing, à prendre en main le plus tôt possible : **Infrastructure as Code**, ou comment scripter la création, la gestion et la configuration de vos infrastructure Cloud.

Parmi les nombreuses façon possible de gérer et organiser ses infrastructures, une seule peu prétendre être **la** bonne pratique: ***les Script***.

## Pourquoi scripter ses Infrastructures ?

Pourquoi faut-il faire de l'**Infrastructure As Code**?

### Infrastructure Versionning & Historisation

Il peut vite devenir très compliqué de mettre en place et de maintenir des infrastructures Cloud si on ne fait pas attention à comment on s'y prend. Et c'est d'ailleurs là que beaucoup d'entre nous nous précipitons dans un premier pièges malgré nous : **[L'AWS Console](https://aws.amazon.com/fr/console/)**.

Cette Console est une interface Web, extrêmement bien faite qui permet de gérer absolument tous les services - quelques fonctionnalité peuvent parfois s'avérer cachée ou difficilement accessible, mais dans la globalité vous ne serai jamais perdu dans cette console.
Cette console est en fait tellement pratique à utiliser, qu'on finit par y passer beaucoup trop de temps. Oubliant parfois au passage de noter les modifications que l'on vient d'apporter, pourquoi nous les avons mises en place, ou pire encore, quelles étaient les précédents services, configurations et paramètres en place !

Pour éviter ce genre de déboire, il est essentiel de passer par des scripts de configuration de votre infrastructure, que vous pourrez facilement maintenir dans un repos GIT -ou tout autre outils de source control à votre goût- et ainsi profitez de tout l'écosystème que vous avez déjà très certainement en place pour votre code applicatif.

Versionning, Changelogs, travail collaboratif, auto-documentation... Il n'y a aucune raison pour ne pas immédiatement adopté cette pratique.

Et vous vous rendrez très rapidement compte qu'au final, ce n'est pas si compliqué tellement la grande majorité des documentations sont bien réalisées et les ressources annexes sont abondantes.

### Facilitez vos mises à jour et vos déploiement

Ce n'est un secret pour personne, les procédures de mise à jour manuelle tel que le déploiement d'une nouvelle version de votre site web, une update de votre application, etc... Sont un vrai calvaire pour le commun des mortels. Il en va de même pour les Infrastructure ! Mettre à jour manuellement des ressources, ajouter manuelle des services, modifier des configurations, ... 

Pour les exacts même raison que l'automatisation est devenu un standard incontournable dans le développements web ou applicatif, le scripting de vos infrastructures devient lui aussi indispensable.

Modifier 1 à 2 lignes de scripts, exécuter la validation pour vérifier les potentielle erreurs, générer un rapport précisant les actions qui seront menées, et enfin publier les modifications lorsqu'on est satisfait des étapes précédentes, le tout en seulement 3 clics ou 2 lignes de commandes : ça n'a pas de prix. Que ce soit pour vos nerfs, pour la gestion des risques immuables à toutes modifications ou simplement pour le gain de temps (et souvent donc d'argent), encore une fois vous avez toutes les raisons d'adopter cette pratique.

### Disponibilité et déduplication de vos infrastructures

On en parle assez peu, car on espère toujours que tout se passe bien, mais qu'allez-vous faire le jour ou votre infrastructure sera inaccessible indépendamment de votre volonté (oui car je préfère partir du principe que ça arrivera afin d'y être pret, et je vous le conseil également) ?

> AWS nous a habitué à une disponibilité des services à toutes épreuves, mais **le risque zéro n'existe pas**.

Il est tout à fait possible qu'un jour l'une des 16 régions AWS tombe, même si ce scénario et difficilement envisageable, des incidents moins importants peuvent toujours entrainer du simple disfonctionnement à une indisponibilité majeure de votre infrastructure.

Alors que faire dans cette situation ? Bien sûr vous aurez des snapshots, et autre backup répliqué dans d'autre régions, mais je vous mets au défi de réussir à remonter toute votre infrastructure en seulement quelques minutes et avec un minimum d'effort si vous n'avez pas préalablement scripter le tout !

> L'un des avantages des scripts, est qu'ils sont paramétrables et ré-exécutable.

Exécuter votre script de création d'infrastructure dans une nouvelle région devient alors d'une facilité presque déconcertante. Au-delà même de pouvoir remonter une nouvelle infrastructure, dupliquez votre infrastructure, pour par exemple mettre en place des environnements de développement, stagging, test, ... devient une simple formalité. Mieux encore, vous pourrez également les détruire tout aussi facilement et donc ne pas avoir à supporter des coûts constants, mais seulement à la demande.

Il n'aura jamais était aussi simple de tester, éprouver, expérimenter, ... sur de telles échelles !

## Comment scripter ses Infrastructures ?

Maintenant que vous êtes convaincus de vous mettre à l'**Infrastructure as Code**, par où commencer ?

> La première chose et de bien choisir ses outils. Dois-je faire des script *shell* utilisant directement l'**[AWS CLI](https://aws.amazon.com/fr/cli/)** ? Dois-je développer des exécutables utilisant le **[SDK AWS](https://aws.amazon.com/fr/tools/#sdk)** dans mon langages favoris? **Evidemment non !**

Seuls les plus téméraires si risquerons, et bien même si vous êtes très confiant, vous allez très rapidement obtenir des projets extrêmement compliqués à lire, à maintenir et au final à utiliser. Ils faut donc utiliser des outils, et comme bien souvent quand on parle de Cloud, plusieurs solutions existes : CloudFormation, Terraform, Ansible, Salt, etc... Pour ne citer que ceux-là.

Je n'ai personnellement testé que **[Terraform](https://www.terraform.io/)** et **[CloudFormation](https://aws.amazon.com/fr/cloudformation/)**. Et mon choix final s'est arrêté sur *CloudFormation* fournit par... **AWS**. Pourquoi aller chercher loin quand on a le nécessaire à porter de main ! AWS CloudFormation étant au plus près qu'il est possible de l'être du fournisseur de services *(puisqu'il est lui-même un de ses services, vous suivez ?)*, son support des nouvelles fonctionnalités, ses corrections de bugs et surtout sa documentation sont au plus haut niveau possible.

### Correctement s'outiller

Maintenant que vous avez choisi votre langage de script, il vous reste comme pour tout langage de programmation, à vous outiller correctement pour pouvoir l'utiliser et l'exploiter au mieux !

Evidemment rien ne vous empeche d'utiliser un bon vieux `notepadd++` et de commencer à écrire vos premières lignes d'Infrastructure. Vous devriez cependant rapidement constater que les premiers freins vont vite apparaitre sans autocompletion, suggestions, validation *in-place* du document etc... C'est pourquoi je vous conseille vivement l'utilisation d'un IDE, ou plus exactement du plugin nécessaire pour votre IDE préféré.

Dans mon cas, étant spécialisé dans les technologie Microsoft .Net Web, j'ai tout naturellement jeté mon dévolu sur Visual Studio. La version *[community](https://www.visualstudio.com/fr/vs/community/)* est gratuite tant que vous êtes un developpeur individuels. Pour les entreprises quelques conditions s'applique que je vous laisse découvrir [ici en bas de page](https://www.visualstudio.com/fr/vs/community/). J'ai également testé rapidement [Visual Studio Code](https://code.visualstudio.com/) (100% gratuit et Open Source), mais le plugin CloudFormation manque de quelques features qu'il devient difficile de se passer quand on s'y est habituées (validation des références, conditions, dépendances...).

En plus du très bon plugin [AWS Toolkit for pour Visual Studio](https://aws.amazon.com/fr/visualstudio/), vous pourrez avoir au sein d'une même solution, vos templates CloudFormation, vos scripts et autres utilitaires, ainsi que les projets de vos applications *(nodejs, .NetCore, ...)*. Cela peut parraitre un peu exagéré de tout centraliser dans une même solution VS, mais dans certain cas ou le code est extrêmement dépendant de votre infrastructure, c'est presque indispensable *(oui `Lambda fonction`, je parle pour toi)*.

> Attention si vous utilisez le `Dark theme` sous VS2015, une petite modification de couleur vous sera nécessaire pour rendre lisible vos templates. Allez dans `Tools` > `Options` > `Environment` > `Fonts and Colors`, et reconfigurez les couleurs pour les propriétés commençant par `CloudFormation Template` qui ne sont pas lisible.

Une fois votre IDE configuré selon vos besoins, il ne vous manque plus qu'a installer l'[AWS CLI](https://aws.amazon.com/fr/cli/) si ce n'est pas déjà fait. Vous pourriez utiliser la console pour déployer vos templates, mais comme déjà évoqué plus haut, vous devriez tout de suite vous habituer à vous en passer.
L'**AWS CLI** installée, n'oubliez pas de la configurer, pour cela vous pouvez suivre une fois encore la [très bonne documentation qui l'accompagne](http://docs.aws.amazon.com/fr_fr/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration).

Maintenant prêt, il ne vous reste plus qu'a créer votre premier template en créant un nouveau projet CloudFormation: `File` > `New` > `Project` > `AWS` > `AWS CloudFormation Project`.

### Par où commencer ?

Vous voilà fin pret à écrire votre première ligne de code, mais par où commencer sans se noyer sous une tonnes de `Resources` et configuration pas toujours simple à comprendre dès le premier template?

Mon conseil, si vous n'avez jamais fait d'infrastructure as code, sera de commencer léger. Ne chercher pas à scripter immédiatement tous vos services. Ciblez un point en particulier que vous maitrisez suffisamment et qui ne devrait pas représenter un défi trop ambitieux. Vous voyez cette petite application web, avec son Load balancer ? Ça me semble être un exercice assez standard et particulièrement courant, qui vous permettra de très facilement trouver de l'aide et des ressources lors de vos premiers blocages.

### Dans la pratique, à quoi ça ressemble ?

Sans plus de supsens, quelque ligne de code pour lesquels j'ai choisi le JSON *(par simple affinitté personnel)*, mais sachez que CloudFormation supporte également le YAML.

```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "My auto-scalled, laod balanced, web application",
    "Metadata": {},
    "Parameters": {},
    "Mappings": {},
    "Conditions": {},
    "Resources": {},
    "Outputs": {}
}
```

Je ne vais pas aller dans le détails de chaque propriétés, comme je l'ai déjà indiqué plus haut, [la documentation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html) est très bien faite et devrez répondre à toutes vos questions.

Ce qui nous intéresse tout particulièrement ici sont les propriétés : `Parameters`, qui comme son nom l'indique vont nous permettre de configurer les paramètres de notre template comme la région, le nom des ressources, etc... Ainsi que la propriété `Resources` qui vas nous permettre de définitr un à un les services et leurs configurations que nous voulons mettre en place.

Commençons immédiatement notre script avec une simple Instance EC2 configurée pour héberger une application web.

```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "Simple AWS Linux instances",
    "Resources": {
       "StaticWebAppInstance" : {
           "Type" : "AWS::EC2::Instance",
           "Properties" : {
               "ImageId" : "ami-82be18ed",
               "InstanceType" : "t2.micro"
           }
       }
    }
}
```
Et pour déployer cette stack vous n'avez plus qu'à exécuter cette ligne de commande :
> `aws cloudformation deploy --template-file web-app.template --stack-name web-app`

Vous pouvez suivre l'évolution de la création de la stack dans votre [console AWS](https://console.aws.amazon.com/cloudformation/home), et vous devriez voir également apparaitre votre instance `Running` dans la [section EC2 de la console](https://console.aws.amazon.com/ec2/v2/home), félicitations vous venez d'entrer dans le monde de l'Infrastructure as Code !

Pour supprimer cette stack et l'instance EC2 associé, une nouvelle ligne de commande fera le tout pour vous :
> `aws cloudformation delete-stack --stack-name web-app`

**Vous êtes maintenant pret à créer vos propres templates et déployer vos premières stack !**

J'espère que cet article vous aura été utile, dans le prochain nous créerons étape par étape un un template permettant de déployer toute la configuration réseau standard d'un nouveau VPC pouvant ensuite servir pour n'importe quel type d'infrastructure.

<br><br><br><br>

[Home](/)