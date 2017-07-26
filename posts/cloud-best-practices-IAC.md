# AWS Best practices: Infrastructure as Code

> Dans cet article nous allons l'un des principales bonnes pratique en terme de Cloud Computing à prendre en main le plus rapidement possible: Infrastructure as Code, ou comment scripter la création, la gestion et la configuration de vos infrastructure Cloud.

Parmis les nombreuses façon possible de faire de gérer et organiser ses infrastucture, une seule peu prétendre être **la** bonne pratique: le Script.

## Pourquoi scripter ses Infrastructures ?

### Infrastructure Versionning & Historisation

Il peut vite devenir très compliqué de mettre en place et de maintenir des infrastructure Cloud si on ne fait pas attention à comment on s'y prend. Et c'est d'ailleurs là qu'AWS nous précipite dans un premier pièges malgré lui: **L'AWS Console**.

Cette Console est une interface Web, extrèmement bien faite qui permet de gérer absolument tous les services - quelques fonctionnalité peuvent parfois s'avérer cachée ou difficilement accessible, mais dans la globalité vous ne serai jamais perdu dans cette console.
Cette console est en fait tellement pratique à utiliser, qu'on fini par y passer beaucoup trop de temps. Oubliant parfois au passage de noté les modifications que l'ont vient d'apporter, pourquoi nous les avons mise en place, ou pire encore, quelles étaient les précédents services, configurations et paramètres en place !

Pour éviter ce genre de déboire, il est essentiel de passer par des scripts de configuration de votre infrastructure, que vous pourrez facilement maintenir dans un repos GIT -ou tout autre outils de source control a votre goût- et ainsi profitez de de tout l'ecosysteme que vous avez déjà très certainement en place pour votre code applicatif.

Versionning, Changelogs, travail collaboratif, auto-documentation... Il n'y a aucune raison pour ne pas immédiatement adopté cette pratique.

Et vous vous rendrez très rapidement compte qu'au final, ce n'est pas si compliqué tellement la grande majorité des documentations sont bien réalisées les ressources annexes sont abondantes.

### Facilitez vos mise à jour et vos déploiement

Ce n'est un secret pour personne, les procédures de mise à jour manuelle tel que le déploiement d'une nouvelle version de votre site web, une update de votre application, etc... Sont un vrai calvaire pour le commun des mortels. Il en vas de même pour les Infrastructure! Mettre à jour manuellement des ressources, ajouté manuelle des services, modifier des configurations, ... 

Pour les exacts même raison que l'automatisation est devenu un standard incontournable dans le développements, le scripting de vos infrastructure devient indispensable.

Modifier 1 à 2 lignes de scripts, executer la validation pour vérifier les potentielle erreurs, générer un rapport précisant les actions qui seront mené, et enfin publier les modifications lorsqu'on est satisfait des étapes précédetentes, le tout en seulement 3 clics ou 2 lignes de commandes: ça n'a pas de prix. Que ce soit pour vos nerfs, pour la gestion des risques immuables à toutes modifications ou simplement pour le gains de temps et donc d'argent pour votre entreprise, encore une fois vous avez toutes les raison d'adopter cette pratique.

### Disponibilité et duplication de vos infrastrucures

On en parle assez peu, car on espère toujours que tout se passe bien, mais qu'allez vous faire le jour ou votre infrastrucure sera inaccesible indépendamment de votre volonté (oui car je préfère partir du principe que ça arrivera afin dy être pret, et vous devriez en faire autant :))?

> AWS nous a habitué à une disponibilité des services à toutes épreuves, mais **le risque zéro n'existe pas**. Il est tout à fait possible qu'un jour l'une des 16 régions AWS tombe, même si ce scénario et difficilement ensisageable, des incidents moins importants peuvent toujours entrainer des simples disfonctionnements à une indisponibilité majeur de votre infrastructure.

Alors que faire dans cette situation ? Bien sûr vous aurez des snapshots, et autre backup répliqué dans d'autre régions, mais je vous met au défit de réussir à remonter toute votre infrastructures en seulement quelques minutes et avec un minimum d'effort si vous n'avez pas préalablement scripter le tout !

> L'un des avantages des scripts, c'est qu'ils sont paramétrable. Executer votre scripts de création d'infrastructure dans une nouvelle région devient alors d'une facilité presque déconcertante.

Au delà même de pouvoir remonter une nouvelle infrastructure, dupliquez votre infrastructure, pour par exemple mettre en place des environements developement, stagging, tests, ... devient une simple formalité. Mieux encore, vous pourrez également les détruires tout aussi facilement.

Il n'aura jamais était aussi simple de tester, éprouver, expérimenter, ... sur de telles échelles!

## Comment scripter ses Infrastructure

Maintenant que vous êtes convaincus de vous mettre à l'**Infrastructure as Code**, par où commencer?

La première chose et de bien choisir ses outils. Dois-je faire des script *shell* utlisant directement l'**AWS CLI**? Dois-je developpez des executables utilisant le **SDK AWS** dans mon languages favoris?

Evidemment non, seuls les plus expérimenté si risquerons, et bien même si vous êtes très confiant, vous allez très rapidement obtenir des projets extrèmement compliqué à lire, à maintenir et au final à utiliser.

Ils faut donc utliser ses outils, et comme bien souvent quand on parle de Cloud, plusieurs solutions existes: Cloudformation, Terraform, Ansible, Salt, etc... Pour ne citer que ceux là.

Je n'ai personnellement testé que Terrform et Cloudformation. Et mon choix final s'est arrété sur Cloudformation fournit par... **AWS**. Pourquoi aller chercher loin quand on a le nécessaire à porter de main!

AWS Cloudformation étant au plus pret qu'il est possible de l'être du fournisseur de services *(puisqu'il est lui même un de ses services, vous suivez?)*, son support des nouvelles fonctionnalités, ses corrections de bugs et surtout sa documentation sont au plus au niveau possible.

### Par où commencer ?

Mon conseil, si vous n'avez jamais fait d'infrastructure as code, sera de commencer léger. Ne chercher pas à scripter immédiatement tous vos services. Ciblez un point en particulier que vous maitrisez suffisament et qui ne devrait pas représenter une défi trop ambitieux. Vous voyez cette petite application, avec son Load balancer et son auto-scalling? Ca me semble être un exercide assez standard et particulièrement courant, qui vous permettra de très facilement trouver de l'aide et des ressources lors de vos débuts.

### Dans la pratique, à quoi ça ressemble ?

Sans plus de supsens, quelque ligne de code pour lesquels j'ai choisi le JSON (par simple affinité personnel):

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








