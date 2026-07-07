# 3-2-2-Rendu de templates, passage de données au template

Le rendu de templates consiste à générer une page HTML dynamique en combinant un fichier statique (le template) avec des données provenant du serveur. Dans Flask, cette opération est réalisée par la fonction `render_template()`. 

L'injection de données permet de personnaliser l'affichage pour chaque utilisateur ou chaque requête, en transmettant des variables Python directement au moteur Jinja2.

## 1. Le mécanisme d'injection de données

La fonction `render_template()` prend en premier argument le nom du fichier HTML (situé dans le dossier `templates`). Tous les arguments nommés (keyword arguments) qui suivent sont transmis au template en tant que variables.

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/profil')
def profil():
    # Passage d'une variable simple nommée 'pseudo'
    return render_template('profil.html', pseudo="admin_noc")
```

Dans le fichier `profil.html`, la variable sera accessible via la syntaxe Jinja2 `{{ pseudo }}`.

## 2. Passer des structures de données complexes

Flask ne se limite pas aux chaînes de caractères ou aux entiers. Vous pouvez transmettre n'importe quel objet Python : listes, dictionnaires, tuples, ou même des instances de classes personnalisées.

**Exemple côté Python (`app.py`) :**
```python
@app.route('/parc')
def parc():
    # Une liste de dictionnaires simulant des données issues d'une base de données
    equipements = [
        {"id": 1, "hostname": "srv-web-01", "ip": "192.168.1.10", "actif": True},
        {"id": 2, "hostname": "sw-access-02", "ip": "192.168.1.2", "actif": False},
        {"id": 3, "hostname": "rtr-core-01", "ip": "10.0.0.1", "actif": True}
    ]
    
    infos_parc = {
        "nom": "Datacenter DC1",
        "site": "Paris"
    }
    
    return render_template(
        'parc.html', 
        liste_equipements=equipements, 
        parc=infos_parc
    )
```

**Exemple côté Template (`templates/parc.html`) :**
Jinja2 permet d'accéder aux clés d'un dictionnaire de deux manières : `parc['nom']` ou `parc.nom`. La notation par point est généralement privilégiée pour sa lisibilité.

```html
<h1>Parc {{ parc.nom }} ({{ parc.site }})</h1>

<h2>Équipements supervisés :</h2>
<ul>
    {% for equipement in liste_equipements %}
        <li>
            <strong>{{ equipement.hostname }}</strong> - {{ equipement.ip }}
            
            {% if equipement.actif %}
                <span style="color: green;">(En ligne)</span>
            {% else %}
                <span style="color: red;">(Hors ligne)</span>
            {% endif %}
        </li>
    {% endfor %}
</ul>
```

## 3. Bonne pratique : L'utilisation d'un dictionnaire de contexte

Lorsque vous avez de nombreuses variables à transmettre, la signature de `render_template()` peut devenir très longue et difficile à lire. Une pratique courante consiste à regrouper toutes les données dans un dictionnaire unique (souvent appelé `context`), puis à le "déballer" (unpacking) avec l'opérateur `**`.

```python
@app.route('/tableau-de-bord')
def dashboard():
    # Regroupement des données
    contexte = {
        "utilisateur": "admin_noc",
        "alertes_actives": 3,
        "derniere_synchro": "2026-07-03",
        "role": "Administrateur réseau"
    }
    
    # L'opérateur ** transforme le dictionnaire en arguments nommés
    # Équivaut à : render_template('dashboard.html', utilisateur="admin_noc", alertes_actives=3, ...)
    return render_template('dashboard.html', **contexte)
```

## 4. Flux de transmission des données

Le diagramme ci-dessous détaille le cycle de vie des données, depuis leur extraction dans la vue jusqu'à leur affichage dans le navigateur.

```mermaid
graph TD
    A[Requête Client] --> B(Vue Flask : @app.route)
    
    subgraph Logique Serveur (Python)
        B --> C{Récupération des données}
        C -->|Base de données / API| D[Création des variables Python<br/>ex: listes, dicts]
        D --> E[Regroupement dans un dictionnaire 'context']
    end
    
    subgraph Moteur de Template (Jinja2)
        E -->|render_template('page.html', **context)| F(Analyse du Template HTML)
        F --> G[Remplacement des {{ variables }}]
        F --> H[Exécution des {% boucles %} et {% conditions %}]
    end
    
    G --> I[Génération du code HTML final]
    H --> I
    
    I --> J[Réponse HTTP 200]
    J --> K[Affichage Navigateur]
```

---
**Sources utilisées :**
*   *Documentation officielle Flask (3.1.x) - Templates* (flask.palletsprojects.com/en/stable/tutorial/templates/)
*   *Documentation officielle Jinja2 - Template Designer Documentation* (jinja.palletsprojects.com/en/stable/templates/)