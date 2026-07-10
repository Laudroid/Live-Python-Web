## Exercice 4 — Limiter la consommation (API4) *(Intermédiaire — 30 min)*

**Risque OWASP :** API4 Unrestricted Resource Consumption (et prévention API6).

**Contexte :** l'API renvoie toute la table d'un coup et n'impose aucune limite de débit.

**Consignes :**
1. Activez la **pagination** (`PageNumberPagination`, `PAGE_SIZE` raisonnable).
2. Activez la **limitation de débit** (`UserRateThrottle` / `AnonRateThrottle`) avec des taux dans `DEFAULT_THROTTLE_RATES`. Pour tester facilement, fixez temporairement un taux bas (ex. `3/min`).
3. Limitez la taille des corps de requête (`DATA_UPLOAD_MAX_MEMORY_SIZE`).
4. Vérifiez : après dépassement du taux, l'API renvoie **429 Too Many Requests** ; la liste est paginée.

**Critères :** pagination effective, throttling déclenchant un 429, limite de taille en place.