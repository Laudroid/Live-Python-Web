
# Arborescence du projet

```text
projet/
│
├── app.py
│
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── actifs.html
│   ├── detail.html
│   └── statistiques.html
│
└── static/
    └── style.css (optionnel)
```

---

# app.py

```python
from flask import Flask, render_template

app = Flask(__name__)

# --------------------------------------------------------------------
# Données simulées
# --------------------------------------------------------------------

equipements = [
    {
        "id": 1,
        "nom": "Serveur Web",
        "categorie": "Serveur",
        "actif": True
    },
    {
        "id": 2,
        "nom": "Poste RH",
        "categorie": "Poste",
        "actif": True
    },
    {
        "id": 3,
        "nom": "Routeur principal",
        "categorie": "Réseau",
        "actif": False
    },
    {
        "id": 4,
        "nom": "Switch étage 2",
        "categorie": "Réseau",
        "actif": True
    },
    {
        "id": 5,
        "nom": "Serveur Base de données",
        "categorie": "Serveur",
        "actif": False
    }
]

# --------------------------------------------------------------------
# Routes
# --------------------------------------------------------------------

@app.route("/")
def accueil():
    return render_template(
        "index.html",
        equipements=equipements
    )

@app.route("/actifs")
def actifs():

    liste = [
        e for e in equipements
        if e["actif"]
    ]

    return render_template(
        "actifs.html",
        equipements=liste
    )


@app.route("/equipement/<int:id>")

def detail(id):

    equipement = next(
        (e for e in equipements if e["id"] == id),
        None
    )

    return render_template(
        "detail.html",
        equipement=equipement
    )


@app.route("/statistiques")
def statistiques():

    nb_total = len(equipements)
    nb_actifs = len([
        e for e in equipements
        if e["actif"]
    ])

    nb_inactifs = nb_total - nb_actifs

    return render_template(
        "statistiques.html",
        total=nb_total,
        actifs=nb_actifs,
        inactifs=nb_inactifs
    )


if __name__ == "__main__":
    app.run(debug=True)
```

---

# templates/base.html

```html
<!DOCTYPE html>
<html lang="fr">

<head>
    <meta charset="UTF-8">
    <title>Gestion du parc</title>

    <style>

        body{
            font-family:Arial;
            width:900px;
            margin:auto;
        }

        nav{
            background:#eeeeee;
            padding:15px;
        }

        nav a{
            margin-right:20px;
            text-decoration:none;
        }

        table{
            border-collapse:collapse;
            width:100%;
        }

        th,td{
            border:1px solid gray;
            padding:8px;
        }

        .actif{
            color:green;
            font-weight:bold;
        }

        .inactif{
            color:red;
            font-weight:bold;
        }

    </style>
</head>
<body>

<h1>Gestion du parc informatique</h1>
<nav>

<a href="/">Accueil</a>
<a href="/actifs">Equipements actifs</a>
<a href="/statistiques">Statistiques</a>

</nav>

<hr>
{% block contenu %}
{% endblock %}

</body>
</html>
```

---

# templates/index.html

```html
{% extends "base.html" %}
{% block contenu %}

<h2>Tous les équipements</h2>

<table>
<tr>
<th>ID</th>
<th>Nom</th>
<th>Catégorie</th>
<th>Etat</th>
<th>Détail</th>
</tr>

{% for e in equipements %}

<tr>
<td>{{ e.id }}</td>
<td>{{ e.nom }}</td>
<td>{{ e.categorie }}</td>
<td>

{% if e.actif %}
<span class="actif">Actif</span>
{% else %}
<span class="inactif">Inactif</span>
{% endif %}
</td>

<td>
<a href="/equipement/{{ e.id }}">Consulter</a>
</td>
</tr>

{% endfor %}
</table>
{% endblock %}
```

---

# templates/actifs.html

```html
{% extends "base.html" %}
{% block contenu %}

<h2>Equipements actifs</h2>

{% if equipements %}
<ul>
{% for e in equipements %}
<li>
{{ e.nom }}
({{ e.categorie }})
</li>
{% endfor %}
</ul>

{% else %}
<p>Aucun équipement actif.</p>
{% endif %}
{% endblock %}
```

---

# templates/detail.html

```html
{% extends "base.html" %}
{% block contenu %}

{% if equipement %}
<h2>Détail</h2>
<ul>
<li>

<strong>ID :</strong>

{{ equipement.id }}

</li>

<li>
<strong>Nom :</strong>
{{ equipement.nom }}
</li>

<li>
<strong>Catégorie :</strong>
{{ equipement.categorie }}
</li>

<li>
<strong>Etat :</strong>
{% if equipement.actif %}
Actif
{% else %}
Inactif
{% endif %}
</li>
</ul>

{% else %}

<h2>Erreur</h2>
<p>
Cet équipement n'existe pas.
</p>
{% endif %}
{% endblock %}
```

---

# templates/statistiques.html

```html
{% extends "base.html" %}
{% block contenu %}

<h2>Statistiques</h2>

<ul>
<li>
Nombre total : {{ total }}
</li>

<li>
Equipements actifs : {{ actifs }}
</li>

<li>
Equipements inactifs : {{ inactifs }}
</li>

</ul>

{% endblock %}
```

---

# Ce que cette correction permet de réviser

Cette correction couvre la quasi-totalité des fonctionnalités de base de Jinja2 rencontrées dans une première prise en main :

| Fonctionnalité                   | Mise en œuvre                                              |
| -------------------------------- | ---------------------------------------------------------- |
| Variables                        | `{{ e.nom }}`, `{{ total }}`                               |
| Boucles                          | `{% for %}`                                                |
| Conditions                       | `{% if %}`                                                 |
| Héritage de templates            | `{% extends %}`                                            |
| Blocs                            | `{% block contenu %}`                                      |
| Navigation entre vues            | Liens entre les pages                                      |
| Passage de données Python → HTML | `render_template()`                                        |
| Route dynamique                  | `/equipement/<int:id>`                                     |
| Traitement côté Python           | Filtrage des équipements actifs et calcul des statistiques |

