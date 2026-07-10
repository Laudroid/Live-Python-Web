# Examen final — Programmation web en Python (Modules 1 à 5)

**Nom / Prénom :** ……………………………………………  **Groupe :** …………

## Consignes
- Épreuve **individuelle**, **notes de cours autorisées**.
- **45 questions fermées (QCM)** : une **seule** réponse correcte par question — entourez la lettre.
- **13 questions ouvertes** : répondez dans l'espace prévu ; pour les analyses de code, soyez précis (ligne, cause, correction).
- Contexte de toutes les questions : la **gestion d'un parc d'équipements réseau** (serveurs, switchs, routeurs, interfaces).

---


**Q1.** Que vaut `len(set([22, 80, 443, 22, 80]))` ?
- a) 5
- b) 3
- c) 2
- d) 4

**Q2.** Parmi ces méthodes HTTP, laquelle **n'est pas** idempotente ?
- a) GET
- b) PUT
- c) POST
- d) DELETE

**Q3.** Quel code de statut HTTP indique qu'une ressource a été **créée** ?
- a) 200
- b) 201
- c) 301
- d) 404

**Question ouverte O1 *(2 pts)*.** Expliquez en deux phrases la différence entre **Internet** et le **Web**.

> Réponse : ………………………………………………………………………………………………………………………

**Q4.** *(Analyse de code)* Que produit l'exécution de cet extrait ?
```python
adresse = ("10.0.0.1", 22)
adresse[1] = 443
```
- a) Affiche `('10.0.0.1', 443)`
- b) Lève une `TypeError`
- c) Affiche `('10.0.0.1', 22)`
- d) Lève une `IndexError`

**Q5.** Quelle structure Python **n'autorise pas les doublons** et **n'est pas ordonnée** ?
- a) `list`
- b) `tuple`
- c) `set`
- d) `dict`

**Q6.** Dans une API REST, quelle URL respecte les bonnes pratiques ?
- a) `/getEquipements`
- b) `/equipements`
- c) `/equipement/delete/5`
- d) `/list`

**Q7.** *(Analyse de code)* Qu'affiche `print(2 ** 6 - 2)` (nombre d'hôtes d'un /26) ?
- a) 64
- b) 62
- c) 126
- d) 30

**Question ouverte O2 *(4 pts)*.** Écrivez une fonction Python `hotes_disponibles(bits_hote)` qui renvoie le nombre d'adresses **hôtes utilisables** d'un sous-réseau, puis donnez le résultat pour un **/24**.

> Réponse :
> ```python
>
> ```

**Q8.** Que renvoie une fonction Python qui ne contient **aucun** `return` ?
- a) `0`
- b) `""`
- c) `None`
- d) `False`

**Q9.** À quoi sert l'en-tête HTTP `Content-Type: application/json` ?
- a) À authentifier le client
- b) À indiquer le format du corps du message
- c) À définir la méthode HTTP
- d) À fixer le code de statut

**Q10.** Quel format de données est le standard d'échange des API REST modernes ?
- a) XML
- b) CSV
- c) JSON
- d) YAML

**Question ouverte O3 *(3 pts)*.** Qu'est-ce que l'**idempotence** d'une méthode HTTP ? Donnez une méthode idempotente et une non idempotente, en justifiant.

> Réponse : ………………………………………………………………………………………………………………………

---


**Q11.** À quoi sert une **clé primaire** ?
- a) À relier deux tables
- b) À identifier de façon unique chaque ligne
- c) À trier les résultats
- d) À chiffrer les données

**Q12.** Une **clé étrangère** permet de…
- a) accélérer les requêtes
- b) créer une relation vers la clé primaire d'une autre table
- c) empêcher les doublons
- d) définir le type d'une colonne

**Q13.** *(Analyse de code)* Qu'affiche cet extrait ?
```python
resultats = [("srv-web-01", "192.168.1.10"), ("sw-access-02", "192.168.1.2")]
print(resultats[1][0])
```
- a) `srv-web-01`
- b) `192.168.1.10`
- c) `sw-access-02`
- d) `192.168.1.2`

**Question ouverte O4 *(3 pts)*.** Expliquez ce qu'est une **clé étrangère** et à quoi elle sert, à l'aide d'un exemple `equipement` / `interface`.

> Réponse : ………………………………………………………………………………………………………………………

**Q14.** En SQL, quelle commande permet de **lire** des données ?
- a) `INSERT`
- b) `SELECT`
- c) `UPDATE`
- d) `DELETE`

**Q15.** Pourquoi utiliser des `?` (requête paramétrée) avec `sqlite3` ?
- a) C'est plus rapide
- b) Pour éviter les injections SQL
- c) C'est obligatoire
- d) Pour trier les résultats

**Q16.** « Un équipement possède plusieurs interfaces » : de quel type de relation s'agit-il ?
- a) Un-à-un (1-1)
- b) Un-à-plusieurs (1-N)
- c) Plusieurs-à-plusieurs (N-N)
- d) Aucune relation

