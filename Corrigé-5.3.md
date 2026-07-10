## Corrigé 3 — BOPLA : propriétés sensibles (API3)

```python
# parc/serializers.py
class EquipementSerializer(serializers.ModelSerializer):
    class Meta:
        model = Equipement
        fields = ["id", "hostname", "ip", "actif", "proprietaire"]  # 'notes_internes' NON exposé
        read_only_fields = ["proprietaire"]                          # anti mass-assignment
```
**Points clés :** on n'utilise **jamais** `fields = "__all__"` sur un modèle à champs sensibles (exposition excessive). `read_only_fields` neutralise le mass assignment : même si le client envoie `proprietaire`, la valeur est ignorée (le propriétaire est fixé côté serveur par `perform_create`).
*(Vérifié : `notes_internes` absent de la réponse ; `proprietaire` envoyé par le client ignoré.)*