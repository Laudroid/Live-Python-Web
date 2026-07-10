## Exercice 2 — Cloisonner l'accès aux objets / BOLA (API1)

**Risque OWASP :** API1 Broken Object Level Authorization.

**Contexte :** tout utilisateur authentifié peut lire/modifier **n'importe quel** équipement via son ID.

**Consignes :**
1. Ajoutez au modèle `Equipement` un champ `proprietaire` (clé étrangère vers `User`) ;

    proprietaire = models.ForeignKey(settings.AUTH_USER_MODEL,
                                     on_delete=models.CASCADE,
                                     related_name="equipements")


    migrez.
    
2. Dans l'`EquipementViewSet`, surchargez `get_queryset()` pour ne renvoyer que les équipements du `request.user`.
3. Faites en sorte que la création affecte automatiquement le propriétaire (`perform_create`).
4. Ajoutez une permission au niveau **objet** (`has_object_permission`) qui vérifie l'appartenance.
5. Vérifiez : l'utilisateur A ne peut pas accéder (404/403) à l'équipement de l'utilisateur B.

**Critères :** cloisonnement effectif par propriétaire (queryset + permission objet), démonstration A/B.