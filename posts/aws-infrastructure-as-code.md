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
L'**AWS CLI** installée, n'oubliez pas de la configurer, pour cela vous pouvez suivre une fois encore la [très bonne documentation qui l'accompagne](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration).

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

Si vous utilisez Visual Studio, vous pouvez aussi simplement faire un clic droit sur le fichier dans l'explorateur de Solution et choisir `Deploy to AWS cloudformation` et vous laissez guider. Mais encore une fois, je vous conseil de vous habituer aux lignes de commande, que vous pourrez facilement versionner, réutiliser, automatiser...

### Créer un template paramétrable et réutilisable

Notre précédent template est très bien pour servir d'exemple mais ne correspond en aucun cas à quelques chose que vous feriez dans la vrai vie. Nous allons donc ajouter différentes options pour le rendre plus proche d'un véritable cas d'utilisation. Pour cela, je vais commencer par définir l'interface réseau de cette instance, en lui assignant une IP publique, un `Subnet` ainsi qu'un `Security Group`. Nous allons également lui donner le nom d'une `KeyPair` nous permettant de nous connecter en SSH. *Si vous n'avez pas encore de `KeyPair` la documentation vous expliquera tout le nécessaire [ici](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html).*

```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "Simple AWS Linux instances",
    "Resources": {
       "StaticWebAppInstance" : {
           "Type" : "AWS::EC2::Instance",
           "Properties" : {
               "ImageId" : "ami-82be18ed",
               "InstanceType" : "t2.micro",
               "KeyName" : "XXXXX",    
               "NetworkInterfaces": [
					{
						"DeviceIndex": "0",
						"AssociatePublicIpAddress": "true",
						"GroupSet": ["sg-XXXXX"],
						"SubnetId": "subnet-XXXXX"
					}
				]
           }
       }
    }
}
```
*A vous de remplire les valeurs pour les propriétés `KeyName`, `GroupSet` et `SubnetId`, vous pouvez pour cela utiliser le VPC par défaut ainsi que le SecurityGroup par défaut de la région. Attention cependant à bien vous donner les accès nécessaires.*

> Avant de mettre à jour notre template, il y a une chose **très importante** à savoir: La modification de certaines propriétés entraine un re-création de toute la ressource.
Dans notre cas cela signifie que l'instance déjà deployée sera **détruite** et qu'une nouvelle instance sera créée. Soyez toujours vigilant et vérifiez le comportant des propriétés que vous modifiées via la docs, afin d'anticiper le comportement d'un update. Dans le cas de [`KeyName`](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html#cfn-ec2-instance-keyname) et [`NetworkInterfaces`](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html#cfn-ec2-instance-networkinterfaces), on peut voir `Update requires: Replacement` qui nous renvoie vers [cette explication](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-replacement). L'instance sera donc fraichement recréé à partir de zéro à la modification des ces propriétés.

Avec ce comportement en tête, mettons à jour notre stack, en utilisant la même commande que leur de sa création:
> `aws cloudformation deploy --template-file web-app.template --stack-name web-app --region eu-central-1`

Notre template commence à ressemble à quelques chose de viable, on peut par exemple maintenant s'y connecter via SSH:
> `ssh -i /CHEMIN/VERS/VOTRE/KEYPAIR.pem ec2-user@IP_PUBLIQUE_DE_LINSTANCE`

Cependant ce template ne fonctionne que pour un VPC en particulier, dans une zone en particulier, n'est pas très explicite, et difficilement ré-utilisable hors de son contexte de création bien définit. Nous allons donc ajouter quelques paramètres.

```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "Simple AWS Linux instances",
    "Parameters": {
        "Owner": {
            "Type": "String"
        },
        "Project": {
            "Type": "String"
        },
        "Environment": {
            "Type": "String"
        },
        "SecurityGroupIds" : {
            "Type" : "List<AWS::EC2::SecurityGroup::Id>"
        },
        "SubnetId" : {
            "Type" : "AWS::EC2::Subnet::Id"
        },
        "ImageId" : {
            "Type" : "AWS::EC2::Image::Id"
        },
        "InstanceType" : {
            "Type" : "String"
        },
        "KeyName" : {
            "Type" : "String"
        }
    },
    "Resources": {
       "StaticWebAppInstance" : {
           "Type" : "AWS::EC2::Instance",
           "Properties" : {
               "ImageId" : { "Ref" : "ImageId" },
               "InstanceType" : { "Ref" : "InstanceType" },
               "KeyName" : { "Ref" : "KeyName" },    
               "NetworkInterfaces": [    
                    {
                        "DeviceIndex": "0",
                        "AssociatePublicIpAddress": "true",
                        "GroupSet": { "Ref" : "SecurityGroupIds" },
                        "SubnetId": { "Ref" : "SubnetId" }
                    }
                ],
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": { "Fn::Join": [".", ["ec2", { "Ref": "Owner" }, { "Ref": "Project" }, { "Ref": "Environment" }]] }
                    },
                    {
                        "Key": "Stack",
                        "Value": { "Ref": "AWS::StackName" }
                    },
                    {
                        "Key": "Environment",
                        "Value": { "Ref": "Environment" }
                    }
                ]
           }
       }
    }
}
```

Nous avons définit toute une série de `Parameters`, que nous utilisons ensuite via l'instruction `{ "Ref" : "XXXXX" }`. Cette instruction nous permet de faire une substitution avec la valeurs de ce paramètre. Cette instruction peut également servir de référence vers une autre `Resource` du template si nous avions des dépendances.
Nous utilisons également le [pseudo paramètre](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html) `AWS::StackName`. Les pseudo paramètre ne sont autres que des paramètres directement définit par CloudFormation lors de l'éxécution du template.
Enfin, nous utilisons également une [fonction intrinsèque](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html) qui nous permet de concaténer des valeurs pour former une chaine de caractère. Dans notre cas nous nous en servant pour définir le nom de notre instance en fonctionn de plusieurs paramètres du template, via le `Tag Name`.

C'est déjà beaucoup mieux, ils nous suffit maintenant de spécifier nos paramètres via l'habituelle ligne de commande pour mettre à jour notre stack:
> `aws cloudformation deploy --template-file web-app.template --stack-name web-app --region eu-central-1 --parameter-overrides SecurityGroupIds=sg-XXXX,sg-XXXX SubnetId=subnet-XXXX ImageId=ami-82be18ed InstanceType=t2.micro KeyName=XXXX Owner=jbuiss0n Project=webapp Environment=test`

Notre template s'éxécute donc avec les valeurs suivantes, que vous pouvez remplacer à volonté en éxecutant ce même template pour créer plusieurs stack:
- `SecurityGroupIds` = sg-XXXX,sg-XXXX *(à vous de compléter ce paramètres en fonction de votre configuration AWS et de votre VPC)*
- `SubnetId` = sg-XXXX,sg-XXXX *(à vous de compléter ce paramètres en fonction de votre configuration AWS et de votre VPC)*
- `ImageId` = ami-82be18ed
- `InstanceType` = t2.micro
- `KeyName` = XXXX *(à vous de compléter ce paramètres en fonction de votre configuration AWS et de votre VPC)*
- `Owner` = jbuiss0n
- `Project` = webapp
- `Environment` = test

Ce qui une fois correctement déployer nous donne par exemple les `Tags` suivant sur notre instance:
- `Name` = ec2.jbuiss0n.webapp.test
- `Stack` = web-app
- `Environment` = test

*Vous pouvez également constater l'éxistance de `Tags` généré directement par cloudformation dans la console.*

Il y a une vrai progression, mais au final le deploiement d'une nouvelle stack à partir de ce template est aussi complexe que la création d'une instance à partir de l'instruction `aws ec2 create-instance [...]` de l'**AWS CLI**. Nous allons donc une nouvelle fois améliorer notre template, en incorporante notemment les notions de [`Mappings`](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html) et de [`Conditions`](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/conditions-section-structure.html), en définissant des paramètres par défaut, etc...

```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "Simple AWS Linux instances",
    "Parameters": {
        "Owner": {
            "Type": "String",
            "Default": "jbuiss0n"
        },
        "Project": {
            "Type": "String",
            "Default": "webapp"
        },
        "Environment": {
            "Type": "String",
            "Default": "test"
        },
        "SecurityGroupIds" : {
            "Type" : "List<AWS::EC2::SecurityGroup::Id>"
        },
        "SubnetId" : {
            "Type" : "AWS::EC2::Subnet::Id"
        }
    },
    "Mappings" : {
        "RegionMap" : {
            "us-east-1": {
                "AwsLinuxAmi" : "ami-a4c7edb2",
                "Ec2KeyPair": ""
            },
            "us-east-2": {
                "AwsLinuxAmi" : "ami-8a7859ef",
                "Ec2KeyPair": ""
            },
            "us-west-1": { 
                "AwsLinuxAmi" : "ami-327f5352",
                "Ec2KeyPair": ""
            },
            "us-west-2": { 
                "AwsLinuxAmi" : "ami-6df1e514",
                "Ec2KeyPair": "XXXXX"
            },
            "eu-west-1": { 
                "AwsLinuxAmi" : "ami-d7b9a2b1",
                "Ec2KeyPair": "XXXXX" 
            },
            "eu-west-2": { 
                "AwsLinuxAmi" : "ami-ed100689",
                "Ec2KeyPair": "XXXXX" 
            },
            "ca-central-1": { 
                "AwsLinuxAmi" : "ami-a7aa15c3",
                "Ec2KeyPair": ""
            },
            "eu-central-1": { 
                "AwsLinuxAmi" : "ami-82be18ed",
                "Ec2KeyPair": "XXXXX"
            },
            "ap-southeast-1": { 
                "AwsLinuxAmi" : "ami-77af2014",
                "Ec2KeyPair": ""
            },
            "ap-southeast-2": { 
                "AwsLinuxAmi" : "ami-10918173",
                "Ec2KeyPair": ""
            },
            "ap-northeast-1": { 
                "AwsLinuxAmi" : "ami-3bd3c45c",
                "Ec2KeyPair": ""
            },
            "ap-northeast-2": { 
                "AwsLinuxAmi" : "ami-e21cc38c",
                "Ec2KeyPair": ""
            },
            "ap-south-1": { 
                "AwsLinuxAmi" : "ami-47205e28",
                "Ec2KeyPair": ""
            },
            "sa-east-1": { 
                "AwsLinuxAmi" : "ami-87dab1eb",
                "Ec2KeyPair": ""
            }
        },
        "Environments": {
            "prod": {
                "InstanceType": "t2.medium"
            },
            "test": {
                "InstanceType": "t2.micro"
            }
        }
    },
    "Conditions" : {
        "HasEc2KeyPair" : { "Fn::Not" : [{ "Fn::Equals" : [{ "Fn::FindInMap" : ["RegionMap", { "Ref" : "AWS::Region" }, "Ec2KeyPair"] }, ""] }] }
    },
    "Resources": {
       "StaticWebAppInstance" : {
           "Type" : "AWS::EC2::Instance",
           "Properties" : {
               "ImageId" : { "Fn::FindInMap" : ["RegionMap", { "Ref" : "AWS::Region" }, "AwsLinuxAmi"] },
               "KeyName" : { "Fn::If": ["HasEc2KeyPair", { "Fn::FindInMap" : ["RegionMap", { "Ref" : "AWS::Region" }, "Ec2KeyPair"] }, { "Ref" : "AWS::NoValue" }] },
               "InstanceType" : { "Fn::FindInMap" : ["Environments", { "Ref" : "Environment" }, "InstanceType"] },
               "NetworkInterfaces": [
                    {
                        "DeviceIndex": "0",
                        "AssociatePublicIpAddress": "true",
                        "GroupSet": { "Ref" : "SecurityGroupIds" },
                        "SubnetId": { "Ref" : "SubnetId" }
                    }
                ],
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": { "Fn::Join": [".", ["ec2", { "Ref": "Owner" }, { "Ref": "Project" }, { "Ref": "Environment" }]] }
                    },
                    {
                        "Key": "Stack",
                        "Value": { "Ref": "AWS::StackName" }
                    },
                    {
                        "Key": "Environment",
                        "Value": { "Ref": "Environment" }
                    }
                ]
           }
       }
    }
}
```

Notre template commence à prendre de nouvelles dimentions! Nous avons maintenant un `Mapping` "RegionMap", qui nous permet de spécifier l'`Amazon Linux AMI` de chaque région, ainsi que la `KeyPair` que vous auriez défini dans chaque région. Mieux encore, la `Condition` "HasEc2KeyPair" nous permet de définir si vous avez ou non une `KeyPair` dans la région en cours de déploiement et donc de la fournir ou non à l'instance lors de sa création. Nous avons également un `Mapping` "Environments", permettant de définir des configuration différente en fonction de l'environement correspond à la stack en cours. Enfin, nous avons aussi définit des valeurs par défaut pour certains paramètres.

Quelques explication sur ce nouveau template:
- La condition `HasEc2KeyPair`: nous déduisons si la valeur de `Ec2KeyPair`, pour la région en cours de déploiement, est différente d'une chaine de caractère vide.
- La propriété `KeyName`: si la condition `HasEc2KeyPair` est vrai, nous allons chercher dans notre `RegionMap` définit plus haut cette clé. Sinon nous spécifions explicitement que nous ne voulons pas définir de valeur pour cette propriété via la constante `AWS::NoValue`.
- La propriété `ImageId`: nous allons encore chercher dans notre `RegionMap`, la valeur de l'AMI correspondant à la région en cours de déploiement.
- La propriété `InstanceType`: nous allons dans les `Environments` le type d'instance en fonction du paramètre `Environment`, ayant lui même une valeur par défaut à "test".

Il n'y a plus qu'a éxécuter notre nouvelle commande, maintenant beaucoup plus simplement:
> `aws cloudformation deploy --template-file web-app.template --stack-name web-app --region eu-central-1 --parameter-overrides SecurityGroupIds=sg-XXXX SubnetId=subnet-XXXX`

**Vous êtes maintenant pret à créer vos propres templates et déployer vos premières stack !**

J'espère que cet article vous aura été utile, il y a évidemment encore beaucoup de chose à dire et à faire sur la création des templates, mais cela fera l'objet d'un prochain article.

<br><br><br><br>

[Home](/)