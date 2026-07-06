## Exercice 2.3 — Inventaire avec l'ORM SQLAlchemy *(Avancé — 45 min)*

**Compétences visées :** ORM SQLAlchemy 2.0 (`Mapped` / `mapped_column`), modèles liés (`relationship`, `ForeignKey`), `Session`.

**Consignes :**
1. Définissez deux modèles `Equipement` et `Interface` avec une relation **1-N** (`relationship` + `ForeignKey` + `back_populates`).
2. Créez le moteur SQLite et générez les tables (`create_all`).
3. Dans une `Session`, créez **un équipement avec ses deux interfaces en une seule fois**, puis `commit`.
4. Récupérez les équipements avec `select()` et affichez, pour chacun, la liste de ses interfaces.

**Livrables :** `inventaire_orm.py`, `USAGE-IA.md`.

**Critères d'évaluation :** modèles + relation corrects, gestion de la session/commit, requête `select()`, affichage des objets liés.

---
