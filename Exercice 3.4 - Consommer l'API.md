# Exercice 3.4 — Développer une application Flask avec API REST, interface Jinja2 et base SQLite3

## Objectifs pédagogiques

À l'issue de cet exercice, vous serez capable de :

* créer une application Flask combinant une API REST et une interface Web ;
* utiliser une base de données SQLite3 pour stocker des informations ;
* réaliser les opérations essentielles d'un CRUD (Create, Read, Update, Delete) ;
* concevoir une interface HTML dynamique avec Jinja2 ;
* créer un formulaire HTML permettant d'ajouter des données ;
* consommer une API REST avec la bibliothèque `requests` ;
* gérer correctement les erreurs de communication avec une API.

---

# Contexte

Une entreprise souhaite développer une petite application permettant de gérer son parc d'équipements informatiques.

L'application devra être accessible de deux façons :

* par une API REST destinée à d'autres applications ;
* par une interface Web destinée aux utilisateurs.

Toutes les données devront être enregistrées dans une base de données SQLite3.

---

# Travail demandé

## Étape 1 — Préparer le projet

* Créez un nouveau projet Python et mettez en place un environnement virtuel.
* Installez les bibliothèques nécessaires au développement de l'application.
* Organisez votre projet de manière à séparer le code Python, les templates HTML et la base de données.

---

## Étape 2 — Créer la base de données

Créez une base SQLite3.
Cette base devra contenir une table permettant de stocker les équipements.
Chaque équipement devra posséder au minimum :

* un identifiant unique ;
* un nom ;
* un état (actif ou inactif).

Vérifiez que la table est correctement créée.

---

## Étape 3 — Initialiser Flask

Créez une application Flask.
Assurez-vous que celle-ci démarre correctement et qu'une première page de test est accessible depuis un navigateur.

---

## Étape 4 — Accéder à la base de données

Écrivez les fonctions permettant :

* d'ouvrir une connexion à SQLite ;
* d'exécuter une requête SQL ;
* de récupérer les résultats ;
* de fermer correctement la connexion.

Ces fonctions devront être réutilisées dans toute l'application.

---

## Étape 5 — Développer l'API REST

Créez les routes permettant d'exposer les données sous forme JSON.

Votre API devra permettre :

* d'obtenir la liste complète des équipements ;
* d'ajouter un nouvel équipement.

Vérifiez le bon fonctionnement de ces routes avec un navigateur ou un outil de test d'API.

---

## Étape 6 — Réaliser l'interface Web

Ajoutez une nouvelle page Web affichant le parc des équipements.
Cette page devra être générée avec un template Jinja2.
Les données affichées devront provenir de la base SQLite.

---

## Étape 7 — Construire le template Jinja2

Dans votre template :

* affichez les équipements dans un tableau HTML ;
* utilisez une boucle Jinja2 pour parcourir les données ;
* affichez le nom et l'identifiant de chaque équipement ;
* affichez l'état de chaque équipement.

L'état devra être affiché différemment selon que l'équipement est actif ou inactif.

---

## Étape 8 — Ajouter un formulaire

Ajoutez au-dessus du tableau un formulaire HTML permettant de créer un nouvel équipement.

Le formulaire devra comporter :

* un champ de saisie pour le nom ;
* un bouton d'envoi.

Lors de la validation :

* les données devront être envoyées au serveur ;
* le nouvel équipement devra être enregistré dans SQLite ;
* la page devra être réaffichée avec la nouvelle liste.

---

## Étape 9 — Ajouter des actions sur les équipements

Pour chaque ligne du tableau, ajoutez des boutons permettant :

* de modifier l'état de l'équipement (actif ↔ inactif) ;
* de supprimer l'équipement.

Après chaque action, l'utilisateur devra être redirigé vers la liste mise à jour.

---

## Étape 10 — Développer un client Python

Créez un second programme Python jouant le rôle de client.

Ce programme devra communiquer avec l'API REST à l'aide de la bibliothèque `requests`.

Il devra :

1. récupérer la liste des équipements ;
2. afficher cette liste dans le terminal ;
3. ajouter un nouvel équipement via l'API ;
4. récupérer de nouveau la liste ;
5. afficher le résultat.

---

## Étape 11 — Gérer les erreurs

Tous les appels HTTP devront être sécurisés.

Votre programme devra notamment :

* définir un délai maximal d'attente pour chaque requête ;
* vérifier que la réponse du serveur est valide ;
* afficher un message clair si le serveur est indisponible ;
* éviter tout arrêt brutal du programme en cas d'erreur réseau.

---

## Étape 12 — Vérifier le fonctionnement

Vérifiez que les opérations suivantes fonctionnent correctement :

* création de la base de données ;
* ajout d'un équipement via le formulaire Web ;
* affichage dynamique des données dans le template Jinja2 ;
* modification de l'état d'un équipement ;
* suppression d'un équipement ;
* consultation de l'API REST ;
* ajout d'un équipement via le client Python.

---

# Livrables attendus

Le projet devra contenir au minimum :

* le fichier principal de l'application Flask ;
* le client Python utilisant `requests` ;
* la base SQLite3 (ou son script de création) ;
* le dossier `templates` contenant les pages Jinja2 ;
* un document `USAGE-IA.md` décrivant l'utilisation éventuelle d'une IA générative pendant le développement.

---

# Critères d'évaluation

L'évaluation portera sur les points suivants :

| Critère             | Points d'attention                                                     |
| ------------------- | ---------------------------------------------------------------------- |
| Base SQLite         | Création correcte et accès fonctionnel                                 |
| API REST            | Routes opérationnelles et réponses JSON conformes                      |
| CRUD                | Ajout, consultation, modification et suppression fonctionnels          |
| Interface Jinja2    | Utilisation correcte des variables, boucles et conditions              |
| Formulaire HTML     | Enregistrement correct des données dans SQLite                         |
| Client `requests`   | Appels HTTP correctement réalisés                                      |
| Gestion des erreurs | Timeout, vérification des réponses et traitement propre des exceptions |
| Qualité du code     | Organisation, lisibilité et séparation des responsabilités             |
