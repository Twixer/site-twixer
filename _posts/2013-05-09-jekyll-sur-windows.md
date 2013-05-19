---
layout: post
title: "Jekyll sur Windows"
description: ""
category: tuto
tags: [tuto, jekyll, ruby, git, windows]
---
{% include JB/setup %}

#Introduction
Après avoir fait le choix de Jekyll pour *propulser* ce site, il fallait mettre en place un environnement de développement sur mon poste local.
Je travaille sur un poste sous Windows.
Autant le dire tout de suite, j'ai lutté pour finaliser un environnement stable.  
Entre l'installation de la bonne version de Ruby, des gems nécessaires, du choix d'un template Jekyll, la route fut pavée de difficultés et problèmes en tout genre.  
Cet article présente les différentes étapes à suivre pour disposer d'un environnement de développement pour générer un site de blogging avec Jekyll.

---------------------------------------
#Environnement technique
J'utilise un poste sous Windows 7 Professionnel SP1 (sytème 64 bits). 

---------------------------------------
#Ruby
Jekyll a besoin de ruby pour fonctionner. Pour l'installation j'ai suivi le tutoriel [Installation de Jekyll sous Windows][]. Ce tutoriel est à suivre à la lettre et seule la dernière astuce n'a pas été nécessaire dans mon cas.
Les versions de ruby et du devKit que j'ai utilisé sont à récupérer sur [RubyInstaller][]:
- [rubyinstaller-1.9.3-p392.exe] (http://rubyforge.org/frs/download.php/76798/rubyinstaller-1.9.3-p392.exe)
- [DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe](https://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe)

Avec ces deux versions et le suivi du tutoriel d'installation, vous aurez un environnement fonctionnel pour Jekyll sous Windows.

Voici mon dépôt de gems avec leur version (commande *gem list*) :   
	
	** LOCAL GEMS **
	anemone (0.7.2)
	bigdecimal (1.1.0)
	chunky_png (1.2.8)
	classifier (1.3.3)
	colorator (0.1)
	commander (4.1.3)
	commonjs (0.2.6)
	compass (0.12.2)
	directory_watcher (1.5.1, 1.4.1)
	fast-stemmer (1.0.2)
	fssm (0.2.10)
	highline (1.6.18)
	io-console (0.3)
	jekyll (0.12.1)
	json (1.5.5)
	kramdown (0.14.2)
	less (2.3.2)
	liquid (2.5.0)
	maruku (0.6.1)
	minitest (2.5.1)
	nokogiri (1.5.9 x86-mingw32)
	posix-spawn (0.3.6)
	pygments.rb (0.4.2, 0.3.7)
	rake (0.9.2.2)
	rdiscount (2.0.7.2)
	rdoc (3.9.5)
	ref (1.0.4)
	robotex (1.0.0)
	rvm (1.11.3.7)
	safe_yaml (0.7.1)
	sass (3.2.8)
	syntax (1.0.0)
	yajl-ruby (1.1.0 x86-mingw32)

##Les pièges à éviter
J'ai tenté une installation avec les dernières versions de Ruby et du devKit, respectivement les versions 2.0.0 et 4.7.2, mais j'ai eu des problèmes avec les versions 64 bits.  
J'ai également tenté d'installer une version 32 bits de ces versions mais c'est avec les dépendances de Jekyll que j'ai rencontré des problèmes. Jekyll n'est pas compatible avec la dernière version des gems Ruby 2, donc je suis resté sur les versions 1.9.3.

A chaque ouverture d'une fenêtre de commande Windows, il faudra taper les commandes suivantes pour la prise en compte de l'encoding en UTF-8 : 

{% highlight console %}
	set LC_ALL=en_US.UTF-8
	set LANG=en_US.UTF-8
{% endhighlight %}
Dans le cas contraire, des erreurs pourraient apparaître lors de la création du site avec la commande *jekyll*.

---------------------------------------
#Git
Le versionning est assuré au moyen de Git. Il faut donc installer [Git pour Windows][].
Dans mon cas j'utilise la version [1.7.9](http://code.google.com/p/msysgit/downloads/detail?name=Git-1.7.9-preview20120201.exe&can=2&q=).

Jusqu'à maintenant je n'ai eu l'occasion d'utiliser que [SVN](http://subversion.tigris.org/) comme gestionnaire de sources. La prise en main de Jekyll avec un versionning sous Github m'a permis de me familiariser avec Git.  
Je maîtrise les grands concepts de versionning (ajout, commit, commentaires, branches, tags ...) mais pour me familiariser avec Git j'ai suivi le [tutoriel Git Immersion][] assez sympa qui permet d'y aller pas à pas. Je conseille à chaque débutant souhaitant comprendre les concepts et la syntaxe des commandes de le suivre au moins une fois.

J'utilise actuellement deux sources de documentation principales :
- le [manuel Git][]
- le [livre Pro Git en ligne][]

---------------------------------------
#Jekyll
Pour démarrer avec [Jekyll][], on peut partir de zéro comme le préconise [Carl Boettiger](http://carlboettiger.info/2012/12/30/learning-jekyll.html). L'apprentissage de zéro permet de n'utiliser que les fonctionnalités dont on a besoin et de maîtriser au fil de l'eau l'outil et les concepts.  
Pour ma part, j'ai décidé de partir sur un template existant.  Je voulais disposer rapidement d'un site avec une structure, une mise en page et des plugins.

##Jekyll-Bootstrap

J'ai choisi de partir sur [Jekyll-Bootstrap][] (JB) pour une première utilisation de Jekyll.
JB contient de base les éléments suivants : 
- [fonctionnalités](http://jekyllbootstrap.com/usage/blog-configuration.html) : commentaires, analytics, pages/tags, partage social ...
- mise en place simple et rapide de [thèmes](http://jekyllbootstrap.com/usage/jekyll-theming.html), changement entre thèmes

Pour démarrer avec JB, rien de plus simple. En ligne de commande, taper la commande suivante : 
{% highlight console %}
git clone https://github.com/plusjade/jekyll-bootstrap.git DEST_PATH
{% endhighlight %}

Les sources pour démarrer se trouvent donc maintenant dans DEST_PATH.  
Pour ma part j'utilise GitHub pour héberger mon repository distant de sources. J'y ai créé le repository *site-twixer*. Pour pouvoir pousser mes fichiers sur mon repository, j'ai suivi les étapes suivantes : 
- supprimer le répertoire .git dans DEST_PATH/
- en ligne de commande : 
{% highlight console %}
	git clone git@github.com:Twixer/site-twixer.git DEST_PATH
{% endhighlight %}
- en ligne de commande et dans le répertoire DEST_PATH :
{% highlight console %}
	git add .  
	git commit -m "add the template Jekyll-Bootstrap"  
	git push  
{% endhighlight %}

Vous pouvez maintenant ajouter des articles dans le répertoire DEST_PATH/_post.
On peut utiliser une commande du rakefile de JB : 
{% highlight console %}
rake post title="Hello World"
{% endhighlight %}
Un fichier avec le pattern *AAAA-MM-JJ-Hello-World.md* sera créé dans le répertoire DEST_PATH/_post avec un en-tête YAML déjà initialisé.

---------------------------------------
#Conclusion

Après avoir installé Ruby et la gem Jekyll, Git et le projet Jekyll-Bootstrap, on est prêt à écrire les articles pour son site de blogging.
La facilité de création d'un thème pour JB m'a conduit à créer un thème [twixer](https://github.com/Twixer/theme-jb-twixer) que je compte modifier au fur et à mesure.
Le déploiement du site chez mon hébergeur sera abordé dans l'article [Jekyll chez 1and1]({% post_url 2013-05-12-jekyll-chez-1and1 %}).

---------------------------------------
#Ressources

- Ruby  
	site [RubyInstaller][] pour Windows  
	[Installation de Jekyll sous Windows][]

- Git  
	[Git pour Windows][]  
	[tutoriel Git Immersion][]  
	[manuel Git][]  
	[livre Pro Git en ligne][]  

- Jekyll  
	site officiel de [Jekyll][]  
	site officiel de [Jekyll-Bootstrap][]  

[RubyInstaller]: http://rubyinstaller.org/downloads/ "Site RubyInstaller pour Windows"
[Git pour Windows]: http://code.google.com/p/msysgit/
[tutoriel Git Immersion]: http://gitimmersion.com/
[manuel Git]: http://gitmanual.org/git.html
[livre Pro Git en ligne]: http://git-scm.com/book
[Installation de Jekyll sous Windows]: http://forresst.github.io/2012/03/20/Installer-Jekyll-Sous-Windows
[Jekyll]: http://jekyllrb.com/
[Jekyll-Bootstrap]: http://jekyllbootstrap.com/	
