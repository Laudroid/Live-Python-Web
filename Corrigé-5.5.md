## Corrigé 5 — Durcir la configuration (API8 + A09)

```python
# settings.py (production)
import os
SECRET_KEY = os.environ["DJANGO_SECRET_KEY"]
DEBUG = os.environ.get("DJANGO_DEBUG", "false").lower() == "true"   # False par défaut
ALLOWED_HOSTS = ["api.monreseau.example"]

# En-têtes / transport
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_REFERRER_POLICY = "same-origin"
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True

# CORS restreint (django-cors-headers) — NE PAS mettre CORS_ALLOW_ALL_ORIGINS = True
CORS_ALLOWED_ORIGINS = ["https://console.monreseau.example"]

# API navigable désactivée en production (rendu JSON uniquement)
REST_FRAMEWORK = {"DEFAULT_RENDERER_CLASSES": ["rest_framework.renderers.JSONRenderer"]}

# Journalisation des échecs de sécurité (sans secrets)
LOGGING = {
    "version": 1, "disable_existing_loggers": False,
    "handlers": {"secu": {"class": "logging.StreamHandler"}},
    "loggers": {"django.security": {"handlers": ["secu"], "level": "WARNING"}},
}
```
**Points clés :** secrets hors du code (variables d'environnement, `fail-fast` si absent) ; `DEBUG=False` empêche la fuite de stack traces et de settings ; en-têtes ajoutés par `SecurityMiddleware` ; CORS restreint aux origines de confiance ; `manage.py check --deploy` doit être propre.
*(Vérifié : `SecurityMiddleware` ajoute `X-Content-Type-Options: nosniff` et `Referrer-Policy: same-origin`.)*