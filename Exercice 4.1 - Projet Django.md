# Module 4 — Exercices : Django & Django REST Framework

Fil rouge : une application Django de **gestion de parc** (`gestion_parc` / app `parc`), puis son **API DRF** sécurisée.

---

## Consignes communes à tous les exercices

**Usage de l'IA — transparence obligatoire.**
L'IA générative n'est **pas interdite**, mais son usage doit être **assumé et documenté**. Chaque rendu **doit obligatoirement contenir un fichier `USAGE-IA.md`**. Sans ce fichier, le rendu est **incomplet**.

`USAGE-IA.md` doit préciser : (1) les **outils utilisés** et pour quelles parties, (2) l'**aide obtenue** (avec un exemple de prompt), (3) ce que vous avez **accepté** et *pourquoi vous l'avez compris/validé*, (4) ce que vous avez **refusé ou corrigé** et *pourquoi*, (5) ce que vous avez **appris**.

> Aucune IA utilisée ? Déclarez-le explicitement.

**Règle d'or : vous devez pouvoir expliquer et justifier chaque ligne de votre rendu.** Gabarit en annexe.

**Pré-requis :** environnement virtuel activé, `pip install django djangorestframework`.

---

## Exercice 4.1 — Projet Django : modèle `Equipement` et admin

**Compétences visées :** projet vs application, modèle, migrations, interface d'administration.

**Consignes :**
1. Créez un projet `gestion_parc` et une application `parc`. Déclarez `parc` dans `INSTALLED_APPS`.
2. Définissez un modèle `Equipement` avec les champs `hostname`, `ip` (utilisez `GenericIPAddressField`) et `actif` (booléen, défaut `True`), plus une méthode `__str__`.
3. Générez et appliquez les migrations (`makemigrations`, `migrate`).
4. Enregistrez le modèle dans l'admin, créez un super-utilisateur (`createsuperuser`) et ajoutez 2-3 équipements via `/admin/`.

**Livrables :** `models.py`, `admin.py`, un court `RENDU.md` décrivant ce que vous voyez dans l'admin (+ capture d'écran si possible), `USAGE-IA.md`.


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
