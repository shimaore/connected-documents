Connected Documents
===================

Versions

0.1 – 2014-05-18 Version initiale
0.2 – 2014-08-13 – Removed trade marks

Application Web Bootstrap
-------------------------

Introduction
------------

L'application décrite dans le présent document est l'application web « seedée » dans l'application décrite dans le document « Application Native ».

Objectifs
---------

L'Application Web Bootstrap permet :

* la saisie d'un code
* qui est utilisé pour localiser un contenu (via un service web ad-hoc dans un domaine prédéfini)
* le contenu est chargé -- le contenu est alors responsable d'utiliser les API pour installer des composants, etc. si nécessaire.

Ce client web requiert un accès Internet pour fonctionner.

Ce client web est activé en cliquant l'icône de démarrage du service décrit dans le document « Application Native ». Du point de vue de l'utilisatrice ce client web constitue l'UI du service, même si d'un point de vue "code" le service n'a pas d'UI native en dehors des boîtes de dialogue de confirmation d'accès aux périphériques et services qu'il contrôle.

Ce client web est stockée dans un document CouchDB dans une base prédéfinie; autrement dit une URL probablement de la forme http://127.0.0.1:5984/base/start/index.html est utilisée comme URL activée en cliquant sur l'icône de l'application native.
