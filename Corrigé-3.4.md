# Corrigé — Exercice 3.4

## Arborescence proposée

```text
parc-informatique/
│
├── app.py
├── database.py
├── init_db.py
├── client.py
├── parc.db
│
├── templates/
│   └── index.html
│
└── requirements.txt
```

---

# Étape 1 — Préparer le projet

Création de l'environnement virtuel :

```bash
python -m venv .venv
```

Activation :

Windows

```bash
.venv\Scripts\activate
```

Linux / macOS

```bash
source .venv/bin/activate
```

Installation des bibliothèques :

```bash
pip install flask requests
```

---

# Étape 2 — Création de la base

## init_db.py

```python
import sqlite3

connexion = sqlite3.connect("parc.db")

curseur = connexion.cursor()

curseur.execute("""
CREATE TABLE IF NOT EXISTS equipements(
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nom TEXT NOT NULL,
    actif INTEGER NOT NULL DEFAULT 1
)
""")

connexion.commit()
connexion.close()

print("Base créée.")
```

Exécution :

```bash
python init_db.py
```

---

# Étape 3 — Initialisation de Flask

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def accueil():
    return "Application opérationnelle"

if __name__ == "__main__":
    app.run(debug=True)
```

---

# Étape 4 — Accès à SQLite

## database.py

```python
import sqlite3

DATABASE = "parc.db"


def get_connection():
    connexion = sqlite3.connect(DATABASE)
    connexion.row_factory = sqlite3.Row
    return connexion
```

---

# Étape 5 — Développer l'API REST

Ajout dans **app.py**

```python
from flask import Flask, jsonify, request
from database import get_connection

app = Flask(__name__)
```

### Liste des équipements

```python
@app.route("/api/equipements")
def api_liste():

    connexion = get_connection()

    equipements = connexion.execute(
        "SELECT * FROM equipements"
    ).fetchall()

    connexion.close()

    return jsonify([dict(e) for e in equipements])
```

### Ajouter un équipement

```python
@app.route("/api/equipements", methods=["POST"])
def api_ajouter():

    donnees = request.get_json()

    connexion = get_connection()

    connexion.execute(
        "INSERT INTO equipements(nom, actif) VALUES (?,1)",
        (donnees["nom"],)
    )

    connexion.commit()
    connexion.close()

    return {"message": "Créé"}, 201
```

---

# Étape 6 — Interface Web

```python
from flask import render_template
```

```python
@app.route("/")
def index():

    connexion = get_connection()

    equipements = connexion.execute(
        "SELECT * FROM equipements"
    ).fetchall()

    connexion.close()

    return render_template(
        "index.html",
        equipements=equipements
    )
```

---

# Étape 7 — Template Jinja2

## templates/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Parc informatique</title>
</head>
<body>

<h1>Liste des équipements</h1>

<table border="1">
<tr>
    <th>ID</th>
    <th>Nom</th>
    <th>État</th>
</tr>

{% for e in equipements %}
<tr>
<td>{{ e.id }}</td>
<td>{{ e.nom }}</td>
<td>
{% if e.actif %}
Actif
{% else %}
Inactif
{% endif %}
</td>
</tr>
{% endfor %}
</table>
</body>
</html>
```

---

# Étape 8 — Formulaire

Ajout dans le template

```html
<form method="POST" action="/ajouter">
<input type="text" name="nom" placeholder="Nom">
<button>Ajouter</button>
</form>
```

Dans **app.py**

```python
from flask import redirect
```

```python
@app.route("/ajouter", methods=["POST"])
def ajouter():

    nom = request.form["nom"]
    connexion = get_connection()
    connexion.execute(
        "INSERT INTO equipements(nom, actif) VALUES (?,1)",
        (nom,)
    )
    connexion.commit()
    connexion.close()

    return redirect("/")
```

---

# Étape 9 — Modifier et supprimer

## Dans le template

```html
<td>
<a href="/toggle/{{ e.id }}">Changer état</a>
</td>
<td>
<a href="/supprimer/{{ e.id }}">Supprimer</a>
</td>
```

### Changer l'état

```python
@app.route("/toggle/<int:id>")
def toggle(id):
    connexion = get_connection()
    connexion.execute("""
        UPDATE equipements
        SET actif = NOT actif
        WHERE id = ?
    """, (id,))
    connexion.commit()
    connexion.close()
    return redirect("/")
```

### Supprimer

```python
@app.route("/supprimer/<int:id>")
def supprimer(id):
    connexion = get_connection()
    connexion.execute(
        "DELETE FROM equipements WHERE id=?",
        (id,)
    )
    connexion.commit()
    connexion.close()
    return redirect("/")
```

---

# Étape 10 — Client Python

## client.py

```python
import requests
URL = "http://127.0.0.1:5000/api/equipements"
reponse = requests.get(URL)
print("Liste initiale")
print(reponse.json())
requests.post(
    URL,
    json={
        "nom": "Serveur NAS"
    }
)

print()
print("Nouvelle liste")
reponse = requests.get(URL)
print(reponse.json())
```

---

# Étape 11 — Gestion des erreurs

Version améliorée du client

```python
import requests

URL = "http://127.0.0.1:5000/api/equipements"
try:

    reponse = requests.get(
        URL,
        timeout=5
    )

    reponse.raise_for_status()
    print(reponse.json())

except requests.exceptions.Timeout:
    print("Le serveur met trop de temps à répondre.")

except requests.exceptions.ConnectionError:
    print("Impossible de joindre le serveur.")

except requests.exceptions.HTTPError as erreur:
    print("Erreur HTTP :", erreur)

except requests.exceptions.RequestException as erreur:
    print("Erreur :", erreur)
```





