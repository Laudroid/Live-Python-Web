# Module 1 — Exercices : Python & Web (HTTP / REST, sans framework)

Fil rouge : la **gestion d'un parc d'équipements réseau** (serveurs, switchs, routeurs). Tous les exercices restent en **Python pur** (bibliothèque standard uniquement, aucun framework web).

---

## Consignes communes à tous les exercices

**Usage de l'IA — transparence obligatoire.**
L'utilisation d'une IA générative (assistant de code, chatbot, complétion automatique) **n'est pas interdite**, mais elle doit être **assumée et documentée**. Chaque rendu **doit obligatoirement contenir un fichier `USAGE-IA.md`**. Un rendu sans ce fichier est considéré comme **incomplet**.

Le fichier `USAGE-IA.md` doit répondre à ces points :
1. **Outils utilisés** : quel(s) outil(s) d'IA, et pour quelles parties de l'exercice.
2. **Aide obtenue** : ce que l'IA a proposé (idées, extraits de code, explications). Donnez au moins un exemple de prompt.
3. **Ce que vous avez accepté** : ce que vous avez repris, et surtout *pourquoi vous l'avez compris et validé*.
4. **Ce que vous avez refusé ou corrigé** : ce que vous avez écarté ou modifié, et *pourquoi* (erreur, hors-sujet, non compris, mauvaise pratique…).
5. **Ce que vous avez appris** et ce que vous feriez autrement.

> Si vous n'avez utilisé aucune IA, écrivez-le explicitement dans `USAGE-IA.md` et décrivez brièvement votre démarche.

**Règle d'or : vous devez pouvoir expliquer et justifier chaque ligne de votre rendu**, que vous l'ayez écrite vous-même ou non. Le gabarit de `USAGE-IA.md` est fourni en annexe (fin de fichier).

---

## Exercice 1.1 — Rapport d'inventaire en Python 

**Compétences visées :** variables, listes, dictionnaires, boucles, conditions, f-strings.

**Contexte :** on vous fournit l'inventaire du parc sous forme d'une liste de dictionnaires.

```python
equipements = [
    {"hostname": "srv-web-01",  "ip": "192.168.1.10", "type": "serveur", "actif": True,  "latence_ms": 12.4},
    {"hostname": "sw-access-02", "ip": "192.168.1.2",  "type": "switch",  "actif": True,  "latence_ms": 1.2},
    {"hostname": "rtr-core-01",  "ip": "10.0.0.1",     "type": "routeur", "actif": False, "latence_ms": 3.5},
    {"hostname": "sw-access-03", "ip": "192.168.1.3",  "type": "switch",  "actif": True,  "latence_ms": 1.8},
]
```

**Consignes :**
1. Calculez le **nombre total** d'équipements et le **nombre d'équipements actifs**.
2. Construisez la **liste des hostnames** dont le `type` est `"switch"`.
3. Calculez la **latence moyenne des équipements actifs**.
4. Affichez un rapport lisible (une ligne par indicateur), latence arrondie à **2 décimales**.

**Livrables :** `inventaire.py`, `USAGE-IA.md`.

**Critères d'évaluation :** exactitude des calculs, usage correct des boucles/compréhensions et des conditions, lisibilité de la sortie.

---


## Annexe — Gabarit `USAGE-IA.md`

```markdown
# Usage de l'IA — Exercice X.Y

## 1. Outils utilisés
(Ex : assistant de code / chatbot ; pour quelles parties. Ou : « aucune IA utilisée ».)

## 2. Aide obtenue
(Ce que l'IA a proposé. Exemple de prompt utilisé : « … »)

## 3. Ce que j'ai accepté
(Ce que j'ai repris et pourquoi je l'ai compris et validé.)

## 4. Ce que j'ai refusé ou corrigé
(Ce que j'ai écarté / modifié, et pourquoi.)

## 5. Ce que j'ai appris
(Ce que je retiens, ce que je ferais autrement.)
```
