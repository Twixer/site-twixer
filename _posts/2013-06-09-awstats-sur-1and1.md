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
Les pré-requis sont donc de disposer d'un serveur (Linux ou Windows) avec Perl activé et d'un serveur Web.  

---------------------------------------
#Pourquoi Awstats
Beaucoup des services rendus par Awstats sont disponibles dans Google Analytics ou dans le panneau d'administration de mon serveur sur 1and1 (cf. [1and1 Web Statistics][]). Alors pourquoi ce choix ?  
Awstats est un outil Open Source qui permet d'avoir la maîtrise de ce qu'on fait et donc de ce qu'on veut. Il offre comme principal avantage de ne pas s'appuyer sur un outil tiers dont on n'a pas la maîtrise.  
Dans le contexte d'une entreprise, en particulier pour ses applicaitons intranet, il n'est souvent pas possible d'utiliser des outils comme Google Analytics. Il est donc judidiceux d'avoir sous la main d'autres outils pour les compenser.
J'ai déjà eu l'occasion d'installer et d'utiliser Awstats pour avoir une vision de l'usage d'une application par ses utilisateurs. Ce type d'outil permet d'avoir rapidement une vision des fonctionnalités de l'application qui sont les plus utilisées et donc les plus sensibles.  
Awstats se focalise uniquement sur l'anlyse statistiques des données par opposition à l'analyse analytique. Dans le contexte de l'Open Source il existe des outils pour couvrir ce besoin comme par exemple [Piwik][].


---------------------------------------
#Pré-requis 1and1
##Les logs sur 1and1
Sur un serveur Linux mutualisé 1and1, les logs se trouve dans le répertoire **~/logs**.  
Ces logs ont une durée de vie d'envrion 3 mois. Si on veut pouvoir faire des analyses sur une durée plus longue, il est impératif de transférer les fichiers dans un autre répertoire via un cron par exemple.  
Pour la semaine courante qui va du lundi au dimanche, un fichier est produit par jour puis compressé au format *gz*. Au bout de la troisième semaine, les fichiers sont rassemblés en un unique fichier avec l'identifiant de la semaine :
{% highlight bash %}
-rw-r--r-- 1 root      root      5606 Apr 14 22:40 access.log.15.gz
-rw-r--r-- 1 root      root      4651 Apr 21 23:13 access.log.16.gz
-rw-r--r-- 1 root      root      6581 Apr 28 22:42 access.log.17.gz
-rw-r--r-- 1 root      root      6163 May  5 22:41 access.log.18.gz
-rw-r--r-- 1 root      root     11812 May 12 23:04 access.log.19.gz
-rw-r--r-- 1 root      root      5748 May 19 23:09 access.log.20.gz
-rw-r--r-- 1 root      root     15358 May 26 23:49 access.log.21.gz
-rw-r--r-- 1 root      root      9302 May 27 22:57 access.log.22.1.gz
-rw-r--r-- 1 root      root      1489 May 28 22:46 access.log.22.2.gz
-rw-r--r-- 1 root      root      2193 May 29 22:19 access.log.22.3.gz
-rw-r--r-- 1 root      root      2399 May 30 21:50 access.log.22.4.gz
-rw-r--r-- 1 root      root      1845 May 31 23:59 access.log.22.5.gz
-rw-r--r-- 1 root      root      1400 Jun  1 23:17 access.log.22.6.gz
-rw-r--r-- 1 root      root      3015 Jun  2 23:56 access.log.22.7.gz
-rw-r--r-- 1 root      root      5043 Jun  3 23:57 access.log.23.1.gz
-rw-r--r-- 1 root      root      3264 Jun  4 23:30 access.log.23.2.gz
-rw-r--r-- 1 root      root      2784 Jun  5 23:34 access.log.23.3.gz
-rw-r--r-- 1 root      root      2641 Jun  6 23:20 access.log.23.4.gz
-rw-r--r-- 1 root      root      1702 Jun  7 20:47 access.log.23.5.gz
-rw-r--r-- 1 root      root      1938 Jun  8 21:12 access.log.23.6.gz
-rw-r--r-- 1 root      root     11056 Jun  9 18:32 access.log.23.7
{% endhighlight %}
Les logs répondent au format suivant (cf. [format de logs 1and1][]):  
{% highlight apache %}
LogFormat "%h %l %u %t \"%r\" %s %b %v \"%{Referer}i\" \"%{User-agent}i\" \"%{X-Forwarded-For}i\""
{% endhighlight %}

##Sous-domaine awstats
Il est préférable de créer un nouveau sous-domaine pour les awstats : par exemple **awstats.monDomaine.fr**.  
Dans la console d'administration de 1and1, créez un nouveau sous-domaine pour votre site et modifier la destination pour pointer par exemple vers le répertoire **~/www/awstats/wwwroot**.  

---------------------------------------
#Installation et Configuration

La documentation présentée en français ici s'inspire fortement de ce [tutoriel en anglais][].  

