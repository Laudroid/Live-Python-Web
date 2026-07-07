# 3-2-5-Structure de projet Flask

Contrairement à d'autres frameworks web (comme Django), Flask est qualifié de "non-opiniâtre" (unopinionated). Il ne vous impose aucune structure de dossiers ou de fichiers. Si cette liberté est un avantage pour démarrer rapidement, elle peut devenir un piège lorsque l'application grandit. 

Adopter une structure de projet claire et évolutive dès le départ est indispensable pour garantir la maintenabilité du code, faciliter le travail en équipe et séparer les responsabilités (logique métier, accès aux données, présentation).

## 1. Structure basique (Petits projets)

Pour un prototype, une API très simple ou un micro-service, une structure "plate" (flat structure) est suffisante. Tout le code Python est centralisé dans un seul fichier.

```text
api_inventaire/
│
├── app.py               # Point d'entrée, routes et logique métier
├── requirements.txt     # Liste des dépendances (Flask, requests, etc.)
├── .env                 # Variables d'environnement (secrets, clés API)
│
├── static/              # Fichiers statiques (CSS, JS, images)
│   └── style.css
│
└── templates/           # Fichiers HTML (Jinja2)
    ├── base.html
    └── index.html
```

## 2. Structure modulaire (Projets moyens à grands)

Dès que votre application dépasse quelques dizaines de routes ou intègre une base de données, le fichier `app.py` devient illisible. Les bonnes pratiques actuelles recommandent d'utiliser le modèle de la **Fabrique d'Application (Application Factory)** couplé aux **Blueprints**.

### A. L'arborescence recommandée

```text
api_inventaire/
│
├── run.py               # Script ultra-léger pour démarrer le serveur
├── config.py            # Classes de configuration (Dev, Prod, Test)
├── requirements.txt
├── .env
│
└── app/                 # Le package Python contenant votre application
    ├── __init__.py      # Contient la fonction "Application Factory"
    ├── extensions.py    # Initialisation des plugins (SQLAlchemy, Login...)
    ├── models.py        # Définition des schémas de base de données
    │
    ├── main/            # Un Blueprint (ex: pages publiques)
    │   ├── __init__.py
    │   └── routes.py
    │
    ├── equipements/     # Un autre Blueprint (ex: gestion de l'inventaire)
    │   ├── __init__.py
    │   └── routes.py
    │
    ├── static/          # Fichiers statiques globaux
    └── templates/       # Templates HTML globaux
```

### B. L'Application Factory (`app/__init__.py`)

Au lieu de créer l'instance de l'application globalement (`app = Flask(__name__)`), on l'encapsule dans une fonction `create_app()`. Cela permet de créer plusieurs instances de l'application (très utile pour les tests automatisés) et d'éviter les problèmes d'importations circulaires.

```python
from flask import Flask
from config import Config
# from app.extensions import db

def create_app(config_class=Config):
    app = Flask(__name__)
    app.config.from_object(config_class)

    # Initialisation des extensions (ex: base de données)
    # db.init_app(app)

    # Enregistrement des Blueprints
    from app.main.routes import bp as main_bp
    app.register_blueprint(main_bp)

    from app.equipements.routes import bp as equipements_bp
    app.register_blueprint(equipements_bp, url_prefix='/equipements')

    return app
```

### C. Les Blueprints (`app/equipements/routes.py`)

Un **Blueprint** est un moyen d'organiser un groupe de routes et de vues liées entre elles. C'est un sous-ensemble de l'application.

```python
from flask import Blueprint, render_template

# Création du Blueprint
bp = Blueprint('equipements', __name__)

@bp.route('/liste')
def liste():
    return render_template('liste_equipements.html')
```

## 3. Architecture et Flux d'initialisation

Le diagramme suivant illustre comment les différents composants s'articulent lors du démarrage de l'application avec le modèle de la Fabrique d'Application.

```mermaid
graph TD
    A[run.py<br/>Point d'entrée] -->|Importe et exécute| B(app/__init__.py<br/>Fonction create_app)
    
    subgraph Fabrique d'Application
        B --> C{Initialisation}
        C --> D[Chargement de config.py]
        C --> E[Liaison avec extensions.py<br/>ex: Base de données]
        C --> F[Enregistrement des Blueprints]
    end
    
    subgraph Modules (Blueprints)
        F --> G[Blueprint : Main<br/>Routes publiques]
        F --> H[Blueprint : Équipements<br/>Routes d'inventaire]
        F --> I[Blueprint : API<br/>Endpoints JSON]
    end
    
    G --> J((Application Flask<br/>Prête à recevoir<br/>des requêtes))
    H --> J
    I --> J
```

---
**Sources utilisées :**
*   *The Flask Mega-Tutorial (Édition 2024) par Miguel Grinberg - Part XV: A Better Application Structure* (blog.miguelgrinberg.com)
*   *Documentation officielle Flask (3.1.x) - Application Setup / Application Factories* (flask.palletsprojects.com/en/stable/patterns/appfactories/)
*   *Documentation officielle Flask (3.1.x) - Modular Applications with Blueprints* (flask.palletsprojects.com/en/stable/blueprints/)