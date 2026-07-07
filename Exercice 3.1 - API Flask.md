# Module 3 — Exercices : Flask & Requests

Fil rouge : une **API d'inventaire** en Flask, puis sa **consommation** avec la bibliothèque `requests`.

---

## Consignes communes à tous les exercices

**Usage de l'IA — transparence obligatoire.**
L'IA générative n'est **pas interdite**, mais son usage doit être **assumé et documenté**. Chaque rendu **doit obligatoirement contenir un fichier `USAGE-IA.md`**. Sans ce fichier, le rendu est **incomplet**.

`USAGE-IA.md` doit préciser : (1) les **outils utilisés** et pour quelles parties, (2) l'**aide obtenue** (avec un exemple de prompt), (3) ce que vous avez **accepté** et *pourquoi vous l'avez compris/validé*, (4) ce que vous avez **refusé ou corrigé** et *pourquoi*, (5) ce que vous avez **appris**.

> Aucune IA utilisée ? Déclarez-le explicitement.

**Règle d'or : vous devez pouvoir expliquer et justifier chaque ligne de votre rendu.** Gabarit en annexe.

**Pré-requis :** environnement virtuel activé, `pip install flask requests`.

---

## Exercice 3.1 — Première API Flask en lecture 

**Compétences visées :** routing Flask, réponses JSON (`jsonify`), code **404**.

**Consignes :**
1. Créez une application Flask avec une liste d'équipements en mémoire (`id`, `hostname`, `ip`).
2. `GET /api/equipements` → renvoie `{"equipements": [...]}` en JSON.
3. `GET /api/equipements/<int:equipement_id>` → renvoie l'équipement, ou `{"erreur": "..."}` avec le code **404** s'il n'existe pas.
4. Lancez le serveur et testez dans le navigateur ou avec `curl`.

**Livrables :** `app.py`, `USAGE-IA.md`.

**Critères d'évaluation :** routes correctes (statique + dynamique), sérialisation JSON, gestion du 404.


## Annexe — Gabarit `USAGE-IA.md`

```markdown
# Usage de l'IA — Exercice X.Y

## 1. Outils utilisés
(Quel(s) outil(s), pour quelles parties. Ou : « aucune IA utilisée ».)

## 2. Aide obtenue
(Ce que l'IA a proposé. Exemple de prompt : « … »)

## 3. Ce que j'ai accepté
(Ce que j'ai repris et pourquoi je l'ai compris et validé.)

## 4. Ce que j'ai refusé ou corrigé
(Ce que j'ai écarté / modifié, et pourquoi.)

## 5. Ce que j'ai appris
(Ce que je retiens, ce que je ferais autrement.)
```