**Q17.** Dans SQLAlchemy 2.0, quel objet gère les opérations `add()` / `commit()` ?
- a) `Engine`
- b) `Session`
- c) `Model`
- d) `Cursor`

**Question ouverte O5 *(4 pts — analyse de code)*.** Identifiez la **faille de sécurité** de cet extrait et **corrigez-la**.
```python
hostname = input("hostname ? ")
cur.execute(f"SELECT * FROM equipement WHERE hostname = '{hostname}'")
```
> Réponse : ………………………………………………………………………………………………………………………

**Q18.** Que fait `cursor.fetchall()` ?
- a) Exécute la requête
- b) Valide la transaction
- c) Renvoie toutes les lignes du résultat
- d) Ferme la connexion

**Q19.** Une relation **N-N** (plusieurs-à-plusieurs) nécessite…
- a) uniquement une clé primaire composite
- b) une table de liaison (d'association)
- c) un index
- d) rien de particulier

**Q20.** Quel est un **avantage** de l'ORM par rapport au SQL brut ?
- a) Il est toujours plus rapide
- b) Il manipule des objets Python et facilite le changement de SGBD
- c) Il supprime le besoin de base de données
- d) Il empêche toute erreur

**Question ouverte O6 *(3 pts — analyse de code)*.** Après ce code, la ligne n'apparaît pas dans la base. **Pourquoi** et **comment corriger** ?
```python
conn = sqlite3.connect("inventaire.db")
cur = conn.cursor()
cur.execute("INSERT INTO equipement (hostname, ip, type) VALUES (?, ?, ?)",
            ("srv-dns-01", "192.168.1.20", "serveur"))
conn.close()
```
> Réponse : ………………………………………………………………………………………………………………………

---


**Q21.** Flask est un…
- a) framework full-stack
- b) micro-framework
- c) ORM
- d) serveur web de production

**Q22.** *(Analyse de code)* Que renvoie cette vue Flask ?
```python
@app.route("/api/statut")
def statut():
    return {"etat": "ok"}
```
- a) Une erreur 500
- b) Du texte brut
- c) Une réponse **JSON** avec le code **200**
- d) Une réponse 204 sans contenu

**Q23.** Quel décorateur associe une URL à une fonction en Flask ?
- a) `@app.get_route`
- b) `@app.route`
- c) `@route`
- d) `@flask.url`

**Question ouverte O7 *(3 pts)*.** À quoi sert la bibliothèque `requests` ? Donnez un exemple d'appel **GET** avec un **timeout**.

> Réponse :
> ```python
>
> ```

**Q24.** Comment lit-on le corps **JSON** d'une requête entrante en Flask ?
- a) `request.body`
- b) `request.get_json()`
- c) `request.json_body`
- d) `flask.read()`

**Q25.** Avec `requests`, comment envoyer un corps **JSON** dans un POST ?
- a) `data=...`
- b) `body=...`
- c) `json=...`
- d) `payload=...`

**Q26.** Que fait `response.raise_for_status()` ?
- a) Fixe le code de statut
- b) Lève une exception si le code est ≥ 400
- c) Convertit la réponse en JSON
- d) Ferme la connexion

**Q27.** En Jinja2, quelle syntaxe **affiche** une variable ?
- a) `{% variable %}`
- b) `{{ variable }}`
- c) `${variable}`
- d) `<%= variable %>`

**Question ouverte O8 *(4 pts — analyse de code)*.** Un client envoie `POST /api/equipements` et reçoit `405 Method Not Allowed`. Voici la route. **Pourquoi** ? **Corrigez-la.**
```python
@app.route("/api/equipements")
def creer_equipement():
    donnees = request.get_json()
    ...
```
> Réponse : ………………………………………………………………………………………………………………………

**Q28.** En Jinja2, quelle syntaxe permet d'écrire une **boucle** ?
- a) `{{ for e in liste }}`
- b) `{% for e in liste %} … {% endfor %}`
- c) `<for e in liste>`
- d) `@for(e in liste)`

**Q29.** Quel moteur de templates Flask utilise-t-il par défaut ?
- a) Mako
- b) Jinja2
- c) Django Templates
- d) Handlebars

**Q30.** Comment renvoyer un JSON **avec le code 201** en Flask ?
- a) `return jsonify(x)`
- b) `return jsonify(x), 201`
- c) `return 201, x`
- d) `return status(201)`

**Question ouverte O9 *(3 pts)*.** Écrivez une route Flask `GET /api/equipements/<int:equipement_id>` qui renvoie l'équipement en JSON, ou une erreur **404** s'il n'existe pas (liste `equipements` en mémoire). Expliquez brièvement votre code.

> Réponse :
> ```python
>
> ```

---


**Q31.** Django est un framework…
- a) micro
- b) full-stack (« batteries included »)
- c) uniquement pour les API
- d) uniquement frontend

**Q32.** Que signifie l'architecture **MTV** de Django ?
- a) Model-Template-View
- b) Model-View-Template
- c) Method-Type-Value
- d) Module-Test-Vue

