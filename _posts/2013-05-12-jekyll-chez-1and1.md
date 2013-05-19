---
layout: post
title: "Jekyll chez 1and1"
description: "Intégration d'un site Jekyll chez 1and1"
category: tuto
tags: [tuto, jekyll, 1and1, git]
---
{% include JB/setup %}

#Introduction
J'utilise les services de l'hébergeur [1and1][] depuis très longtemps, décembre 2005 pour être précis. J'avais souscrit à l'époque en raison d'une offre très agressive et donc très intéressante. Depuis ce temps, je profite donc de cet hébergeur pour mes différents besoins (forums, sites webs ...). J'y possède un serveur mutualisé avec du stockage, des bases de données MySQL, divers noms de domaine.
Cet article va décrire l'intégration de Jekyll chez l'hébergeur [1and1][].  

---------------------------------------
#Domaine
Je possède le nom de domaine twixer.fr et j'ai choisi d'utiliser le sous-domaine dev.twixer.fr pour mon site de blogs. La configuration se fait simplement dans le menu *Domaines & Espaces Web* :
![domaines 1and1][1and1_domaines]
Je fais pointer mon sous-domaine *dev.twixer.fr* vers le répertoire : */www/site-twixer/_site*.

---------------------------------------
#Connexion au serveur 1and1
J'ai un serveur mutualisé sur lequel je peux me connecter via FTP, SSH. Les informations de connexion sont disponibles dans l'espace client de 1and1 : 
![accès 1and1][1and1_access]
Si vous ne les posséder pas déjà, il faut récupérer les identifiants de connexion SSH pour votre domaine (login et mot de passe) dans cette interface. Attention, il y a un léger temps d'attente pour l'activation du mot de passe. Une fois le login et mot de passe en main,  on va pouvoir se connecter via SSH à notre serveur. Personnellement étant sous Windows, j'utilise le client [Putty][]. 

---------------------------------------
#Jekyll et 1and1
Sur mon serveur 1and1, j'ai accès à ruby et à Git. J'ai donc fait le choix de générer mon site sur mon serveur 1and1.  
Version de ruby : 
> ruby 1.8.7 (2010-08-16 patchlevel 302) \[i486-linux\]
Version de git : 
> git version 1.7.2.5

##Installation de Jekyll
Pour installer des gems Ruby chez 1and1, il faut changer le répertoire par défaut de destination des nouvelles gems en raison de problèmes d'écriture dans les répertoires. Il faut donc modifier le fichier *.bash_profile* en y ajoutant les lignes suivantes :  
{% highlight console %}
export PREFIX=~/ruby/gem
export GEM_HOME=$PREFIX/lib/ruby/gems/1.8
export RUBYLIB=$PREFIX/lib/ruby:$PREFIX/lib/site_ruby/1.8
export PATH=$PATH:$PREFIX/lib/ruby/gems/1.8/bin
{% endhighlight %}
N'oublier pas de recharger ce fichier avec la commande *source .bash_profile*.
On va donc déposer les nouvelles gems dans un répertoire *./ruby/gem/lib/ruby/gems* à la racine de notre arborescence.  

Ensuite il faut installer Jekyll de la façon classique : 
{% highlight console %}
gem install jekyll
gem install rdiscount
{% endhighlight %}

Mon dépôt de gems contient (*gem list*) : 
{% highlight console %}
*** LOCAL GEMS ***

chunky_png (1.2.8)
classifier (1.3.3)
compass (0.12.2)
directory_watcher (1.5.1)
fast-stemmer (1.0.2)
fssm (0.2.10)
jekyll (0.12.1)
kramdown (0.14.2)
liquid (2.5.0)
maruku (0.6.1)
posix-spawn (0.3.6)
pygments.rb (0.3.7)
rake (10.0.4)
rdiscount (2.0.7.2)
sass (3.2.8)
syntax (1.0.0)
yajl-ruby (1.1.0)
{% endhighlight %}

Jekyll est donc maintenant installé sur notre serveur 1and1 et nous pouvons générer des sites directement depuis le serveur.

##Git et le repository GitHub
Pour récupérer les fichiers de mon site de blogging, rien de plus simple. J'ai créé un répertoire *./www/site-twixer* à la racine de mon *home* afin d'accueillir mon site de blogs. Je me place dans le répertoire *./www* et je clone mon repository GitHub :
{% highlight console %}
git clone git@github.com:Twixer/site-twixer.git
{% endhighlight %}
Inutile de spéficier un répertoire de destination parce que mon repository se nomme *site-twixer* et ira bien dans le répertoire attendu.   
A partir de là on peut lancer la commande jekyll depuis *./www/site-twixer* pour construire notre site qui sera généré dans le répertoire *./www/site-twixer/_site.* (cf. la configuration de destination pour le sous-domaine !).  

Pour les mises à jour ultérieures du site, il suffit d'utiliser la commande git pull dans le répertoire *site-twixer* puis relancer la génération du site avec la commande *jekyll*.

##Paramétrages supplémentaires
###Erreur 404
Par défaut, les erreurs HTTP 404 de 1and1 sont renvoyées vers une page standard de 1and1. Nous voulons que l'erreur 404 pointe vers la page *404.html* de notre site. Pour ce faire, il faut créer un fichier *.htaccess* dans le répertoire *./www/site-twixer* avec la ligne suivante : 
{% highlight console %}
ErrorDocument 404 /404.html
{% endhighlight %}
On pourrait donc rajouter d'autres pages d'erreur à notre site en n'oubliant pas à chaque fois d'ajouter la ligne correspondante dans ce fichier (400, 500, ...).

---------------------------------------
#Conclusion
Nous pouvons donc maintenant générer directement notre site Jekyll sur notre serveur 1and1. Ce fonctionnement nous permet donc de modifier le site depuis n'importe quel endroit tant qu'on le pousse sur le repository GitHub.   
Le cycle de vie n'est pas idéal pour l'instant car la modification d'une page sur un poste en local avec un push sur GitHub nous oblige à effectuer des étapes manuelles pour mettre à jour notre site : 
1. connexion via SSH au serveur 1and1
2. *git pull* pour récupérer les dernières modifications
3. *jekyll* pour générer le sites  

Même si elles restent simples, ces opérations devraient être automatisées. Affaire à suivre ...

---------------------------------------
#Ressources
[1and1][]  
[Putty][]

[1and1]: https://www.1and1.fr/
[Putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[1and1_access]: /assets/images/1and1_access.png
[1and1_domaines]: /assets/images/1and1_domaines.png