---
layout: post
title: "Trucs &amp; Astuces pour Maven sous Windows"
description: ""
category: hints and tips
tags: [maven, windows, oracle, jdbc]
---
{% include JB/setup %}

#Introduction
Cet article liste un ensemble de commandes et astuces pour Maven. Comme je travaille la plupart du temps sous Windows, certaines astuces seront liées à cet environnement.
Cet article est également un moyen pour moi de garder une liste des commandes que je cherche régulièrement.

---------------------------------------
#Trucs & Astuces
##Où se trouve le repository local Maven sous Windows ?
Le repository local Maven se trouve à la racine de son répertoire de profil dans le répertoire .m2

- *c:\Profiles\NOM_PROFIL\.m2\repository*

##Comment trouver la déclaration d'une dépendance pour une bibliothèque donnée ?
Prenons l'exemple de la bibliothèque Spring que nous voulons utiliser sur notre projet. Pour trouver la déclaration du POM on peut se rendre sur le site du [repository central Maven][] et taper le nom de notre bibliothèque dans le champ de recherche :  
![maven_spring_search][maven_spring_search]

On aura donc les résultats suivants pour Spring : 
- Spring Context 
- Spring Core
- Spring Beans
- Spring TestContext Framework 
- ...

Si c'est Spring Core qui nous intéresse nous arriverons sur une page avec la liste des version disponibles :  
![maven_spring_versions][maven_spring_versions]

Un simple clic sur une des versions fournira la bonne déclaration de dépendance à ajouter à son POM :  
![maven_spring_320][maven_spring_320]

##Comment ajouter un JAR qui n'est pas dans le repository central de Maven ?
Exemple avec [les drivers JDBC Oracle][], le POM dans le repository Maven est le suivant :  
{% highlight xml %}
  <dependency>
  	<groupId>com.oracle</groupId>
  	<artifactId>ojdbc14</artifactId>
 	<version>10.2.0.4.0</version>
  </dependency>
{% endhighlight %}
Si on ajoute cette dépendance dans un POM, on aura l'erreur suivante sous Eclipse :  
> Missing artifact com.oracle:ojdbc14:jar:10.2.0.4.0	pom.xml	/gri-persistence	line 45	Maven Dependency Problem

Si on va jeter un coup d'oeil dans le repository maven en local, en ouvrant le POM de notre artefact, on trouve le bloc suivant : 
{% highlight xml %}
    <name>Oracle JDBC Driver</name>
    <description>Oracle JDBC driver classes for use with JDK1.4</description>
    <url>http://www.oracle.com/technology/software/tech/java/sqlj_jdbc/index.html</url>
{% endhighlight %}
Ce bloc est suivi d'un bloc licence : 
{% highlight xml %}
	<licenses>
		<license>
			<name>Oracle Technology Network Development and Distribution License Terms</name>
			<url>http://www.oracle.com/technology/software/htdocs/distlic.html</url>
		</license>
	</licenses>
{% endhighlight %}
En raison de la licence, le JAR des drivers JDBC d'Oracle ne peut pas être déposé dans le repository central Maven. Par contre rien ne nous empêche de l'ajouter en local.  
Pour réaliser cela, il faut se rendre à l'URL spécifiée dans le POM pour télécharger le JAR en question : [drivers JDBC Oracle][]. Une fois le JAR en local (par exemple ojdbc6.jar), on doit taper la commande suivante :
{% highlight console %}
	mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=10.2.0.4.0 -Dpackaging=jar -Dfile=ojdbc6.jar -DgeneratePom=true
{% endhighlight %}

La même chose est à réaliser pour l'artefact [JTA en version 1.0.1B][] : 
{% highlight console %}
	mvn install:install-file -DgroupId=javax.transaction -DartifactId=jta -Dversion=1.0.1B -Dpackaging=jar -Dfile=jta-1_0_1B.jar -DgeneratePom=true
{% endhighlight %}

---------------------------------------
#Ressources
[repository central Maven][]  
[Stackoverflow.com][]  

[repository central Maven]: http://mvnrepository.com/
[Stackoverflow.com]: http://stackoverflow.com/
[les drivers JDBC Oracle]: http://stackoverflow.com/questions/1074869/find-oracle-jdbc-driver-in-maven-repository

[drivers JDBC Oracle]: http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html
[JTA en version 1.0.1B]: http://www.oracle.com/technetwork/java/javaee/tech/jta-138684.html

[maven_spring_search]: /assets/images/maven_repository_spring_search.png
[maven_spring_versions]: /assets/images/maven_spring_versions.png
[maven_spring_320]: /assets/images/maven_spring_3.2.0.png
