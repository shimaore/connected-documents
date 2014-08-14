Connected Documents
===================

Versions

0.1 – 2014-05-16 – Version initiale 
0.2 – 2014-08-13 – Removed trade marks 

Application Redirection Connected Documents

Introduction
------------

L'application décrite dans le présent document permet de traduire 
un identifiant alphanumérique unique en une URL. 

Architecture
------------

Cette section est reprise du document « Application Offline ».

« Le code est traduit en une URL complète par un service en ligne 
(offert dans un domaine différent); le résultat de la traduction 
redirige vers une URL propre au contenu.

Par exemple, le code 63728 pourrait être traduit par l'URL https://example.com/install.html#63728

Contenus
--------

API REST/JSON pour permettre à des applications tierces d'introduire des traductions.

Redirection web standard pour les traductions.

Développement
-------------

* Node.js
* CoffeeScript
* PouchDB et superagent pour les accès CouchDB et API
