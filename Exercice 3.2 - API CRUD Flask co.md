## Exercice 3.2 — API CRUD Flask complète 

**Compétences visées :** `POST` / `PUT` / `DELETE`, lecture du corps JSON (`request.get_json`), **validation**, codes **201 / 400 / 404**.

**Consignes :** repartez de l'API du 3.1 et ajoutez :
1. `POST /api/equipements` : crée un équipement depuis le JSON reçu. `hostname` est **obligatoire** (sinon **400**). Renvoie **201** et l'objet créé (avec un `id` auto-incrémenté).
2. `PUT /api/equipements/<id>` : met à jour les champs fournis ; **404** si l'équipement n'existe pas.
3. `DELETE /api/equipements/<id>` : supprime l'équipement ; **404** si absent.
4. Vérifiez chaque opération avec `curl`.

**Livrables :** `app.py`, `TESTS.md` (vos commandes `curl` et les réponses), `USAGE-IA.md`.

**Critères d'évaluation :** CRUD complet et cohérent, validation des entrées, bons codes HTTP.

---
