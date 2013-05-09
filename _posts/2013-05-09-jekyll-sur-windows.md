---
layout: post
title: "Jekyll sur Windows"
description: ""
category: jekyll
tags: [tuto, jekyll, ruby, git, cygwin]
---
{% include JB/setup %}

#Introduction
Après avoir fait le choix de Jekyll pour **propulser** ce site, il fallait mettre en place un environnement de développement sur mon poste local.
Je travaille sur un poste sous Windows (eh oui, pas de Linux par défaut bien que j'utilise régulièrement des VM).
Autant le dire tout de suite, j'ai lutté pour finaliser un environnement stable sur mon poste.
Entre l'installation de la bonne version de Ruby, des gems qui vont bien, du choix d'un template Jekyll, la route fut pavée de difficultés et problèmes en tout genre.
Cet article présente les différentes étapes à suivre pour disposer d'un environnement de développement pour générer un site de blogging avec Jekyll.

#Environnement technique
J'utilise un poste sous Windows 7 Professionnel SP1 (sytème 64 bits). 

#Ruby
Jekyll a besoin de ruby pour fonctionner. Pour l'installation j'ai suivi le tutoriel [Installation de Jekyll sous Windows](http://forresst.github.io/2012/03/20/Installer-Jekyll-Sous-Windows). Ce tutoriel est à suivre à la lettre et seule la dernière astuce n'a pas été nécessaire dans mon cas.
Les versions de ruby et du devKit que j'ai utilisé sont à récupérer sur [RubyInstaller](http://rubyinstaller.org/downloads/):
- [rubyinstaller-1.9.3-p392.exe] (http://rubyforge.org/frs/download.php/76798/rubyinstaller-1.9.3-p392.exe)
- [DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe](https://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe)

J'ai tenté une installation avec les dernières version de Rubis et du devKit mais j'ai ensuite eu des problèmes avec Jekyll et ses dépendances.

#


#Sources
[Installation de Jekyll sous Windows](http://forresst.github.io/2012/03/20/Installer-Jekyll-Sous-Windows)