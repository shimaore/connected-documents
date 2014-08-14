Connected Documents
===================

Versions

0.1 – 2014-05-18 – Version initiale
0.2 -2014-08-13 – Removed trade marks

Application "Offline First"
---------------------------

Introduction
------------

L'application décrite dans le présent document permet de naviguer un contenu de façon interactive en combinant des actions dans le mobile ou ordinateur et dans le monde réel via des objets connectés.

D'une façon plus large, elle constitue le service tel qu'il est perçu par l'utilisatrice, que ce soit en mode « online » ou en mode « offline ».

Architecture
------------

L'application est une application web "offline-first".

Elle consiste en un document CouchDB qui décrit la boutique.  Les pièces jointes du document sont utilisées pour fournir les contenus (HTML, CSS, JavaScript) qui décrivent et implémentent la boutique.

L'application est « offline first » mais est utilisable à la fois:

* si l'Application Native n'est pas installée ou pas disponible, comme c'est par exemple le cas d'un accès direct sur le site web: l'Application décrite dans ce document est alors l'application principale du site web sécurisé ; dans ce cas l'application est servie depuis la base « /public » du site, et il est fait référence aux bases CouchDB situées sur les serveurs du domaine https://example.com/ ; les accès aux contenus sont sécurisés par ailleurs (mot de passe ou login utilisant les API Facebook, Twitter, ..) ;

* si l'application Native est installée et disponible: l'Application décrite dans ce document constitue l'interface utilisateur (UI) sur le mobile, PC ou Mac ; dans ce cas l'application est servie depuis la base « public » du serveur local CouchDB (qui est inclus dans l'Application Native), et il est fait référence aux bases CouchDB situées sur la machine locale, typiquement sur http://127.0.0.1:5984/ ; les accès aux contenus sont sécurisés par la présentation par le client web d'un token qui identifie l'application et son utilisatrice. (Ce token est validé par l'Application Native lors de la première création de la base de donnée de l'utilisatrice.)

L'application utilise en effet les services fournis par l'Application Native via interface HTTP locale (accès aux capteurs, actuateurs, etc.) qui sont également sécurisés par ce token.

L'application comprend entre autre la liseuse électronique.  (Une liste plus complète des fonctionnalités est fournie plus bas.)

Les services qui requièrent des interventions externes / un accès aux serveurs (tel que par exemple monétisation) sont implémentés en mode offline-first : création d'un document de demande d'achat dans une base CouchDB locale, document qui est ensuite répliqué puis validé par une opération de backend. Si une connexion Internet est disponible, l'opération est réalisée dans la foulée (par exemple, dans le cas d'une opération de monétisation, par débit sur un compte pré-payé ou via paiement par carte) ; si une connexion Internet n'est pas disponible, l'opération est mise en attente et sera résolue lors d'une prochaine réplication CouchDB.

Installation
------------

L'étape d'installation est nécessaire afin de permettre une utilisation « Offline ».

Afin de satisfaire les exigences des différentes « AppStore », l'installation se réalise en deux étapes.

L'utilisatrice doit d'abord installer le service décrit dans le document « Application Native », qui est disponible dans une « AppStore » ou peut être téléchargé depuis le site. Ce service ne contient aucune information qui le lie à un site donné.

Lorsque l'utilisatrice démarre l'icône de l'Application Native, une page web est ouverte ; cette page web est décrite dans le document « Application Web Bootstrap ». Elle consiste essentiellement en la fourniture par l'utilisatrice d'un code ; celui-ci se trouve dans le livret papier fourni avec l'objet.

Le code est traduit en une URL complète par un service en ligne (offert dans un domaine différent de celui du site). (Ce service ne fait pas parti du présent document.) Le résultat de la traduction redirige vers une URL du site.

Par exemple, le code 63728 pourrait être traduit tout d'abord par l'application « Web Bootstrap » de Connected Documents en:

    https://example.com/location/63728

