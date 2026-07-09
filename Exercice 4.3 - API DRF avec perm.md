## Exercice 4.3 — API DRF avec permissions

**Compétences visées :** DRF (sérialiseur, `ModelViewSet`, routeur), **permissions**.


**Consignes :**

 **Installation de DRF :**
    ```bash
    pip install djangorestframework
    ```

1. Ajoutez `rest_framework` à `INSTALLED_APPS`.
2. Créez un `EquipementSerializer` (`ModelSerializer`) exposant `id`, `hostname`, `ip`, `actif`.
3. Créez un `EquipementViewSet` (`ModelViewSet`) enregistré via un `DefaultRouter` sous `/api/equipements/`.
4. Appliquez la permission `IsAuthenticatedOrReadOnly` : **lecture ouverte à tous**, mais **création / modification / suppression réservées aux utilisateurs authentifiés**.
5. Démontrez le comportement : lecture anonyme **OK (200)**, écriture anonyme **refusée (401/403)**, écriture authentifiée **OK (201)**.

**Livrables :** `serializers.py`, `views.py`, `urls.py`, `TESTS.md` (vos essais et les codes obtenus), `USAGE-IA.md`.

