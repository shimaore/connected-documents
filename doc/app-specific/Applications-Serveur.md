Connected Documents
===================

Versions

0.1 – 2014-05-18 – Version initiale
0.2 – 2014-08-13 – Removed trade marks

# Application "Online" (Serveur)

## Introduction

Le site web public est la partie SEO.

Le site web privatif (utilisatrices) est la partie décrite dans le document « Application Offline », y compris les extensions marquées « ONLINE ONLY ». Par contre ce document ne décrit pas les interfaces serveurs.

Le présent document décrit les attentes d'ensemble du site (phasing) et les besoins qui en découlent du côté serveur.

## Phasing

Ouverture (v1): (DONE)
* visuel + concept
* s'inscrire pour devenir "early adopteuse"
* actu (veille)

L'objectif initial était d'utiliser Drupal mais un site dédié a été créé.

Lancement (v2) (*en cours d'écriture*):

* concept, etc sur site SEO public
* site privatif :
  * création de compte, login (email, Facebook, Twitter) (DONE)
  * site online only (en cours) avec DB utilisatrices (DONE)
  * création d'une communauté de lectrices via la soumission de titres (papier, électroniques, textes en ligne, ..) (*en cours*) et les commentaires (*en cours*)

Campagne de financement (v3):

* crowdfunding
* pré-achat sur le site

Site post-funding (v4):

* TBD

Services offerts par le serveur
-------------------------------

### Module de paiement

Le but du module de paiement est d'être capable de construire des tokens d'autorisation de chargement de contenus (et autres).  Les tokens sont systématiquement liés à :

* une utilisatrice (représentée par son uuid)
* un objet (représenté par son uuid)

Tout token créé pour un contenu numérique peut être utilisé un nombre indéfini de fois (afin de permettre le chargement sur plusieurs périphérique / le re-chargement d'un contenu en cas de perte du mobile / etc.).

Les abonnements prépayés sont convertis en une séquence d'objets (chaque objet de la séquence d'abonnement a son propre uuid) au moment de leur création. Lors de l'achat d'un abonnement, chaque élément de la séquence reçoit un token. Les tokens sont échangés ultérieurement contre les contenus, au fur et à mesure qu'ils sont mis à disposition.

Les tokens sont créés lors du paiement d'une commande. Une commande peut contenir plusieurs objets (y compris des abonnements), la validation de la commande entraîne la création d'un token pour chaque élément de la commande.

L'argent est disponible sous la forme de :

* compte prépayé
* paiement en direct (carte de crédit)
* paiement différé (virement)
* code promotionnels

### Paiement offline

[Note : HoodieHQ implémente le paiement offline, regarder comment cela a été implémenté. A priori un document « ordre d'achat » est créé sur le mobile/PC s'il est offline; lors du passage en mode « online » l'Application Offline sur le mobile/PC réalise l'opération de paiement telle qu'elle est demandée dans l'ordre d'achat.]

### Multinational, multilingual (I18N)

Dès la v1, support pour le Français (France : fr-FR) et l'anglais (en-US et/ou en-UK).

Tâches/difficultés attendues lors de l'ajout d'un nouveau pays et/ou d'une nouvelle langue :

* traduction des applications
* éditeur de contenu (support des jeux de caractères, multidirectionnel)
* monnaie, système bancaire (accepter les paiements en monnaie locale)
* interfaces avec les services d'expéditions, les douanes
* gestion des règles d'édition et de publication (business rules)
* SAV (mail, chat, téléphone)

## Backends

L'application décrite dans ce document s'interface donc :

* avec CouchDB ; elle fournit en particulier des services d'escalation de droits, qui sont contrôlés par des règles écrites dans le code (par exemple pouvoir poster un document dans la base « /private » afin de contacter le support etc.) ou via les tokens (pour l'accès aux contenus qui sont stockés dans la base « /private », les contenus gratuits étant stockés et librement accessible dans la base « /shared »).
* backend de solution(s) de paiements
* backend des services de livraison (à voir : il est peut-être possible de faire ça côté client si des API sont disponibles)

Développement

Node.js:

* CoffeeScript
* ZappaJS pour les services frontaux (fourniture des API à l'Application Offline, frontal d'authentification CouchDB)
* PouchDB et superagent pour les accès CouchDB et API backends

CouchDB

* Serveurs installés, ou service (BigCouch/Cloudant, ..)