L'application de Redirection Connected Documents enverrait en réponse une redirection HTTP vers l'URL

    https://example.com/public/site/index.html#install-63728

Cette dernière page, dans l'ordre:

* demande à l'utilisatrice de s'identifier (si elle a déjà créé un compte, ou en utilisant les API d'authentification de Facebook, Twitter, etc.), ou de créer un compte ;

* dans les deux cas, une fois le compte créé, réalise une première réplication de la database CouchDB de l'utilisatrice vers le mobile, PC, ou Mac;

* créé un icône sur le mobile qui pointe vers le réplica local de l'application et est clairement identifié comme étant l'application Connected Documents ;

* ajoute automatiquement le contenu correspondant au code 63728 dans la base de l'utilisatrice sur le serveur ; ce contenu est éventuellement répliqué sur le mobile/PC/Mac.

L'application Offline décrite dans le présent document est donc installée au cours de la deuxième étape du processus ci-dessus.  Elle se traduit sous la forme d'une icône "lien" vers le document de la boutique, ce qui donne une présence à la boutique en question sur la machine/ le mobile.

D'une façon pragmatique, le processus d'installation fait lui aussi parti de la présente « Application Offline », et l'URL
https://example.com/public/site/index.html donnée en exemple ci-dessus est aussi celle de cette application (mais sur le serveur).

Principaux éléments
-------------------

### Boutique

Les documents "contenu interactif" sont mis à disposition dans une boutique qui est visible sur le site web de la boutique (en mode « online ») et dans l'application offline. D'un point de vue technique, la boutique est une base de données CouchDB ; en mode online son contenu est accessible directement sur le serveur ; en mode offline le contenu répliqué sur le mobile/PC/Mac est utilisé.

La première fois qu'une utilisatrice achète un document dans la boutique ou demande l'installation de la boutique (par exemple en enregistrant son accessoire sur le site web de la boutique en question), la boutique (=la base CouchDB qui décrit la boutique) est répliquée sur le PC ou Mobile.

A priori le contenu entier de la boutique n'est pas mis à disposition sur le mobile (pour des raisons de performance et de limitation de stockage) ; une réplication filtrée (à définir et dépendante des choix de l'utilisatrice) est utilisée.

(Dans les versions préliminaires de développement, la boutique est décrite d'une part par un document dans une base nommée « public », qui contient les codes HTML, CSS, Javascript, images, etc.  qui implémentent l'application ; d'autre part par une base « shared » qui est utilisée une fois l'utilisatrice connectée et qui contient les données en propre.)

### Contenu Interactif

Chaque document « contenu interactif » acheté ou mis à disposition est stocké dans un document CouchDB, d'abord sur le serveur, puis (via réplication automatique de la base de l'utilisatrice), sur le mobile / PC. Le contenu JSON du document sert de métadata, tandis que les pièces jointes (« attachments » CouchDB) aux formats HTML, images, sons, etc. constituent le contenu en lui-même.  (A priori la liseuse démarre la lecture en exécutant la pièce jointe « index.html » du document décrivant le contenu.)

### Achat de documents

Les documents gratuits sont mis à disposition dans la base de donnée partagée (« /shared ») du serveur. Ils sont copiés par l'application de la base partagée vers la base privée de l'utilisatrice sans autre contrôle.

Les documents payant sont mis à disposition dans la base de donnée privée (« /private » ) du serveur. Ils ne sont accessible que via une API spécifique qui ne fournira accès qu'avec la fourniture par le client d'un token lié au compte de l'utilisatrice et au document en question. Le token est fourni lors de la procédure d'achat par l'API de monétisation et est stocké dans la base de l'utilisatrice (ce qui permet de charger de nouveau un contenu acheté qui aurait pu être perdu).

### Sécurité / protection du contenu des documents

TBD (question similaire à celle des DRM)

### Liseuse

