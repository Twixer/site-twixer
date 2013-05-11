---
layout: post
title: "Le cinquième commandement"
description: "Pourquoi Jekyll ?"
category: jekyll 
tags: [jekyll]
---
{% include JB/setup %}

#Blog static vs blog dynamic
Pour ce blog, mon besoin était le suivant : 
1. une mise à jour simple des pages
2. un déploiement simplifié quelque soit l'hébergeur
3. système de plugins (commentaires, mise en forme, images ...)


## Mise à jour des pages
Pour avoir déjà été confronté à des blog dynamiques, plusieurs contraintes ne me satisfaisaient pas. 
Je ne voulais plus m'embarasser d'une base de données pour le stockage des posts. 
Le stockage dans une base obligeant à passer par une console d'administration pour mettre à jour les articles était une contrainte.

## Hébergement
Je n'avais pas d'idée précise quant à l'hébergement final de mon blog.
Je possède bien sûr un hébergeur mais je voulais rester souple et pouvoir en changer. Je ne voulais pas non plus être lié à une plateforme de blogging.
L'hébergement d'un site statique, fournissant du simple HTML, est celui qui présente le moins de contraintes pour la migration.

## Plugins
Enfin, bien que le site soit statique, la possibilité d'inclure des éléments dynamiques comme les commentaires était important.

#Jekyll
Mon choix s'est porté sur [Jekyll](http://jekyllrb.com/).
Jekyll répond aux contraintes que je me suis fixé mais il a en plus d'autres attraits qui m'ont attiré.  
En effet, Jekyll est le moteur qui propulse les sites hébergé par [Github](https://help.github.com/articles/using-jekyll-with-pages). J'ai un compte Github depuis longtemps mais je n'ai jamais eu l'occasion de l'utiliser. C'était donc là une bonne occasion de s'y mettre.  
Chaque moteur de blogs statiques utilise un langage propre : PHP, Python, Ruby ... Jekyll utilise Ruby.  
Démarrer avec Jekyll implique de franchir un palier technique plus ou moins important en fonction de son background :
1. Notions Ruby : installation, langage, système de gems, rakefile ...
2. Jekyll : arborescence, Liquid templating,  ...
3. Langage Markdown pour les articles

J'ai trouvé une bonne introduction aux concepts de Jekyll sur [Jekyll-Bootstrap](http://jekyllbootstrap.com/lessons/jekyll-introduction.html). Un article en français qui liste ses avantages et ses inconvénients peut être trouvé sur [http://jeremyherve.com](http://jeremyherve.com/2011/08/28/passer-de-wordpress-jekyll-premiere-partie/).

## Cycle de vie
Une fois établi le choix technique du moteur de blogging, il faut également penser au développement et déploiement de son site.
Pour ma part, mon cycle de vie est le suivant : 
1. rédaction des articles sur mon poste en local dans un simple éditeur
2. versionning gérer par Git et poussé sur un repository distant ([Github](https://github.com/Twixer/site-twixer))
3. récupération de la dernière version sur mon hébergeur
