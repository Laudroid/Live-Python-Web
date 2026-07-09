## Corrigé 4.2 — Lister les équipements actifs

**Vue (`parc/views.py`) :**
```python
from django.shortcuts import render
from .models import Equipement


def liste_equipements_actifs(request):
    equipements = Equipement.objects.filter(actif=True)
    return render(request, 'parc/liste.html', {'equipements': equipements})
```

**URLs (`parc/urls.py`) :**
```python
from django.urls import path
from . import views

urlpatterns = [
    path('equipements/', views.liste_equipements_actifs, name='liste_equipements_actifs'),
]
```
```python
# gestion_parc/urls.py — ne pas oublier d'inclure les URLs de l'app :
from django.urls import path, include

urlpatterns = [
    path('', include('parc.urls')),
]
```

**Template (`parc/templates/parc/liste.html`) :**
```html
<!DOCTYPE html>
<html lang="fr">
<head><meta charset="UTF-8"><title>Équipements actifs</title></head>
<body>
    <h1>Équipements actifs</h1>
    <ul>
        {% for e in equipements %}
            <li>{{ e.hostname }} — {{ e.ip }}</li>
        {% empty %}
            <li>Aucun équipement actif.</li>
        {% endfor %}
    </ul>
</body>
</html>
```

**Bonus — version CBV :**
```python
from django.views.generic import ListView
from .models import Equipement


class EquipementsActifsView(ListView):
    model = Equipement
    template_name = 'parc/liste.html'
    context_object_name = 'equipements'

    def get_queryset(self):
        return Equipement.objects.filter(actif=True)
# urls.py : path('equipements/', EquipementsActifsView.as_view(), name='liste_equipements_actifs')
```

**Points clés :** `filter(actif=True)` renvoie un `QuerySet` (évaluation paresseuse) ; la balise `{% empty %}` gère le cas « liste vide » ; en CBV, on surcharge `get_queryset()` pour filtrer.