La liseuse est une partie de l'application qui permet de jouer un contenu. Les contenus étant fournis sous la forme de pages HTML complètes, il est entendu que la partie applicative de la liseuse en elle-même est relativement simple (e.g. creation d'un IFRAME pour afficher le document « qui se lit tout seul »), les interactions étant encodées dans le document HTML sous la forme de tags qui sont lus par le code JavaScript embarqué dans le document CouchDB. Ce sont donc les actions décrites par ce code embarqué qui sont décrites dans cette section.

Certaines opérations sont des méta-opérations :

* reprendre la lecture où on s'en était arrêté = marque-page (l'ID de la section en cours de lecture est sauvegardé dans le document CouchDB au fur et à mesure de la lecture)
* avancer
* reculer
* aller à une page donnée
* aller à un chapitre donné à partir de la table des matières

D'autres opérations sont utilisées pour déclencher une action (« triggers »):

* flèche d'avancement
* etc.

D'autres opérations sont des actions déclenchées :

* audio
* commande de l'objet connecté
* affichage de la section suivante du contenu

D'autres sont des contrôles d'actions déclenchées :

* durée définie: bouton stop
* bouton rejouer
* supprimer ou substituer une pattern de commande que l'utilisatrice n'aime pas

Pour résumer, les interactions à développer sont :

* display
  * faire apparaître le texte normalement (i.e. cacher le texte précédent et afficher le nouveau texte)
  * faire apparaître le texte mélangé
* évènements déclencheurs
  * secouer le mobile
  * bouton logo (à cliquer, toucher)
  * bouton rejouer
  * plus évenements qui arrêtent: bouton stop
* actions
  * audio
  * (vidéo)
  * commandes (avec pattern)
  * passer à la partie suivante

