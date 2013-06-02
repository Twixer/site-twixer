---
layout: post
title: "DSM - Design Structure Matrix"
description: "Un petit tour de Lattix et des DSM"
category: tuto
tags: [tuto, dsm, lattix, java, spring, dtangler, sonar]
published: true
---
{% include JB/setup %}

#Introduction
Dans le cadre de mon travail, j'ai été amené à étudier l'outil [Lattix][]. Cet outil permet de créer des matrices de structures de dépendances (*Design|Dependency Structure Matrix* ou *DSM*) et de les manipuler. L'objectif d'une DSM est la modélisation de l'architecture d'un projet et la définition de règles pour le maintien de la cohérence de cette architecture.
L'analyse demandée portait sur la capacité de l'outil à modéliser un gros projet sous Oracle utilisant beaucoup de schémas et de procédures stockées.  
Cet article va décrire mon retour d'expérience sur les DSM et donner un petit aperçu de Lattix.

---------------------------------------
#Démarche
J'avais deux axes à suivre pour mener à bien mon travail : 
1. monter en compétence sur la notion de DSM
2. monter en compétence sur Lattix 

Pour démarrer, j'avais sous la main un projet exemple utilsant une base de donnée Oracle 11g ainsi que la couche Java accédant à cette base au moyen du doublé Spring/Hibernate. Les fichiers à disposition étaient les suivants : 
- script de création des tablespaces
- script de création des users et des droits 
- dump d'une base de développement
- sources Java de la couche persistence avec Spring/Hibernate

L'idée était donc de me documenter sur les DSM et d'utiliser le projet pour manipuler Lattix et voir ce qu'il y avait sous le capot.

#DSM
--------------------------------------- 

##Définition
Le sigle DSM est utilisé pour **D**esign ou **D**ependency **S**tructure **M**attrix. Pour traduire mot à mot l'anglais, je dirais que c'est une matrice de la structure de dépendences d'un système. La DSM est une représentation sous forme de tableau (matrice) des liens entre les composants d'un système. Le champ d'application des DSM n'est pas uniquement l'informatique bien que ce soit ce domaine qui nous intéresse ici (cf. [DSM-conference][]).

##La cible
Dans le domaine de l'IT, l'approche DSM couvre les besoins de conception, de maintenance ou d'analyse d'une architecture. Si nous devons modéliser l'architecture d'un système, nous aurons comme premier réflexe l'utilisation d'UML et du diagramme de classes pour des langages objets ou alors nous passerons par un MCD/MPD si nous avons affaire à une base de données. Cette conception nous conduit à des schémas comme : ![cas simple de modélisation][simple_class_diagramm]
ou comme : ![cas de modélisation compliquée][complicated_class_diagramm]  
Il n'est pas tout de suite évident d'appréhender tous les liens et les dépendences entre les éléments. L'oeil humain est capable de noter la plus ou moins grande complexité du système mais identifier rapidement quel élément est en lien avec lequel ou lister rapidement tous les liens est bien plus compliqué.  
C'est là qu'interviennent des représentations comme la DSM. Celle-ci est capable de synthétiser en un tableau les liens entre chaque élément d'un système et de percevoir la structure du système.

##Contenu d'une DSM
Le schéma suivant représente un exemple de DSM : ![exemple de DSM][dsm_sample]  
Une DSM contient les mêmes éléments en abscisses et en ordonnées et ce dans le même ordre. Certaines DSM ne représentent pas le nom de l'élément en colonne mais en général un clic sur un élément du tableau met en évidence l'élément en colonne.  
En parcourant la colonne de l'élément **A**, on note sur chaque ligne les liens de cet élément avec les autres éléments si un entier est présent (on peut avoir un X, cette représentation dépend des produits). Donc dans notre exemple, **A** possède un lien vers **C** et **G**. En parcourant la ligne pour l'élément **A**, on peut noter tous les éléments qui lui font appel c'est-à-dire **B** et **F** dans notre exemple.
Donc pour résumer : 
- colonne Y : une colonne représente tous les éléments qui sont appelés par Y
- ligne X : une ligne représente tous les éléments qui font appel à X

##Système modulaire
Un système bien conçu doit être découpé en modules (ou composants ou couches suivant la littérature). Les différents modules doivent avoir une adhérence faible avec les autres modules. Par exemple, on pourrait avoir une application avec les modules suivants : 
1. module IHM
2. module services
3. module stockage de données  
Dans ce découpage, il est déconseillé de créer un lien entre le module IHM et le module stockage de données.  
Ce découpage en modules et le découplage entre chacune permet de limiter les impacts en cas de modification de design au sein d'un module. On peut par exemple modifier la façon dont on stocke/accède aux données sans pour autant impacter la partie IHM.     
Une fois ces concepts posés, c'est là que l'approche par la DSM est intéressante. 

