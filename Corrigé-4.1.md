## Corrigé 4.1 — Projet Django : modèle & admin

**Commandes :**
```bash
django-admin startproject gestion_parc
cd gestion_parc
python manage.py startapp parc
# -> ajouter 'parc' à INSTALLED_APPS dans gestion_parc/settings.py
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver   # puis http://127.0.0.1:8000/admin/
```

**Modèle (`parc/models.py`) :**
```python
from django.db import models


class Equipement(models.Model):
    hostname = models.CharField(max_length=200)
    ip = models.GenericIPAddressField()
    actif = models.BooleanField(default=True)

    def __str__(self):
        return self.hostname
```

**Admin (`parc/admin.py`) :**
```python
from django.contrib import admin
from .models import Equipement

admin.site.register(Equipement)
```

**Points clés :** `GenericIPAddressField` valide le format d'adresse IP (IPv4/IPv6) — plus juste qu'un `CharField`. Après `makemigrations`/`migrate`, la table `parc_equipement` existe ; l'enregistrement dans l'admin rend le modèle gérable via `/admin/`. `__str__` améliore l'affichage dans l'admin.

