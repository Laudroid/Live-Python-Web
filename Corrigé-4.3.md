## Corrigé 4.3 — API DRF avec permissions

**Sérialiseur (`parc/serializers.py`) :**
```python
from rest_framework import serializers
from .models import Equipement


class EquipementSerializer(serializers.ModelSerializer):
    class Meta:
        model = Equipement
        fields = ['id', 'hostname', 'ip', 'actif']
```

**ViewSet (`parc/views.py`) :**
```python
from rest_framework import viewsets
from rest_framework.permissions import IsAuthenticatedOrReadOnly
from .models import Equipement
from .serializers import EquipementSerializer


class EquipementViewSet(viewsets.ModelViewSet):
    queryset = Equipement.objects.all()
    serializer_class = EquipementSerializer
    permission_classes = [IsAuthenticatedOrReadOnly]
```

**Routeur (`parc/urls.py`) :**
```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import EquipementViewSet

router = DefaultRouter()
router.register(r'equipements', EquipementViewSet, basename='equipement')

urlpatterns = [
    path('api/', include(router.urls)),
]
```

**Tests (`TESTS.md`) :**
```bash
# Lecture anonyme -> 200
curl http://127.0.0.1:8000/api/equipements/

# Écriture anonyme -> 403 (Authentication credentials were not provided)
curl -X POST http://127.0.0.1:8000/api/equipements/ \
     -H "Content-Type: application/json" -d '{"hostname":"srv-dns-01","ip":"192.168.1.20"}'
```
Pour l'**écriture authentifiée** (→ **201**), le plus simple est d'utiliser l'**API navigable** de DRF : ouvrir `http://127.0.0.1:8000/api/equipements/` dans le navigateur, se connecter (lien « Log in », après avoir ajouté `path('api-auth/', include('rest_framework.urls'))`), puis soumettre le formulaire de création. La session authentifiée autorise alors le `POST`.

**Comportement attendu :**
| Requête | Utilisateur | Code |
| :--- | :--- | :--- |
| `GET /api/equipements/` | anonyme | 200 |
| `POST /api/equipements/` | anonyme | 403 |
| `POST /api/equipements/` | authentifié | 201 |

**Points clés :** `ModelViewSet` + `DefaultRouter` fournissent tout le CRUD sans écrire chaque méthode ; `IsAuthenticatedOrReadOnly` autorise les méthodes « sûres » (GET/HEAD/OPTIONS) à tous et exige l'authentification pour POST/PUT/PATCH/DELETE.