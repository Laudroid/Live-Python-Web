## Exercice 3 — Propriétés sensibles / BOPLA (API3)

**Risque OWASP :** API3 Broken Object Property Level Authorization.

**Contexte :** le sérialiseur expose `fields = "__all__"`. Or `Equipement` possède un champ sensible `notes_internes`, et le champ `proprietaire` ne doit pas être imposable par le client (mass assignment).

**Consignes :**
1. Ajoutez le champ `notes_internes` (texte) au modèle s'il n'existe pas ; migrez.
2. Remplacez `fields = "__all__"` par une **liste explicite** de champs qui **n'expose pas** `notes_internes`.
3. Empêchez le **mass assignment** de `proprietaire` (`read_only_fields`) — il doit toujours être défini côté serveur.
4. Vérifiez : (a) la réponse JSON ne contient pas `notes_internes` ; (b) un `POST` qui tente de fixer `proprietaire` à un autre utilisateur est ignoré (le propriétaire reste l'appelant).

**Critères :** aucun champ sensible exposé, mass assignment neutralisé, tests probants.

---