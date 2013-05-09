---
layout: post
title: "Jekyll sur Windows"
description: ""
category: jekyll
tags: [tuto jekyll ruby]
---
{% include JB/setup %}

#Introduction
Après avoir fait le choix de Jekyll pour **propulser** ce site, il fallait mettre en place un environnement de développement sur mon poste local.
Je travaille sur un poste sous Windows (eh oui, pas de Linux par défaut bien que j'utilise régulièrement des VM).
Autant le dire tout de suite, j'ai lutté pour finaliser un environnement stable sur mon poste.
Entre l'installation de la bonne version de Ruby, des gems qui vont bien, du choix d'un template Jekyll, la route fut pavée de difficultés et problèmes en tout genre.
Cet article présente les différentes étapes à suivre pour disposer d'un environnement de développement pour générer un site de blogging avec Jekyll.

#Ruby
Jekyll a besoin de ruby pour fonctionner. Pour l'installation j'ai suivi le tutoriel 
{% highlight javascript %} 
public static main(String[] args) {
	System.out.println("Hello World");
}
{% endhighlight %}

#Sources
[Installation de Jekyll sous Windows](http://forresst.github.io/2012/03/20/Installer-Jekyll-Sous-Windows)