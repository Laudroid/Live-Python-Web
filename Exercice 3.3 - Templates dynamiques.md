# Exercice 3.3 — Jinja2 multi-pages

## Navigation + templates dynamiques (sans base de données)

---

## Objectifs pédagogiques

* structurer une application Flask avec plusieurs routes ;
* générer plusieurs pages HTML avec Jinja2 ;
* utiliser des templates réutilisables ;
* exploiter des données simulées côté serveur (Python) ;
* manipuler boucles et conditions dans Jinja2 ;
* construire une navigation entre pages ;
* séparer clairement logique serveur et affichage.

---

# Contexte

Vous développez une mini application de type **tableau de bord de gestion de parc informatique**.

Les données sont **simulées en mémoire (liste Python)**.

L’application doit permettre :

* d’afficher une liste globale des équipements ;
* de voir uniquement les équipements actifs ;
* de consulter une page de détail par équipement ;
* de naviguer entre les pages via des liens HTML.

---

# Structure proposée

```
app.py
/templates
    base.html
    index.html
    actifs.html
    detail.html
```

---

# Travail demandé

---

## Étape 1 — Initialiser l’application Flask

Créer une application Flask fonctionnelle.
L’application doit pouvoir afficher une page d’accueil.

---

## Étape 2 — Créer les données simulées

Définir côté serveur une liste Python représentant des équipements.
Chaque équipement contient :

* un identifiant ;
* un nom ;
* un statut (actif / inactif) ;
* une catégorie (serveur, poste, réseau, etc.).

Ces données sont **fixes et non modifiables**.

---

## Étape 3 — Créer une structure de templates

Mettre en place :

* un template principal servant de base commune ;
* une structure avec bloc de contenu réutilisable ;
* un menu de navigation commun à toutes les pages.

Le menu doit permettre de naviguer entre les pages principales.

---

## Étape 4 — Page d’accueil (liste complète)

Créer une route et une page affichant :

* la liste complète des équipements ;
* toutes leurs informations.

Dans le template :

* utiliser une boucle Jinja2 ;
* afficher chaque équipement dans un bloc structuré ;
* distinguer visuellement les équipements actifs et inactifs avec une condition.

---

## Étape 5 — Page “équipements actifs”

Créer une page dédiée affichant uniquement :
* les équipements actifs.

La logique de filtrage doit être réalisée côté serveur (Python), pas dans le template.

Dans le template :
* afficher uniquement les éléments fournis par la vue ;
* utiliser une boucle simple sans logique complexe.

---

## Étape 6 — Page de détail d’un équipement

Créer une route dynamique permettant d’afficher :
* un équipement spécifique selon son identifiant.

La page doit afficher :
* toutes les informations de l’équipement ;
* un message d’erreur si l’identifiant n’existe pas.

Dans le template :
* utiliser des variables Jinja2 ;
* utiliser une condition pour gérer le cas “non trouvé”.

---

## Étape 7 — Navigation entre pages

Dans le template principal :
* ajouter un menu de navigation commun ;
* permettre d’accéder à toutes les pages ;
* assurer une cohérence visuelle sur toutes les vues.

---

## Étape 8 — Mise en forme Jinja2 obligatoire

Dans les templates, vous devez utiliser :
* des boucles `{% for %}` ;
* des conditions `{% if %}` ;
* des variables `{{ }}` ;
* au moins un bloc réutilisable (`block`) ;
* une logique de filtrage côté Python (obligatoire).

---

## Étape 9

Ajouter :
* une page “statistiques” affichant :

  * nombre total d’équipements ;
  * nombre d’actifs ;
  * nombre d’inactifs ;
* un message conditionnel si la liste est vide dans une page.

---

# Livrables attendus

* application Flask fonctionnelle ;
* plusieurs routes Python ;
* 3 à 4 templates Jinja2 ;
* aucune base de données ;
* aucune méthode POST ;
* aucune modification de données.
* fichier USAGE-IA.md