##Analyse d'un système, la théorie
L'approche DSM permet d'identifier rapidement les éléments de haut et bas niveau et d'identifier les modules du système étudié. Dans la matrice, les premiers éléments font partie des modules de haut niveau qui ne sont pas accédés par les autres éléments. Les derniers éléments de la liste représentent le coeur de l'application, les classes utilitaires accédées par la plupart des autres modules. On pourrait catégoriser ce module comme le framework coeur de l'application. Seul le triangle inférieur doit posséder des chiffres, dans le cas contraire cela signigie qu'il y a un problème de conception et qu'un refactoring est nécessaire (dépendance cyclique possible). 
Exemple d'un système découpé en modules fortement couplées :  
![dsm_lattix_01][]
Voici un autre exemple de système où les modules sont bien découpées et où un module n'accède qu'au module inférieure :  
![dsm_lattix_02][]  
Dans la réalité, même si ce dernier découpage semble idéal, il est bien difficile de l'atteindre.

#Analyse d'un système, la pratique
Voici un exemple de DSM pour la bibliothèque [springframework][] :
![dsm_lattix_springframework_dtangler][]  
On constate que le framework est plutôt bien architecturé avec en haut les éléments de haut niveau, en dans en bas le coeur de la bibliothèque avec les packages *core* et *util*. Il n'y a pratiquement pas d'entier dans le triangle supérieur.  
Voici un autre exemple avec la bibliothète *commons-collection* de la [fondation Apache][]. Cette fois on peut voir un DSM de la même bibliothèque mais le premier est généré avec Sonar et le second avec [Dtangler][]  :  
![dsm_lattix_commons-collections_sonar][] 
![dsm_lattix_commons-collections_dtangler][]  
On peut constater que les classes dans le package *org.apache.commons.collecions* (colonne 8), font appel à beaucoup d'autres packages et sont appelées par tous les éléments de la bibliothèque. De même le package *list* est appelé par beaucoup d'autres.  
Sans avoir à concevoir le modèle de classe en UML on identifie donc très rapidement avec une DSM l'architecture d'une bibliothèque et les points faibles de celle-ci.


##Pour aller plus loin
Ce chapitre liste quelques sites par lesquels je suis passé pour m'approprier les notions DSM.

###Wikipedia
Comme d'habidute, Wikipedia est la première ressource disponible pour avoir une définition générale du concept : [DSM (Wikipedia)][]. La page reste très verbeuse et une présentation plus graphique est nécessaire pour illustrer les concepts.

###Lattix
Lattix met à disposition quelques vidéos de démonstration sur son site ([Lattix Tuto][]) qui permettent de comprendre les notions basiques sur les DSM.  
Une collaboration entre lattix et un professeur du MIT a conduit à la publication d'un papier sur l'usage des DSM dans la conception d'une architecture : [Lattix and MIT][]. Ce papier n'est plus disponible gratuitement sur le site en question, mais il est encore accessible sur le site de Lattix ([oopsla05.pdf][]).
Le suivi des tutoriels vidéos de Lattix et la lecture attentive de la publication du MIT devrait donner une bonne idée des concepts qui se cachent derrière les DSM.

###NDepend
C'est sur le site de NDepend que j'ai finalement trouvé les meilleurs schémas explicatifs : [DSM par NDepend][].  
Plusieurs exemples d'architectures modélisées en DSM sont présentées et permettent de mieux comprendre la puissance du DSM pour comprendre, concevoir et maintenir une architecture.

---------------------------------------
#Lattix

Afin de tester le produit, je me suis appuyer sur mon projet exemple avec une base de données Oracle ainsi que sur des projets Java (personnel ou public).  
Pour utiliser le produit [Lattix][] il faut obtenir une licence sur le site de l'éditeur.  

##Les types de projets
Lattix permet de créer des DSM à partir de plusieurs sources : 
- projets Java (fichiers jar ou classes)
- projets Hibernate ou Spring (fichiers de configuration XML)
- projets base de données Oracle (connexion directe à la base)
- projets .Net
- ...

Un premier reproche qui peut être fait à Lattix est qu'il ne prend pas les fichiers sources Java en entrée mais les classes compilées ou les fichiers JAR. Cela a été un frein pour des projets dont j'avais une partie des sources mais pas toutes les dépendances. Je ne pouvais pas compiler mes sources et je n'ai donc pas réussi à intégrer le projet dans Lattix.  
Un second reproche concerne les projets Oracle. Je n'ai pas réussi à intégrer mon projet à partir du script de création d'un schéma. Il me fallait un accès direct à la base Oracle pour pouvoir mener l'analyse. Cela a donc impliqué de m'installer l'environnement Oracle 11g en local.

