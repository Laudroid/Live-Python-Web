## Corrigé 2 — BOLA : cloisonnement par propriétaire (API1)

```python
# parc/models.py  (ajout)
proprietaire = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE,
                                 related_name="equipements")
```
```python
# parc/views.py
from rest_framework.permissions import IsAuthenticated, BasePermission

class IsProprietaire(BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.proprietaire_id == request.user.id

class EquipementViewSet(viewsets.ModelViewSet):
    serializer_class = EquipementSerializer
    permission_classes = [IsAuthenticated, IsProprietaire]

    def get_queryset(self):
        return Equipement.objects.filter(proprietaire=self.request.user)

    def perform_create(self, serializer):
        serializer.save(proprietaire=self.request.user)
```
**Points clés :** deux couches complémentaires — `get_queryset()` **cloisonne** la collection (l'objet d'un autre n'apparaît même pas → 404), et `has_object_permission` protège l'accès direct. Ne jamais se fier à l'ID fourni.
*(Vérifié : Bob → 404 sur l'équipement d'Alice ; Alice → 200.)*