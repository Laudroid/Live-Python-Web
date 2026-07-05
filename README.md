# Cours Python

## Objectifs de la formation

- Savoir créer des APIs web avec Flask et Django
- Savoir créer des sites web avec Flask et Django
- Apprendre à respecter les bonnes pratiques du développement web
## Séance 1 : Jour 1 - Fondamentaux du Web et Python

**Durée :** 7 heures

### Objectifs pédagogiques

- Comprendre le protocole HTTP et les conventions REST.
- Maîtriser les bases de Python 3.14, y compris la POO.
### Contenus

#### Introduction au Web et HTTP

- Introduction au Web: historique, concepts clés.
- Le protocole HTTP en détail: requêtes (GET, POST, PUT, DELETE), réponses (codes de statut), ressources (URL), en-têtes (Host, User-Agent, Content-Type, Authorization), méthodes, idempotence.
- Bonnes pratiques du Web et conventions RESTful: principes de conception d'API REST.
#### Bases de Python 3.14 et POO

- Installation de Python 3.14, environnements virtuels.
- Variables, types de données (int, float, str, list, dict, tuple, set), opérateurs.
- Structures de contrôle (if/elif/else, for, while).
- Fonctions: définition, arguments, valeurs de retour, lambda.
- Programmation Orientée Objet: classes, objets, attributs, méthodes, héritage, encapsulation, polymorphisme.
## Séance 2 : Jour 2 - Persistance des Données avec Python

**Durée :** 7 heures

### Objectifs pédagogiques

- Interagir avec des bases de données SQL et NoSQL en Python.
- Appliquer les concepts de la POO pour la persistance des données.
### Contenus

#### Interaction avec une Base de Données SQL

- Introduction aux bases de données relationnelles.
- SQL en Python: utilisation de `sqlite3` (base de données embarquée) ou SQLAlchemy (ORM).
- Création de tables, insertion, mise à jour, suppression et récupération de données.
- Modélisation de données simples.
#### Interaction avec une Base de Données NoSQL

- Introduction aux bases de données NoSQL: types (document, clé-valeur, colonne large).
- Découverte de MongoDB (base de données orientée document).
- Utilisation de `pymongo` pour interagir avec MongoDB: connexion, insertion, recherche, mise à jour, suppression de documents.
## Séance 3 : Jour 3 - Développement Web avec Flask

**Durée :** 7 heures

### Objectifs pédagogiques

- Comprendre les avantages et inconvénients de Flask.
- Créer des APIs RESTful avec Flask.
- Développer des pages web interactives avec Flask et Jinja.
### Contenus

#### Introduction à Flask et APIs

- Avantages et inconvénients de Flask vs Django (cas d'utilisation).
- Présentation de Flask: micro-framework, principes, écosystème.
- Installation de Flask et création de la première application.
- Routing, vues, réponses HTTP (JSON).
- Création d'APIs web (GET, POST, PUT, DELETE) pour une ressource simple (ex: Tâches).
- Consommation de l'API Flask avec un script Python client (`requests`).
#### Pages Web et Entrées Utilisateur avec Flask

- Utilisation de Jinja2 pour les templates HTML.
- Rendu de templates, passage de données au template.
- Formulaires HTML et traitement des entrées utilisateur (GET/POST, `request` object).
- Gestion des sessions et des messages flash.
- Structure de projet Flask.
## Séance 4 : Jour 4 - Développement Web avec Django

**Durée :** 7 heures

### Objectifs pédagogiques

- Comprendre l'architecture de Django.
- Créer un site web complet avec des pages et des APIs REST.
- Mettre en place l'authentification et l'autorisation utilisateur.
### Contenus

#### Introduction à Django et Pages Web

- Présentation de Django: full-stack framework, philosophie DRY, MTV (Model-Template-View).
- Installation de Django, création d'un projet et d'une application.
- Modèles Django (ORM), migrations.
- Vues (fonctions et classes), URLs, templates Django.
- Création de pages web simples et gestion des données via l'ORM.
#### Django REST Framework et Authentification

- Introduction à Django REST Framework (DRF): sérialiseurs, vues d'API.
- Création d'APIs RESTful avec DRF pour les modèles existants.
- Consommation d'APIs externes depuis une application Django (e.g., `requests` library).
- Mise en place d'utilisateurs Django, formulaires de login/logout.
- Gestion des rôles simple (ex: staff, admin, utilisateurs normaux) via les permissions Django.