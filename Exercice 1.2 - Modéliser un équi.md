## Exercice 1.2 — Modéliser un équipement (POO) & calculer un sous-réseau 

**Compétences visées :** fonctions, POO (classe, `__init__`, `self`, méthode), fonction `lambda`, tri.

**Consignes :**
1. Écrivez une classe `Equipement` avec :
   - un constructeur `__init__(self, hostname, ip, latence_ms, actif=True)` ;
   - une méthode `__str__` renvoyant une description lisible ;
   - une méthode `est_operationnel(self, seuil_ms=100)` qui renvoie `True` si l'équipement est **actif ET** que sa latence est **sous le seuil**.
2. Écrivez une fonction `hotes_disponibles(bits_hote)` qui renvoie le nombre d'adresses **hôtes utilisables** d'un sous-réseau (indice : `2 ** bits - 2`).
3. Écrivez une fonction `trier_par_latence(equipements)` qui renvoie la liste triée par latence croissante (utilisez `sorted` + `lambda`).
4. Créez 3 équipements, affichez-les **triés par latence** avec leur état opérationnel, puis affichez le nombre d'hôtes d'un `/24` (soit 8 bits d'hôte).

**Livrables :** `equipement.py`, `USAGE-IA.md`.

**Critères d'évaluation :** POO correcte (`self`, méthodes), `lambda` pour le tri, exactitude du calcul réseau.

