# Module 2 — Exercices : Bases de données relationnelles

Fil rouge : la **base d'inventaire du parc** (équipements et leurs interfaces). On passe du SQL brut (`sqlite3`) à l'ORM (SQLAlchemy 2.0).

---

## Consignes communes à tous les exercices

**Usage de l'IA — transparence obligatoire.**
L'utilisation d'une IA générative n'est **pas interdite**, mais elle doit être **assumée et documentée**. Chaque rendu **doit obligatoirement contenir un fichier `USAGE-IA.md`**. Un rendu sans ce fichier est **incomplet**.

`USAGE-IA.md` doit préciser :
1. **Outils utilisés** (et pour quelles parties).
2. **Aide obtenue** (avec au moins un exemple de prompt).
3. **Ce que vous avez accepté** (et *pourquoi* vous l'avez compris et validé).
4. **Ce que vous avez refusé ou corrigé** (et *pourquoi*).
5. **Ce que vous avez appris**.

> Aucune IA utilisée ? Déclarez-le explicitement dans `USAGE-IA.md`.

**Règle d'or : vous devez pouvoir expliquer et justifier chaque ligne de votre rendu.** Gabarit de `USAGE-IA.md` en annexe.

---

## Exercice 2.1 — Créer et interroger une base d'inventaire *(Découverte — 25 min)*

**Compétences visées :** SQL de base, `sqlite3`, requêtes **paramétrées** (anti-injection).

**Consignes :**
1. Avec le module `sqlite3`, créez une base `inventaire.db` et une table `equipement(id, hostname, ip, type)` (l'`id` est une clé primaire auto-incrémentée).
2. Insérez au moins **3 équipements** à l'aide d'une requête **paramétrée** (placeholders `?`).
3. Écrivez une requête qui récupère **uniquement les équipements de type `switch`**, puis affichez-les.
4. En commentaire, expliquez **pourquoi** on utilise les `?` plutôt qu'un formatage de chaîne (`f"...{valeur}..."`).

**Livrables :** `inventaire_sqlite.py`, `USAGE-IA.md`.

**Critères d'évaluation :** schéma correct, insertion paramétrée, filtrage `WHERE`, explication anti-injection.


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