##Manipulation dynamique
Par rapport à un outil comme Sonar, Lattix permet une manipulation dynamique de la DSM produite. Il est par exemple possible de monter ou descendre les éléments et de voir directement l'impact en terme de métriques. On peut masquer les lignes qui ne nous intéressent pas et également créer des sous-packages et réduire ces nouveaux packages pour augmenter la lisibilité de la matrice.  
L'outil aurotirse donc une manipulation dynamique du DSM qui permet réellement de concevoir un nouveau design pour notre système.  

##Partitionnement
Lattix est capable de partitionner la matrice produite. Derrière le partitionnement se cache des algorithmes qui vont permettre de réaliser des regroupements d'éléments dans la matrice. Avec la création dynamique de packages, il est donc possible de faire des tests de re-arrangement les éléments. La finalité est de réaliser un refactoring d'un système dont les liens ne seraient pas corrects.

##Règles
Lattix permet d'ajouter des règles pour maintenir la cohésion du système. Il est par exemple possible de définir deux types de règles : 
- peut utiliser : permet de dire qu'un élément X peut faire appel à Y
- ne peut pas utiliser : permet de dire qu'un élément X ne peut pas faire appel à un élément Y
Ces règles sont visualiser graphiquement et les manquements sont mis en évidence.

---------------------------------------
#Conclusion
L'analyse du produit Lattix m'a permis d'aborder et de comprendre l'approche DSM. J'avais déjà été confronté à cette représentation sans y prêter attention avec par exemple [Sonar][] dans le monde Java. L'analyse de l'architecture d'un projet sous Sonar sera maintenant d'autant plus simple et rapide.  
Lattix se révèle être un produit assez puissant par rapport à un outil comme Sonar ou [Dtangler][]. Les fonctionnalités de Lattix permettent réellement à un architecte de réaliser son travail en amont comme en aval de la réalisation d'un projet. En amont, il permet de définir des règles pour une architecture et de vérifier que ces règles sont maintenues. En aval, c'est un bon outil pour appréhender un nouveau système inconnu et identifier rapidement les choix de conception et les éventuels problèmes.

---------------------------------------
#Ressources

- [Lattix][]
- [DSM-conference][]
- [DSM (Wikipedia)][]
- [Lattix Tuto][]
- [Lattix and MIT][]
- [oopsla05.pdf][]
- [DSM par NDepend][]
- [Sonar][]
- [dtangler][]

<!-- Ressources URL -->
[Lattix]: http://www.lattix.com/

[fondation Apache]: http://commons.apache.org/proper/commons-collections/
[springframework]: http://www.springsource.org/spring-framework
[DSM-conference]: http://www.dsm-conference.org/
[DSM (Wikipedia)]: http://en.wikipedia.org/wiki/Design_structure_matrix

[Lattix Tuto]: http://www.lattix.com/node/72
[Lattix and MIT]: http://www.lattix.com/node/62
[oopsla05.pdf]: http://www.lattix.com/files/dl/wp/oopsla05.pdf

[DSM par NDepend]: http://www.ndepend.com/doc_matrix.aspx 

[Oracle 11g Express Edition]: http://www.oracle.com/technetwork/products/express-edition/downloads/index.html

[SQL Developer]:http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html
[SQL Developer Documentation]: http://docs.oracle.com/cd/E35137_01/index.htm


[Sonar]: http://www.sonarsource.org/
[Dtangler]: http://web.sysart.fi/dtangler/

<!-- Ressources images -->

[simple_class_diagramm]: /assets/images/dsm_classdiagram_simple.png
[complicated_class_diagramm]: /assets/images/dsm_classdiagram_complicated.jpg
[dsm_sample]: /assets/images/dsm_sample.gif
[dsm_log4j]: /assets/images/dsm_log4j_01.png
[dsm_lattix_01]: /assets/images/dsm_lattix_layer_01.jpg
[dsm_lattix_02]: /assets/images/dsm_lattix_layer_02.jpg
[dsm_lattix_commons-collections_dtangler]: /assets/images/dsm_dtangler_commons-collections.png
[dsm_lattix_commons-collections_sonar]: /assets/images/dsm_sonar_commons-collections.png
[dsm_lattix_springframework_dtangler]: /assets/images/dsm_dtangler_springframework.png

