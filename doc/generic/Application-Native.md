Connected Documents
===================

Application Native
------------------

Versions
0.1 – 2014-05-18 Version initiale
0.2 – 2014-08-13 Removed trademarks

Introduction
------------

Afin de satisfaire les exigences des différentes “AppStore”, l'application native est conçue comme une application ayant pour but d'augmenter les capacités d'un navigateur web existant sur la plateforme choisie.

Dans ce qui suit :
* « application » désigne l'application dont il est question dans ce document ;
* « client » désigne un navigateur web local.

Objectifs
---------

L'application native a pour but:
* Tout d'abord de fournir des fonctionnalités qui sont nécessaires mais non disponibles en l'état pour une application web embarquée:
  * capteurs:
    * captation des informations six-axis sur un mobile ou une tablette
    * captation et traitement de signal audio temps réel (détection de bruit blanc par exemple)
    * données GPS et géolocalisation(*)
    * énumération de périphériques, et réception de données bluetooth (SPP, CoreBluetooth/ANCS en émulation de capteur)
  * actuateurs:
    * contrôle du vibreur sur un mobile
    * déclenchement d'une demande d'appairement d'un périphérique Bluetooth
    * envoi de données bluetooth (SPP, ANCS)
  * services:
    * création d'une icône représentant une instance de l'application : cliquer l'icône revient à ouvrir un navigateur avec une URL fournie.

(*) Certaines fonctionnalités sont parfois disponibles suivant le navigateur.

  Les données des capteurs sont fournies au client via une API JSON basée sur WebSocket ou une API JavaScript (en fonction des possibilités de la plateforme et du mode de fonctionnement: navigateur natif de la plateforme, ou navigateur embarqué dans l'application).
  Les données des actuateurs sont transmises par le client via une API REST/JSON ou une API JavaScript.
  Une interface spécifique à la gestion des droits d'accès du client est également disponible qui permet d'énumérer les périphériques et services disponibles, ainsi que les droits qui ont été autorisés auprès de l'application.

* De fournir les fonctionnalités qui sont nécessaires au stockage de données “offline” et à leur synchronisation avec des services centralisés. La solution choisie est basée sur CouchDB et son algorithme solide de réplication.

  Plusieurs approches sont envisageables:
  - une approche en “navigateur pur” en utilisant PouchDB comme solution dans le navigateur; dans ce cas il ne serait pas nécessaire que l'application native fournisse les services correspondants; malheureusement cette solution se heurte à des limitations fortes sur iOS (50Mo maximum de taille de stockage);
  - une approche qui inclue les fonctionnalités requises dans l'application native, soit en incluant CouchDB sur les plateformes où il est directement supporté, soit en utilisant une version native; la solution à suivre dans ce cas est celle fournie par le travail du projet HoodieHQ; en particulier il est intéressant de suivre les développements sur Android et iOS de HoodieHQ.

Dans tous les cas, l'Application Native authentifie les accès via un token qui est créé lors de la première réplication de la base de données de l'utilisatrice; lors de cette première session l'utilisatrice doit confirmer sa volonté de permettre au client d'accéder aux services de l'Application Native. Ce token est fourni au client, qui doit le fournir en retour lorsque requis par l'Application Native. (Ceci afin d'éviter que des clients web tierces n'essaient d'utiliser les services de l'Application Native sans autorisation.) [Note: revalider le modèle de sécurité utilisé ici, l'idée étant que le client stockera son token d'accès dans un service de stockage du navigateur web, dont on attend qu'il soit lié à l'URL de la page qui demande le stockage.]

Plateformes
-----------

Spécifiquement les plateformes suivantes sont désignées, par ordre d'importance décroissant:
- iOS
- Android
- Windows (PC)
- MacOSX

D'autre part:
- Linux (développement / démo) – avec l'objectif de servir de “solution de référence” pour le développement sur les autres plateformes.
- Windows Mobile : pas prévu
[A réaliser : tableau décrivant le mapping entre plateforme et fonctionnalités attendues telles que décrites dans la section précédente.]

Interface Utilisateur
---------------------

L'application native se présentant sous la forme d'un service (tâche de fond), il est attendu que l'interface utilisateur sera réduite à des boîtes de dialogues afin de confirmer l'accès aux services demandés par le navigateur (similairement à la façon dont par exemple l'accès aux services de géolocalisation peut être confirmé par le navigateur).

Spécifiquement :
- Si une requête d'accès à une ressource est présentée par un client HTTP sans autorisation (token individuel ou cookie de session):
  - l'application confirme (via boîte de dialogue) que le client (représentée par un identificateur fourni dans la requête) est bien autorisée ;
  - selon que l'utilisatrice choisit « cette fois-ci seulement » ou « toujours », l'application génère un token et le lie à un cookie renvoyé à l'application ; la durée de vie du token et du cookie sont liés à la réponse de l'utilisateur. Si la durée de vie dépasse une session, le token est également renvoyé au client, qui doit le stocker pour une utilisation ultérieure (par exemple après redémarrage du navigateur ou du système).
  - les requêtes subséquentes utilisant le cookie de session ne déclenchent pas de confirmation.
- Si une requête d'accès est présentée par un client HTTP avec un token valide, l'accès est accordé automatiquement.
- Le client peut énumérer:
  - les capteurs et actuateurs disponibles (en particulier les périphériques bluetooth appairés qui sont compatibles avec l'application) ;
  - pour chaque capteur ou actuateur, si le périphérique en question a été autorisé ou pas (pour ce client spécifique, représenté par son identificateur).

Note : Déploiement
------------------

L'application sera 'seedée' avec un client web (très simple) dont la seule fonctionnalité est de permettre:
- la saisie d'un code
- qui est utilisé pour localiser le contenu (via un service web ad-hoc dans un domaine prédéfini qui sera fourni)
- le contenu est chargé -- le contenu est alors responsable d'utiliser les API pour installer des composants, etc. si nécessaire.
Ce client web requiert un accès Internet pour fonctionner.
Ce client web est activé en cliquant l'icône de démarrage du service. Du point de vue de l'utilisatrice ce client web constitue l'UI du service, même si d'un point de vue "code" le service n'a pas d'UI native en dehors des boîtes de dialogue de confirmation d'accès aux périphériques et services.
Ce client web est stockée dans un document CouchDB dans une base prédéfinie; autrement dit une URL probablement de la forme http://127.0.0.1:5984/base/start/index.html est utilisée comme URL activée en cliquant sur l'icône de l'application native.
Le développement de ce client web n'est pas prévu dans ce cahier des charges mais est décrit dans un document séparé, « Application Web Bootstrap ». Néanmoins son intégration est à prévoir dans le développement de l'application décrite ici afin de pouvoir l'intégrer correctement.

Références
----------
* PouchDB disponible dans Safari/iOS et Android? Oui mais limité à 50MB dans Safari/iOS, see http://pouchdb.com/faq.html
* CouchBase-Lite-HTTP-Listener (voir dessin dans https://github.com/couchbaselabs/Couchbase-Lite-PhoneGap-Plugin#architecture )
  https://github.com/couchbase/mobile
  * https://github.com/couchbase/couchbase-lite-android/wiki/Project-structure
  * https://github.com/couchbase/couchbase-lite-ios/tree/master/LiteServ%20App
* "What about CouchApps" in http://docs.couchbase.com/couchbase-lite/cbl-concepts/#compatibility
