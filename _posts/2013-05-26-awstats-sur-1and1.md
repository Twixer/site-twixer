---
layout: post
title: "Awstats sur 1and1"
description: "Installation d'Awstats sur un serveur 1and1"
category: tuto
tags: [tuto, 1and1, awstats, linux]
published: true
---
{% include JB/setup %}

#Introduction
Cette article va présenter la mise en place d'[Awstats][] sur mon serveur 1and1.  

---------------------------------------
#Awstats en deux mots
## A quoi ça sert ?
[Awstats][] est un outil d'analyse de logs de connexion afin de produire des données statistiques. Les principaux serveurs Web (Apache, IIS, ...) sont pris en compte et les capacités de configuraiton des logs permettent à l'outil de pouvoir analyser pratiquement tous les formats de logs.  
Awstats comporte plusieurs fonctionnalités qui en synthèse sont (cf. [Awstats Features][]) : 
* Nombre de visites / visiteurs uniques
* Longueur de la visite et dernière visite
* Authentification de l'utilisateur
* Représentation par jour de la semaine et par heure (pages, hits, taille pour chaque heure et jour de la semaine)
* Domaines/pays des hôtes des visiteurs (pages, hits, taille, 269 domaines/pays détectés, détéction par IP géographique),
* Liste des hôtes
* Top vues, pages d'entrée et de sortie
* Type de fichier
* Statistiques sur la compression Web (mod_gzip ou mod_deflate)
* Système d'exploitation utilisé (pages, hits, taille par OS, 35 OS détectés),
* Navigateur utilisé (pages, hits, taille, par version
* Visites des robots (319 robots détectés),
* Attaques de vers (5 famille de ver),
* Moteur de recherche, phrase et mot clé utilisés pour trouver le site (google, yahoo, altavista, etc...)
* erreurs HTTP
* ...  

Exemple de nombre de visites sur les derniers mois : 
![][awstats_historique_mensuel]  
Exemple de nombre de visites par pays :  
![][awstats_visites_pays]  
Exemple de nombre de visite des robots : 
![][awstats_visites_robots]  
Le projet est toujours maintenu et la dernière version est sortie en mars 2013. 

##Comment ça marche ?
Awstats est un programme CGI composé de scripts Perl. Les scripts permettent d'anlyser les fichiers de logs et de produire les IHM pour les graphiques.  
Les pré-requis sont donc de disposer d'un serveur avec Perl activé et d'un serveur Web.  

---------------------------------------
#Pourquoi Awstats
Beaucoup des services rendus par Awstats sont disponibles dans Google Analytics ou dans le panneau d'administration de mon serveur sur 1and1 (cf. [1and1 Web Statistics][]). Alors pourquoi ce choix ?  
Awstats est un outil Open Source qui permet d'avoir la maîtrise de ce qu'on fait et donc de ce qu'on veut. Il offre comme principal avantage de ne pas s'appuyer sur un outil tiers dont on n'a pas la maîtrise.  
Dans le contexte d'une entreprise, en particulier pour ses applicaitons intranet, il n'est souvent pas possible d'utiliser des outils comme Google Analytics. Il est donc judidiceux d'avoir sous la main d'autres outils pour les compenser.
J'ai déjà eu l'occasion d'installer et d'utiliser Awstats pour avoir une vision de l'usage d'une application par ses utilisateurs. Ce type d'outil permet d'avoir rapidement une vision des fonctionnalités de l'application qui sont les plus utilisées et donc les plus sensibles.  
Awstats se focalise uniquement sur l'anlyse statistiques des données par opposition à l'analyse analytique. Dans le contexte de l'Open Source il existe des outils pour couvrir ce besoin comme par exemple [Piwik][].


---------------------------------------
#Les logs sur 1and1
Sur un serveur Linux mutualisé 1and1, les logs se trouve dans le répertoire *~/logs*. Ces logs répondent au format suivant : 

---------------------------------------
#Installation

---------------------------------------
#Conclusion


---------------------------------------
#Ressources

<!-- Ressources URL -->
[Awstats]: http://awstats.sourceforge.net/
[Awstats Features]: http://www.awstats.org/#FEATURES
[Piwik]: http://piwik.org/
[1and1 Web Statistics]: http://help.1and1.com/marketing-c85117/1und1-siteanalytics-c37780/how-do-i-access-1und1-siteanalytics-a597243.html

<!-- Ressources images -->

[awstats_historique_mensuel]: /assets/images/awstats_historique_mensuel.png
[awstats_visites_pays]: /assets/images/awstats_visites_pays.png
[awstats_visites_robots]: /assets/images/awstats_visites_robots.png

