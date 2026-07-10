## Corrigé 4 — Limiter la consommation (API4)

```python
# settings.py
REST_FRAMEWORK = {
    # ... (auth/permissions déjà configurées)
    "DEFAULT_PAGINATION_CLASS": "rest_framework.pagination.PageNumberPagination",
    "PAGE_SIZE": 50,
    "DEFAULT_THROTTLE_CLASSES": [
        "rest_framework.throttling.UserRateThrottle",
        "rest_framework.throttling.AnonRateThrottle",
    ],
    "DEFAULT_THROTTLE_RATES": {"user": "1000/day", "anon": "50/day"},
    # Pour tester : abaisser temporairement, ex. {"user": "3/min"}
}
DATA_UPLOAD_MAX_MEMORY_SIZE = 2 * 1024 * 1024   # 2 Mo
```
```bash
# Au-delà du taux configuré -> 429 Too Many Requests
for i in 1 2 3 4; do curl -s -o /dev/null -w "%{http_code}\n" \
  -H "Authorization: Token <clé>" http://127.0.0.1:8000/api/equipements/; done
```
**Points clés :** la pagination borne le volume renvoyé ; le throttling borne le débit par client (429 au dépassement) ; `DATA_UPLOAD_MAX_MEMORY_SIZE` borne la taille des corps.
*(Vérifié : avec un taux bas, la 3ᵉ requête renvoie 429.)*