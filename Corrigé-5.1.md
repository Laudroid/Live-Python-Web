## Corrigé 1 — Exiger l'authentification (API2)

```python
# settings.py
INSTALLED_APPS += ["rest_framework.authtoken"]     # puis : python manage.py migrate

REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework.authentication.TokenAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.IsAuthenticated",
    ],
}
```
```python
# Générer un jeton (shell : python manage.py shell)
from django.contrib.auth.models import User
from rest_framework.authtoken.models import Token
u = User.objects.create_user("alice", password="MotDePasse-12chars")
token, _ = Token.objects.get_or_create(user=u)
print(token.key)
```
```bash
# Sans jeton -> 401/403 ; avec jeton -> 200
curl -i http://127.0.0.1:8000/api/equipements/
curl -i -H "Authorization: Token <clé>" http://127.0.0.1:8000/api/equipements/
```
**Points clés :** l'authentification est refusée par défaut grâce à `IsAuthenticated` global ; le jeton s'envoie dans l'en-tête `Authorization: Token ...`. En production, préférer **JWT** (durée de vie courte + refresh) et **HTTPS** obligatoire.