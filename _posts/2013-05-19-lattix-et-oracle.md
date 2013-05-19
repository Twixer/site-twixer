---
layout: post
title: "Lattix et Oracle"
description: ""
category: oracle
tags: [tuto, lattix, oracle, java, hibernate]
---
{% include JB/setup %}

#Introduction
Dans le cadre de mon travail, on est venue me demander mon avis sur l'outil [Lattix][]. Cet outil permet de créer des matrices de structures de dépendances (*Dependency Structure Matrix* ou *DSM*) et de les manipuler sur des projets Java, .NET, Oracle ... L'objectif final est la modélisation de l'architecture d'un projet, la définition de règles pour le maintien de la cohérence de cette architecture.
L'analyse demandée portait sur la capacité de l'outil à modéliser un gros projet sous Oracle utilisant beaucoup de schémas et de procédures stockées.  
N'ayant aucune connaissance de [Lattix][] et des DSM, ma démarche a été de tester en premier lieu l'outil sur un petit projet Oracle. Cet article va donc illustrer les étapes qui démarrent avec l'installation de la base Oracle jusqu'à la création d'un projet sous Lattix et son exploitation.

---------------------------------------
#Contexte
On m'a fourni le dump d'un projet pour une base de données Oracle 11g ainsi que les sources Java du projet qui accède à cette base au moyen du framework Hibernate.

---------------------------------------
#Installation

##Oracle
La version de la base de données sera Oracle 11g Express Edition. Elle peut être récupérée sur le site de l'éditeur : [Oracle 11g Express Edition][].

---------------------------------------
#Ressources

[Lattix]: http://www.lattix.com/

[Oracle 11g Express Edition]: http://www.oracle.com/technetwork/products/express-edition/downloads/index.html