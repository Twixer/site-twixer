---
layout: post
title: "DSM - Design Structure Matrix"
description: "Un petit tour de Lattix et des DSM"
category: tuto
tags: [tuto, lattix, oracle, java, spring, hibernate, dsm]
published: true
---
{% include JB/setup %}

#Introduction
Dans le cadre de mon travail, j'ai été amené à étudié l'outil [Lattix][]. Cet outil permet de créer des matrices de structures de dépendances (*Design|Dependency Structure Matrix* ou *DSM*) et de les manipuler sur des projets Java, .NET, Oracle ... L'objectif final est la modélisation de l'architecture d'un projet, la définition de règles pour le maintien de la cohérence de cette architecture.
L'analyse demandée portait sur la capacité de l'outil à modéliser un gros projet sous Oracle utilisant beaucoup de schémas et de procédures stockées.  
Cet article va décrire mon retour d'expérience sur Lattix et les DSM.

---------------------------------------
#Démarche
J'avais deux axes à suivre pour mener à bien mon travail : 
1. monter en compétence sur la notion de DSM
2. monter en compétence sur Lattix 

Pour démarrer, j'avais sous la main un projet exemple utilsant une base de donnée Oracle 11g ainsi que la couche Java accédant à cette base au moyen du doublé gagnant Spring/Hibernate. Les fichiers à disposition étaient les suivants : 
- script de création des tablespaces
- script de création des users et des droits 
- dump d'une base de développement
- sources Java de la couche persistence avec Spring/Hibernate

L'idée est donc de me documenter sur les DSM et d'utiliser le projet pour manipuler Lattix et voir ce qu'il y avait sous le capot.

#DSM
--------------------------------------- 

##Définition
Le sigle DSM est utilisé pour **D**esign ou **D**ependency **S**tructure **M**attrix. Pour traduire mot à mot l'anglais, je dirais que c'est une matrice de la structure de dépendences d'un système. La DSM est une représentation sous forme de tableau (matrice) des liens entre les composants d'un système. Le champ d'application des DSM n'est pas uniquement l'informatique bien que ce soit ce domaine qui nous intéresse ici (cf. [DSM-conference][]).

##La cible
Dans le domaine de l'IT, l'approche DSM couvre les besoins de conception, de maintenance ou d'analyse d'une architecture. Si nous devons modélisé l'architecture d'un système, nous aurons comme premier réflexe l'utilsiation d'UML et du diagramme de classe pour des langages objets ou alors nous passerons par un MCD/MPD si nous avons affaire à une base de données. Cette conception nous conduit à des schémas comme : ![cas simple de modélisation][simple_class_diagramm]
ou comme : ![cas de modélisation compliquée][complicated_class_diagramm]  
Il n'est pas tout de suite évident d'appréhender tous les liens et les dépendences entre les éléments. L'oeil humain est capable de noter la plus ou moins grande complexité du système mais identifier rapidement quel élément est en lien avec lequel ou lister rapidement tous les liens est bien plus compliqué.  
C'est là qu'intervienne des représentations comme la DSM. Celle-ci est capable de synthétiser en un tableau les liens entre chaque élément d'un système et de percevoir la structure du système : ![exemple de DSM][dsm_sample]  

##Comment lire une DSM ?
Une DSM contient les mêmes éléments en abscisses et en ordonnées et ce dans le même ordre. Certaines DSM ne représentent pas le nom de l'élément en colonne mais en général un clic sur un élément du tableau met en évidence l'élément en colonne.  
En parcourant la colonne de l'élément **A**, on note sur chaque ligne les liens de cet élément avec les autres éléments si une croix ou un numérique est présent (cette représentation dépend encore des produits). Donc dans notre exemple, **A** possède un lien vers **C** et **G**.

##Comment identifier les points d



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
##Oracle
La version de la base de données sera Oracle 11g Express Edition. Elle peut être récupérée sur le site de l'éditeur : [Oracle 11g Express Edition][].

---------------------------------------
#Conclusion
L'analyse du produit Lattix m'a permis d'aborder et de comprendre l'approche DSM. J'avais déjà été confronté à cette représentation sans y prêter attention avec par exemple [Sonar][] dans le monde Java. L'analyse de l'architecture d'un projet sous Sonar sera maintenant d'autant plus simple et rapide.  

---------------------------------------
#Ressources

<!-- Ressources URL -->
[Lattix]: http://www.lattix.com/

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
[dtangler]: http://web.sysart.fi/dtangler/

<!-- Ressources images -->

[simple_class_diagramm]: /assets/images/dsm_classdiagram_simple.png
[complicated_class_diagramm]: /assets/images/dsm_classdiagram_complicated.jpg
[dsm_sample]: /assets/images/dsm_sample.gif
[dsm_log4j]: /assets/images/dsm_log4j_01.png