##Dernière version d'Awstats
Récupérer la dernière version d'Awstats sur [download Awstats][] et décompressez-la en local.  
Transférer le contenu via FTP dans le répertoire **~/www/awstats/**.

#Configuration d'Awstats
Connectez-vous via SSH à votre serveur 1and1 et copier le fichier **wwwroot/cgi-bin/awstats.model.conf** vers **wwwroot/cgi-bin/awstats.monDomaine.fr.conf**.

##LogFile
Editer et remplacer le paramètre LogFile par :
{% highlight apache %}
LogFile="gunzip -c $(ls -t ~/logs/access*.gz | head -1) |"
{% endhighlight %}
On va prendre le dernier fichier de log zippé dans le répertoire de nos logs Apache.

##LogFormat
Modifier le paramètre LogFormat : 
{% highlight apache %}
LogFormat=%host %other %logname %time1 %methodurl %code %bytesd %virtualname %refererquot %uaquot %otherquot
{% endhighlight %}

##SiteDomain
Mofidier le paramètre SiteDomain avec votre nom de domaine ou de sous-domaine : 
{% highlight apache %}
SiteDomain="monDomaine.fr"
{% endhighlight %}

##HostAliases
Le paramètre HostAliases peut contenir d'autres domaines ou sous-domaines par lesquels votre site est accessible, par exemple : 
{% highlight apache %}
HostAliases="www.mondomaine.fr site.mondomaine.fr www.site.mondomaine.fr"
{% endhighlight %}

##DirCgi
Le paramètre DirCGI doit être modifié avec le sous-domaine que nous avons créé pour Awstats : 
{% highlight apache %}
DirCgi="http://awstats.monDomaine.fr/cgi-bin"
{% endhighlight %}

##DirData
Le paramètre DirData, répertoire de destination des analyses statistiques doit être modifié : 
{% highlight apache %}
DirData="../data"
{% endhighlight %}

##Configuration du cron
Pour q'Awstats puisse analyser chaque jour un nouveau fichier de log, il faut configurer la bonne commande dans le cron.
Si on veut lancer l'analyse d'un nouveau fichier tous les jours à 2h10 du matin, on doit ajouter la ligne suivante à notre cron : 
{% highlight bash %}
10 2 * * * perl ~/www/awstats/wwwroot/cgi-bin/awstats.pl -config=monDomaine.fr -update
{% endhighlight %}

##Sécuration du répertoire Awstats
La façon la plus rapide pour sécuriser l'accès à notre IHM Awstats est de copier le fichier .htaccess du répertoire **~/logs** dans **~/www/awstats/** et de le sécuriser avec la commande : 
{% highlight bash %}
chmod 404 ~/www/awstats/.htaccess
{% endhighlight %}

##Connection
La connexion à l'IHM d'Awstats se fait ensuite par l'URL : http://awstats.monDomaine.fr/cgi-bin/awstats.pl&config=monDomaine.fr.  

---------------------------------------
#Divers
##Rattrapage de journées
Il arrive qu'on ait besoin de rattraper une journée. Pour ce faire la solution simple est d'utiliser le script utilitaire *logresolvemerge.pl* d'Awstats qu'on trouve dans le répertoire **tools** de la distribution.
Les commandes sont : 
{% highlight bash %}
perl awstats/tools/logresolvemerge.pl ~/logs/access.log.*.gz > temp.log  
perl awstats/cgi-bin/awstats.pl -config=monDomaine.fr -LogFile=./temp.log -update  
rm temp.log  
{% endhighlight %}

---------------------------------------
#Conclusion
Il est assez simple d'installer Awstats sur son serveur 1and1 tout comme sur n'importe quel autre serveur.  
L'outil permet ensuite d'avoir une vision suffisante des connexions à son site : combien, quand, qui, quel navigateur ..

---------------------------------------
#Ressources

- [Awstats][]
- [Awstats Features][]
- [Piwik][]
- [1and1 Web Statistics][]
- [format de logs 1and1][]
- [download Awstats][]
- [tutoriel en anglais][]  

<!-- Ressources URL -->
[Awstats]: http://awstats.sourceforge.net/
[Awstats Features]: http://www.awstats.org/#FEATURES
[Piwik]: http://piwik.org/
[1and1 Web Statistics]: http://help.1and1.com/marketing-c85117/1und1-siteanalytics-c37780/how-do-i-access-1und1-siteanalytics-a597243.html
[format de logs 1and1]: http://help.1and1.com/marketing-c85117/1und1-siteanalytics-c37780/what-is-the-log-file-format-for-linux-shared-hosting-a598312.html
[download Awstats]: http://sourceforge.net/projects/awstats/files/
[tutoriel en anglais]: http://1024k.de/awstats/1and1/tutorial.html

<!-- Ressources images -->

[awstats_historique_mensuel]: /assets/images/awstats_historique_mensuel.png
[awstats_visites_pays]: /assets/images/awstats_visites_pays.png
[awstats_visites_robots]: /assets/images/awstats_visites_robots.png