(Cette liste n'est pas exhaustive ni finalisée et sera à affiner au cours des essais.)

Enfin à terme (v4+) il sera possible de personnaliser un contenu en y intégrant vidéos, sons, photos. Ces interactions seront également gérées par la « liseuse » (i.e. le code Javascript embarqué dans le document CouchDB qui décrit le contenu) ; les contenus ajoutés par l'utilisatrice seront stockés dans le document comme pièces jointes.

### Télécommande du jouet

Comme il n'y a pas de différence (autre que sémantique) entre un « contenu » et le reste de l'application, la télécommande du jouet est simplement un widget qui utilise les mêmes API que la liseuse pour déclencher certaines actions (en l'occurrence des vibrations).

Elle permet d'énumérer les modes vibratoires disponibles, de les jouer une fois ou en boucle, de marquer des modes vibratoires comme à ne pas utiliser ou à substituer, de charger de nouveaux modes vibratoires dans l'objet, de créer et d'enregistrer un nouveau mode, etc.

(D'un point de vue pratique elle peut être implémentée comme un « contenu » qui ne présente qu'une seule section, la télécommande.)

### Outils d'encodage des documents textes/images en documents sensoriels.

[Quelles sont les specs d'encodage : à spécifier dans le CdC final de « Application Offline » ? A priori ce sont des tags HTML spécifiques, l'outil d'encodage applique donc ces tags à des « paragraphs » ou « spans » définis dans le document HTML.]

Pour l'instant l'outil sera un simple éditeur de texte (client-side, e.g. ACE [http://ace.c9.io/](http://ace.c9.io/) )qui permettra de mettre à jour les fichiers index.html des documents de contenus.  A terme l'éditeur sera plus évolué, mais reste à spécifier.

## Métriques

Les métriques sont créées dans des documents spécifiques dans la base de l'utilisatrice ; ces documents sont soumis (selon les permissions accordées par l'utilisatrice) dans la base « /metrics » pour y être agrégées.

A terme, utilisation d'un service externe pour les métriques, selon le même principe, la soumission dans la base « /metrics » étant remplacée ou complétée par la soumission au service externe.

Arborescence
------------

Cette section décrit les interactions attendues au niveau applicatif, en particulier l'arborescence des services disponibles.

Les tags suivants sont appliqués à certains items :

> DONE Les interactions déjà écrites (programmées) au jour de release du présent document sont marquées « DONE ».

> EXPECTED DONE Les interactions qui seront déjà programmées au jour où le projet sera mis en route sont marquées « EXPECTED
> DONE ».

> v3, v4, v4+ Les interactions qui seront développées ultérieurement sont marquées par leur ordre de release (v3, v4, v4+).

> ADMIN, AUTHORS Les interactions réservées à une classe particulière d'utilisatrices sont marquées « ADMIN » ou « AUTHORS ».

> ONLINE ONLY Les interactions qui ne font sens qu'en mode connecté (sur les serveurs) sont marquées « ONLINE ONLY ».

### Multinational/multilingual (I18N)

Le service est d'ores et déjà multinational et multilingue dès la v1. Les développement futurs devront préserver cet aspect.

### Arborescence de services

* Login ou enregistrement (ONLINE ONLY): (DONE)
  * Facebook connect  (DONE)
  * Twitter connect  (DONE)
  * Email/mot-de-passe  (DONE)

* Compte client:
  * Achat de nouvelles/contenus
    * enregistrement de carte
    * achat avec carte mémorisée
    * achat sur compte prépayé
    * codes promo (st valentin, noel, code love, ..)
  * Gestion du profil  (DONE)
  * Gestion des permissions
  * Compte prépayé (v3)
  * Mémorisation des coordonnées bancaires (v3)
  * Mémorisation des coordonnées d'expéditions (« addressbook ») (v3)
  * Codes promotion (v4)
  * Abonnements/séries (v4+)

* Présentation des ouvrages (EXPECTED DONE)
  * image  (DONE)
  * descriptif  (DONE)
  * commentaires

* Modération des commentaires (ADMIN)

* Ma bibliothèque (EXPECTED DONE)
  * Contenus achetés
    * Lire → ouverture de la liseuse sur le contenu sélectionné
    * Laisser un commentaire
    * Partager sur facebook, twitter, via mail
  * Catégories:
    * à synchroniser / pas synchroniser
    * déjà lu
  * Permettre de créer de nouvelles catégories
  * Gestion des contenus personnalisés
    * Partager un contenu personnalisé

* Lectrices (EXPECTED DONE)
  * Comité de lecture →"recommandé par.." (utilisatrices du site ou bloggeur/se extérieur)
  * Soumission de nouveaux contenus par référence
  * Feed(s) facebook (proposés par les lectrices)
  * Feed(s) twitter (proposés par les lectrices)
  * Modération (ADMIN)

* SAV
  * Hotline – chat
  * Mail
  * Téléphone
  * Interfaces administratives pour chat, mail(?) (ADMIN)

* FAQ (peut-être ONLINE ONLY ?)
  * Accès aux FAQ
  * Recherche
  * Création de FAQ (ADMIN)

* Intégration de feeds facebook/twitter (ONLINE ONLY)

* Nouveaux auteurs
  * Section marketing qui présente de nouveaux auteurs, de nouveaux contenus, etc.
  * Page permettant à des auteurs de soumettre des textes
  * Création de contenus / backend de gestion des textes à publier (ADMIN, AUTHORS)
    * Pour la version initiale, un éditeur HTML écrit en Javascript (e.g. ACE) suffira (EXPECTED DONE)
    * Pour les versions ultérieures, un Cahier des Charges séparé sera fourni.
  * Révision des contenus avant publication (ADMIN) – a priori utilise les mêmes outils que la section « Création de contenus », il s'agit juste d'une étape de processus. (EXPECTED DONE)
  * Gestion du compte auteur / éditeur
    * Facturation des prestations fournies
    * Paiement des services facturés
    * Revenus générés par les droits d'auteur
    * Gestion des comptes bancaires
    * Gestion des contenus mis à disposition
      * Publier
      * Retirer de la diffusion

* Contacts
  * Contacts (mail, téléphone, ..)
  * CGV, CGU, Charte éditoriale
  * Mentions légales

## Développement

Browserify build
CoffeeScript
PouchDB et superagent pour les accès CouchDB et API
