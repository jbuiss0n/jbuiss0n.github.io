# AWS Cloudformation: Création d'un Infrastucture réseau complète

> Dans cet article je vous propose de réaliser un template CloudFormation permettant de déployer très simplement un réseau VPC standard embarquant toute la configuration nécessaire et standarisé pour n'importe quelle infrastructure, ce qui est souvent désigné sous l'appelation: **NetWord**.


## Avant de commencer

Si vous n'etes pas familier avec CloudFormation ou que vous n'en voyez pas encore l'interet je vous invite à commencer votre lecture par cet article qui devrez éclairer sur ce que nous allons faire ici: [[AWS] Infrastructure As Code: Pourquoi et Comment?](aws-infrastructure-as-code)

## Définition de l'Infrastrcucture Réseau

Nous infrastructure sera basé sur un VPC, comportant deux zones, une publique et une privé. La zone publique sera accessible depuis internet, alors que la zone privé ne sera elle pas visible en dehors de notre VPC. Nous définirons aussi les paramètres DHCP, les Subnets, etc...

Nous devons donc définir les ranges d'IP pour les blocks [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) de notre `VPC` et des ses `Subnets`. Sachant qu'il y a de 2 à 6 zone de disponibilité par région, nous allons prévoir 12 blocks CIDR pour nos subnets: un dans chaqu'une des zones privé et publique. 

Nous pouvons partir sur une configuration très standard, à savoir:
- CIDR VPC: `10.1.0.0/16`
- CIDR Block 1: `10.1.1.0/24`
- CIDR Block 2: `10.1.2.0/24`
- CIDR Block 3: `10.1.3.0/24`
- [...]

Il va également falloir maintenir un mapping des régions et de leur nombre de zone de disponibilité, pour pouvoir dynamiquement choisir si l'on doit créer ou non certain `subnet`.

## Définition des ressources nécessaire

Nous aurons donc un `VPC` et 4 à 12 `Subnets`. Ce ne sera évidement pas suffisant, nous avons besoin d'une resource `DHCPOptions` et d'une resource `VPCDHCPOptionsAssociation` afin de définir un nom de domaine interne à notre réseau. Celui-ci pourra ensuite être géré directement dans [`Route53`](https://aws.amazon.com/route53/), le service de gestion DNS d'AWS en définissant une ressource `VPCHostedZone`.

Nous allons ensuite devoir créer une `InternetGateway`, pour donner accès et rendre accessible depuis Internet à notre zone **publique**.
De la même manière nous créerons une `NatGateway` et son `EIP` pour donner accès à Internet, et au services AWS tel que `S3`, `CodeDeploy`, ..., à notre zone **privée* sans pour autant que celle-ci puisse être accessible depuis Internet.
Puis nous créerons nos `Route` et `RouteTable` permettant de lié le tout à l'aide des `SubnetRouteTableAssociation`, pour finallement créer une `SecurityGroup` par défaut que l'on pourra personaliser pour limiter les accès à notre Infrastructure.

> Retrouvez l'ensemble du code de cette stack à la fin de cet article.

Nous avons maintenant théoriquement de quoi déployer notre Netword, mais il est fastidieux de devoir ensuite utiliser la console (web ou cli) pour obtenir les informations relatices aux ressource créées, pour ensuite les renseigner de nouveaux dans nos autres templates qui dépendront de ce Netword...

C'est la que nous pouvons utiliser la propéiété `Outputs` du template ! Celle ci nous permet en effet de rendre disponiblers des attributs des ressources que nous avons créé en déploiement une nouvelle stack utilisant ce template.

> Vous pouvez télécharger le template directement sur mon [Github ici](https://github.com/jbuiss0n/cloudformation/blob/master/netword.template).

Nous pouvons maintenant exporter les identifiants de notre `VPC`, de nos `Subnets` et de notre `SecurityGroup` et les exploiter dans un nouveau template, c'est que nous allons faire maintenant, en reprenant le template créer dans l'article [[AWS] Infrastructure As Code: Pourquoi et Comment?](aws-infrastructure-as-code) et disponible [ici](https://github.com/jbuiss0n/cloudformation/blob/master/simple-ec2.template).

Dans ce précédent article, nous avions créé un template déploiement une Instance EC2 AWS Linux, et bien que nous gérions dynamiquement toutes les régions disponible, nous devions cependant fournir les paramètres `SecurityGroupIds` et `SubnetId` à  chaque création d'une nouvelle Stack.
A la place nous allons créer un unique paramètre qui sera le nom de la `Stack` référence pour notre `Netword` et utiliser les fonctions intrinsèque [`Fn::ImportValue`](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-importvalue.html) et [`Fn::Sub`](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-sub.html) récupérer nos dépendances :

```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "A simple EC2 Instance",
    "Parameters": {
    [...]
		"NetwordStackName": {
			"Description": "Name of an active CloudFormation stack that contains the networking resources, such as the vpc, subnet, security group, ... That will be used in this stack.",
			"Type": "String",
			"MinLength": 1,
			"MaxLength": 255,
			"AllowedPattern": "^[a-zA-Z][-a-zA-Z0-9]*$"
		}
    },
    "Resources": {
       "StaticWebAppInstance" : {
           "Type" : "AWS::EC2::Instance",
    [...]
               "NetworkInterfaces": [
                    {
                        "DeviceIndex": "0",
                        "AssociatePublicIpAddress": "true",
                        "SubnetId": { "Fn::ImportValue": { "Fn::Sub": "${NetwordStackName}-PrivateSubnetA" } },
                        "GroupSet": [{ "Fn::ImportValue": { "Fn::Sub": "${NetwordStackName}-DefaultSecurityGroup" } }]
                    }
                ]
    [...]
}
```

> Le template complet est disponible également sur mon [Github ici](https://github.com/jbuiss0n/cloudformation/blob/master/simple-ec2.netword.template).

Il ne reste plus qu'à déployer nos 2 stacks:

> `aws cloudformation deploy --template-file netword.template --stack-name netword --region eu-central-1`
> `aws cloudformation deploy --template-file wsimple-ec2.template --stack-name simple-ec2 --region eu-central-1 --parameter-overrides NetwordStackName=netword`

Et voilà, le tour est joué, nous avons un premièr templier `netword` permettant de créer des Infrastructures réseaux complète très simplement, ainsi qu'un exemple de template `simple-ec2` exploitant notre Netword !