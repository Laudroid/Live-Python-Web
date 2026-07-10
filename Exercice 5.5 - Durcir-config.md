## Exercice 5 — Durcir la configuration (API8)

**Risque OWASP :** API8 Security Misconfiguration (+ A09 journalisation).

**Contexte :** le `settings.py` est en configuration de développement (peu sûr pour la production).

**Consignes :**
1. Sortez la `SECRET_KEY` du code : lisez-la depuis une **variable d'environnement** ; faites de même pour `DEBUG`.
2. Passez `DEBUG = False` et renseignez `ALLOWED_HOSTS` explicitement.
3. Activez les **en-têtes de sécurité** (`SECURE_CONTENT_TYPE_NOSNIFF`, `SECURE_REFERRER_POLICY`, `SECURE_SSL_REDIRECT`, `SECURE_HSTS_SECONDS`, cookies `Secure`).
4. Restreignez **CORS** (pas de `CORS_ALLOW_ALL_ORIGINS = True`) et **désactivez l'API navigable** en production (rendu JSON uniquement).
5. Ajoutez une **journalisation** des échecs d'authentification/autorisation (logger dédié), sans journaliser de secret.
6. Lancez `python manage.py check --deploy` et corrigez les avertissements.

**Critères :** secrets hors du code, `DEBUG=False`, en-têtes présents, CORS restreint, `check --deploy` propre, journalisation en place.