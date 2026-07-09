## Exercice 4.2 — Lister les équipements actifs (vue + template)

**Compétences visées :** vue, URL, template, requête ORM avec **filtre**.

**Consignes :**
1. Écrivez une vue `liste_equipements_actifs` qui récupère `Equipement.objects.filter(actif=True)`.
2. Associez-la à l'URL `/equipements/`.
3. Créez un template affichant les équipements actifs dans une liste, avec un message si la liste est vide (balise `{% empty %}`).
4. **Bonus :** réécrivez la même logique en vue basée sur une classe (`ListView` + `get_queryset`).

**Livrables :** `views.py`, `urls.py`, `templates/parc/liste.html`, `USAGE-IA.md`.
