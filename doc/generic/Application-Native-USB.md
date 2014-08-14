Connected Documents
===================

Versions

0.1 – 2014-05-18 – Version initiale
0.2 – 2014-08-13 – Removed trademarks

Introduction
------------

L'application Native USB permet d’accéder au bus USB afin de réaliser des opérations en lien avec un objet connecté, en particulier la mise à jour des firmware.

Objectifs
---------

L'objectif est de fournir une interface HTTP au bus USB afin de faciliter le déploiement de nouvelles fonctionnalités dans les objets connectés.

L'interface USB implémentée par les objets est très simple, et l'application native USB doit être capable de :

* énumérer les objets connectés
* envoyer une séquence courte (8 octets) à l'objet et fournir
  la réponse reçue.

La partie protocolaire (contenu des séquences) est gérée par
le client HTTP.

## Plateformes

* Windows
* MacOSX

Additionnellement

* Linux (développement/démo) – avec l'objectif de servir de
  “solution de référence” pour le développement sur les autres
  plateformes.

## Interface Utilisateur

TBD

## Références

TBD
