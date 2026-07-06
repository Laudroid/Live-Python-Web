## Exercice 2.2 — Relier équipements et interfaces *(Intermédiaire — 35 min)*

**Compétences visées :** clé étrangère, relation **un-à-plusieurs**, jointure `JOIN`.

**Contexte :** un équipement possède plusieurs interfaces ; une interface appartient à un seul équipement.

**Consignes :**
1. Créez deux tables : `equipement(id, hostname)` et `interface(id, nom, equipement_id)`, où `equipement_id` est une **clé étrangère** vers `equipement(id)`.
2. Insérez **un** équipement (ex : `sw-access-02`) et **deux** interfaces rattachées (ex : `GigabitEthernet0/1`, `GigabitEthernet0/2`).
3. Écrivez une requête avec **`JOIN`** listant, pour chaque interface, le `hostname` de l'équipement et le `nom` de l'interface.
4. **Bonus :** comptez le nombre d'interfaces par équipement (`GROUP BY`).

**Livrables :** `relations_sqlite.py`, `USAGE-IA.md`.

**Critères d'évaluation :** clé étrangère correcte, jointure juste, compréhension de la relation 1-N.