**Q33.** Quelle commande **génère** les fichiers de migration ?
- a) `migrate`
- b) `makemigrations`
- c) `startapp`
- d) `runserver`

**Question ouverte O10 *(3 pts)*.** Qu'est-ce qu'une **migration** Django et pourquoi en a-t-on besoin ? (une analogie est acceptée)

> Réponse : ………………………………………………………………………………………………………………………

**Q34.** Quelle commande **applique** les migrations à la base de données ?
- a) `makemigrations`
- b) `migrate`
- c) `syncdb`
- d) `commit`

**Q35.** *(Analyse de code)* L'équipement 999 n'existe pas. Que se passe-t-il ?
```python
Equipement.objects.get(id=999)
```
- a) Renvoie `None`
- b) Renvoie `[]`
- c) Lève `Equipement.DoesNotExist`
- d) Renvoie une erreur 404

**Q36.** Dans DRF, le rôle d'un **sérialiseur** est de…
- a) router les URLs
- b) convertir les objets ↔ JSON et valider les données
- c) gérer les sessions
- d) créer la base de données

**Q37.** Quelle classe DRF fournit **automatiquement** tout le CRUD ?
- a) `APIView`
- b) `ModelViewSet`
- c) `Serializer`
- d) `Router`

**Question ouverte O11 *(4 pts — analyse de code)*.** Cette vue déclenche l'erreur « *didn't return an HttpResponse object* ». **Pourquoi** ? **Corrigez-la.**
```python
def liste_equipements(request):
    return Equipement.objects.filter(actif=True)
```
> Réponse : ………………………………………………………………………………………………………………………

**Q38.** À quoi sert un `DefaultRouter` en DRF ?
- a) À valider les données
- b) À générer automatiquement les URLs d'un ViewSet
- c) À sérialiser les objets
- d) À authentifier les utilisateurs

**Q39.** La permission `IsAuthenticatedOrReadOnly` autorise…
- a) tout à tout le monde
- b) la lecture à tous, l'écriture aux utilisateurs authentifiés
- c) rien aux anonymes
- d) l'écriture à tous

**Q40.** Quel attribut booléen du modèle `User` donne accès à l'interface `/admin/` ?
- a) `is_active`
- b) `is_staff`
- c) `is_superuser`
- d) `is_admin`

**Question ouverte O12 *(4 pts)*.** Écrivez un `EquipementSerializer` (DRF `ModelSerializer`) exposant `id`, `hostname`, `ip`, `actif`, puis expliquez en une phrase la différence entre `APIView` et `ModelViewSet`.

> Réponse :
> ```python
>
> ```

**Q41.** Un utilisateur authentifié accède aux équipements d'un autre en modifiant simplement l'identifiant dans l'URL (/api/equipements/42/). De quel risque OWASP s'agit-il ?
- a) API8 — Security Misconfiguration
- b) API1 — Broken Object Level Authorization (BOLA)
- c) API7 — Server Side Request Forgery
- d) API4 — Unrestricted Resource Consumption

**Q42.** Dans un ViewSet DRF, quelle technique garantit que chaque utilisateur ne voie que ses ressources ?
- a) Mettre fields = "__all__" dans le sérialiseur
- b) Surcharger get_queryset() en filtrant sur request.user
- c) Définir permission_classes = [AllowAny]
- d) Activer la pagination

**Q43.** Pourquoi éviter fields = "__all__" dans un ModelSerializer exposé par une API ?
- a) Cela ralentit les requêtes
- b) Cela désactive l'authentification
- c) Cela peut exposer des champs sensibles et autoriser le mass assignment (API3 BOPLA)
- d) Cela empêche la pagination

**Q44.** Une API récupère une URL fournie par l'utilisateur (webhook). Quelle est la meilleure protection contre la SSRF (API7) ?
- a) Augmenter le timeout de la requête
- b) Tenir une liste noire de chaînes comme "127.0.0.1"
- c) Valider le schéma puis bloquer les IP privées/loopback/link-local (ou utiliser une allowlist)
- d) Renvoyer le corps de la réponse distante au client

**Q45.** Quel code de statut HTTP DRF renvoie-t-il lorsqu'un client dépasse la limite de débit (throttling, API4) ?
- a) 401 Unauthorized
- b) 403 Forbidden
- c) 429 Too Many Requests
- d) 500 Internal Server Error

**Question ouverte O13** - Trouver et corriger la faille (analyse de code)
Le développeur veut renvoyer « mes équipements ». Voici sa vue DRF. Identifiez la faille, nommez le risque OWASP, expliquez l'impact, puis corrigez le code.

# parc/views.py
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Equipement
from .serializers import EquipementSerializer

@api_view(["GET"])
def mes_equipements(request):
    user_id = request.query_params.get("user_id")           # ex. /api/mes-equipements/?user_id=1
    equipements = Equipement.objects.filter(proprietaire_id=user_id)
    return Response(EquipementSerializer(equipements, many=True).data)

> Réponse : ………………………………………………………………………………………………………………………



FIN DU DOCUMENT