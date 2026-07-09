# Module 5 — Sécurité des API REST en Python (OWASP)

## 1. Pourquoi les API REST se sécurisent différemment

Une API REST n'est pas « un site web sans les pages ». Son modèle de menace diffère :

| Application web classique | API REST |
| :--- | :--- |
| Client = navigateur (humain) | Client = autre service, mobile, script (souvent automatisé) |
| Session par cookie | Authentification par **jeton** (Token, JWT) |
| Le serveur rend le HTML (contrôle l'affichage) | Le client reçoit des **données brutes** (JSON) et décide de l'affichage |
| Surface : quelques pages | Surface : **des centaines d'endpoints**, chacun manipulant des identifiants d'objets |

Conséquences directes :
- On ne peut **jamais** se reposer sur l'interface (le client) pour la sécurité : tout contrôle se fait **côté serveur**.
- Le filtrage « à l'affichage » ne protège rien : si le sérialiseur renvoie un champ sensible, il est exposé, même si aucune page ne l'affiche.
- L'**autorisation au niveau de chaque objet** devient le risque n°1 (un identifiant dans l'URL = une tentation d'accès non autorisé).

### Les deux référentiels OWASP utilisés

- **OWASP API Security Top 10 — 2023** : le référentiel **dédié aux API** (édition la plus récente). C'est notre fil conducteur.
- **OWASP Top 10:2025** : le référentiel généraliste (applications web). On y fait référence pour situer chaque risque dans le cadre plus large.

> Rappel de vocabulaire : **authentification** = « qui es-tu ? » (prouver l'identité). **Autorisation** = « as-tu le droit de faire ceci sur cette ressource ? ». La majorité des failles d'API sont des failles d'**autorisation**.

### Principes directeurs (defense in depth)
- **Refus par défaut** : tout est interdit sauf ce qui est explicitement autorisé.
- **Moindre privilège** : chaque acteur n'a que les droits strictement nécessaires.
- **Ne jamais faire confiance à l'entrée** — ni à celle de l'utilisateur, ni à celle d'une API tierce.
- **Défense en profondeur** : plusieurs couches (TLS, authn, authz, validation, quotas, journalisation).

---

## 2. Fondamentaux transverses

### 2.1 HTTPS / TLS
Une API qui transporte des jetons d'authentification **doit** être servie en HTTPS. En clair (HTTP), un jeton intercepté sur le réseau = compte compromis. En production : redirection forcée vers HTTPS et **HSTS**.

### 2.2 Gestion des secrets
La `SECRET_KEY` Django, les mots de passe de base de données, les clés d'API tierces **ne doivent jamais** être écrits en dur dans le code ni versionnés. On les lit depuis des **variables d'environnement** (ou un gestionnaire de secrets).

```python
# settings.py — lecture des secrets depuis l'environnement
import os
SECRET_KEY = os.environ["DJANGO_SECRET_KEY"]      # lève une erreur si absent (fail-fast)
DEBUG = os.environ.get("DJANGO_DEBUG", "false").lower() == "true"
```

### 2.3 Le modèle Equipement du fil rouge
Pour ce module, l'`Equipement` porte un **propriétaire** (l'utilisateur qui le gère) et un champ **sensible** (`notes_internes`) qui ne doit jamais être exposé par l'API.

```python
# parc/models.py
from django.conf import settings
from django.db import models

class Equipement(models.Model):
    hostname = models.CharField(max_length=200)
    ip = models.GenericIPAddressField()
    actif = models.BooleanField(default=True)
    proprietaire = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE,
                                     related_name="equipements")
    notes_internes = models.TextField(blank=True, default="")   # champ sensible

    def __str__(self):
        return self.hostname
```

---

## 3. OWASP API Security Top 10 — appliqué à Django / DRF

Pour chaque risque : **définition → exemple vulnérable → impact → correctif Django/DRF → bonnes pratiques**.

### API1:2023 — Broken Object Level Authorization (BOLA)
*(OWASP Top 10:2025 : A01 Broken Access Control)*

**Définition.** L'API expose des identifiants d'objets (`/api/equipements/42/`) mais ne vérifie pas que l'utilisateur authentifié a le droit d'accéder à **cet** objet précis. C'est le risque **le plus fréquent** des API.

**Vulnérable :**
```python
class EquipementViewSet(viewsets.ModelViewSet):
    queryset = Equipement.objects.all()          # tout le monde voit tout
    serializer_class = EquipementSerializer
    permission_classes = [IsAuthenticated]
# Bob, authentifié, fait GET /api/equipements/42/ (équipement d'Alice) -> 200 OK. FUITE.
```

**Impact.** Un utilisateur légitime accède/modifie/supprime les ressources des autres en changeant simplement l'identifiant dans l'URL.

**Correctif.** Filtrer le `queryset` par propriétaire **et** ajouter une permission au niveau objet.
```python
from rest_framework.permissions import IsAuthenticated, BasePermission

class IsProprietaire(BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.proprietaire_id == request.user.id

class EquipementViewSet(viewsets.ModelViewSet):
    serializer_class = EquipementSerializer
    permission_classes = [IsAuthenticated, IsProprietaire]

    def get_queryset(self):
        # chaque utilisateur ne voit QUE ses équipements
        return Equipement.objects.filter(proprietaire=self.request.user)

    def perform_create(self, serializer):
        serializer.save(proprietaire=self.request.user)
```
**Bonnes pratiques.** Ne jamais faire confiance à l'ID fourni ; contrôler l'appartenance dans **chaque** endpoint qui reçoit un identifiant ; préférer des identifiants non devinables (UUID) pour réduire l'énumération.

### API2:2023 — Broken Authentication
*(Top 10:2025 : A07 Authentication Failures)*

**Définition.** Mécanismes d'authentification absents, faibles ou mal implémentés : endpoints non protégés, jetons sans expiration, mots de passe faibles, absence de limitation des tentatives.

**Vulnérable :** `permission_classes = [AllowAny]` sur des endpoints sensibles ; jetons statiques qui n'expirent jamais.

**Correctif.**
```python
# settings.py
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "rest_framework_simplejwt.authentication.JWTAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES": ["rest_framework.permissions.IsAuthenticated"],
}
# Validateurs de robustesse des mots de passe (django.contrib.auth)
AUTH_PASSWORD_VALIDATORS = [
    {"NAME": "django.contrib.auth.password_validation.MinimumLengthValidator",
     "OPTIONS": {"min_length": 12}},
    {"NAME": "django.contrib.auth.password_validation.CommonPasswordValidator"},
]
```
**Bonnes pratiques.** Jetons **à durée de vie courte** avec *refresh* et rotation (JWT `access`/`refresh`), révocation possible ; HTTPS obligatoire ; limitation des tentatives de connexion (voir API4) ; vérifier **signature et expiration** de chaque jeton (ne jamais faire confiance aux `claims` non vérifiés). Pour une API interne simple, `TokenAuthentication` de DRF est acceptable ; pour du multi-service, préférer JWT.

### API3:2023 — Broken Object Property Level Authorization (BOPLA)
*(Top 10:2025 : A01 Broken Access Control — au niveau propriété)*

**Définition.** Combine l'**exposition excessive de données** (le sérialiseur renvoie des champs sensibles) et le **mass assignment** (le client peut écrire des champs qu'il ne devrait pas, ex. `proprietaire`, `is_admin`).

**Vulnérable :**
```python
class EquipementSerializer(serializers.ModelSerializer):
    class Meta:
        model = Equipement
        fields = "__all__"      # expose notes_internes ET laisse écrire proprietaire !
```

**Correctif.** Champs **explicites** (jamais `__all__` sur un modèle à champs sensibles) + `read_only_fields` + propriétaire imposé côté serveur.
```python
class EquipementSerializer(serializers.ModelSerializer):
    class Meta:
        model = Equipement
        fields = ["id", "hostname", "ip", "actif", "proprietaire"]  # notes_internes NON exposé
        read_only_fields = ["proprietaire"]                          # anti mass-assignment
# + perform_create(...serializer.save(proprietaire=self.request.user)) (cf. API1)
```
**Bonnes pratiques.** Lister explicitement les champs exposés en lecture et en écriture ; ne jamais exposer secrets, hachages, notes internes ; valider les permissions par propriété quand certains champs sont réservés (ex. seul un admin peut passer `actif`).

### API4:2023 — Unrestricted Resource Consumption
*(Top 10:2025 : A06 Insecure Design / déni de service)*

**Définition.** L'API ne borne pas les ressources qu'une requête ou un client peut consommer : pas de pagination, pas de limitation de débit, pas de limite de taille de charge utile.

**Correctif.**
```python
# settings.py
REST_FRAMEWORK = {
    # Pagination : ne jamais renvoyer une table entière
    "DEFAULT_PAGINATION_CLASS": "rest_framework.pagination.PageNumberPagination",
    "PAGE_SIZE": 50,
    # Limitation de débit (throttling)
    "DEFAULT_THROTTLE_CLASSES": [
        "rest_framework.throttling.UserRateThrottle",
        "rest_framework.throttling.AnonRateThrottle",
    ],
    "DEFAULT_THROTTLE_RATES": {"user": "1000/day", "anon": "50/day"},
}
# Limiter la taille des corps de requête (Django)
DATA_UPLOAD_MAX_MEMORY_SIZE = 2 * 1024 * 1024      # 2 Mo
DATA_UPLOAD_MAX_NUMBER_FIELDS = 1000
```
Au-delà de la limite, DRF renvoie **429 Too Many Requests**. **Bonnes pratiques.** Limiter par utilisateur *et* par IP ; timeouts sur les appels sortants ; borner la profondeur/complexité des requêtes ; limiter la taille des fichiers.

### API5:2023 — Broken Function Level Authorization (BFLA)
*(Top 10:2025 : A01 Broken Access Control — au niveau fonction/action)*

**Définition.** Une **action** réservée (ex. supprimer, ou une action d'administration) est accessible à des utilisateurs qui ne devraient pas y avoir droit. Analogue de BOLA, mais au niveau de la **fonction** et non de l'objet.

**Correctif.** Permissions **par action** dans le ViewSet :
```python
from rest_framework.permissions import IsAuthenticated, IsAdminUser

class EquipementViewSet(viewsets.ModelViewSet):
    serializer_class = EquipementSerializer

    def get_permissions(self):
        if self.action == "destroy":            # seul un admin peut supprimer
            return [IsAuthenticated(), IsAdminUser()]
        return [IsAuthenticated(), IsProprietaire()]
```
**Bonnes pratiques.** Refus par défaut ; séparer clairement endpoints d'administration et endpoints utilisateurs ; s'appuyer sur les **groupes/permissions** Django plutôt que sur des tests ad hoc.

### API6:2023 — Unrestricted Access to Sensitive Business Flows
*(Top 10:2025 : A06 Insecure Design)*

**Définition.** Un flux métier légitime (créer un compte, déclencher un scan réseau coûteux, exporter tout l'inventaire) est exposé sans protection contre l'**abus automatisé** (bots, scripts).

**Correctif.** Ce n'est pas qu'un bug de code : c'est de la **conception**. Combiner limitation de débit *métier* (throttle dédié à l'action sensible), détection d'automatisation (CAPTCHA, empreinte d'appareil), quotas, et validation humaine si nécessaire.
```python
from rest_framework.throttling import ScopedRateThrottle
class ScanViewSet(viewsets.ViewSet):
    throttle_classes = [ScopedRateThrottle]
    throttle_scope = "scan_reseau"          # ex. "5/hour" dans DEFAULT_THROTTLE_RATES
```

### API7:2023 — Server Side Request Forgery (SSRF)
*(pas de catégorie dédiée dans le Top 10:2025 ; était A10:2021)*

**Définition.** L'API va chercher une ressource distante à partir d'une **URL fournie par l'utilisateur** (webhook de test, import depuis une URL) sans la valider. L'attaquant fait alors émettre au serveur des requêtes vers des cibles internes : `http://169.254.169.254/` (métadonnées cloud, vol de credentials), services internes, `localhost`.

**Vulnérable :**
```python
@action(detail=True, methods=["post"])
def tester_webhook(self, request, pk=None):
    url = request.data["url"]
    r = requests.get(url)          # SSRF : aucune validation de l'URL
    return Response({"status": r.status_code})
```

**Correctif.** Valider l'URL : schéma autorisé, résolution DNS, **blocage des IP internes** (loopback, privées, link-local), timeout, pas de suivi de redirection non contrôlée.
```python
import ipaddress, socket
from urllib.parse import urlparse

def url_sortante_autorisee(url, schemes_autorises=("http", "https")):
    p = urlparse(url)
    if p.scheme not in schemes_autorises or not p.hostname:
        return False
    try:
        infos = socket.getaddrinfo(p.hostname, None)
    except socket.gaierror:
        return False
    for *_, sockaddr in infos:
        ip = ipaddress.ip_address(sockaddr[0])
        if ip.is_private or ip.is_loopback or ip.is_link_local or ip.is_reserved or ip.is_multicast:
            return False
    return True

# Utilisation : if not url_sortante_autorisee(url): -> 400 ; sinon requests.get(url, timeout=5, allow_redirects=False)
```
**Bonnes pratiques.** Préférer une **liste blanche** de domaines/hôtes autorisés ; isoler la fonction dans un réseau contraint ; ne jamais renvoyer telle quelle la réponse distante.

### API8:2023 — Security Misconfiguration
*(Top 10:2025 : A02 Security Misconfiguration)*

**Définition.** Configuration non durcie : `DEBUG=True` en production (fuite de la stack et des settings), `ALLOWED_HOSTS=['*']`, `SECRET_KEY` en dur, CORS permissif, en-têtes de sécurité absents, API navigable exposée, messages d'erreur verbeux.

**Correctif (extrait de `settings.py` de production) :**
```python
DEBUG = False
ALLOWED_HOSTS = ["api.monreseau.example"]

# En-têtes de sécurité (ajoutés par SecurityMiddleware)
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_REFERRER_POLICY = "same-origin"
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True

# CORS restreint (django-cors-headers) — surtout PAS CORS_ALLOW_ALL_ORIGINS=True
CORS_ALLOWED_ORIGINS = ["https://console.monreseau.example"]

# Désactiver l'API navigable en production (ne garder que le rendu JSON)
REST_FRAMEWORK = {"DEFAULT_RENDERER_CLASSES": ["rest_framework.renderers.JSONRenderer"]}
```
**Bonnes pratiques.** Un `settings` par environnement ; vérifier avec `python manage.py check --deploy` ; erreurs génériques côté client, détails uniquement dans les logs.

### API9:2023 — Improper Inventory Management
*(Top 10:2025 : lié à A02 / gouvernance des actifs)*

**Définition.** Versions d'API obsolètes toujours en ligne, endpoints de debug oubliés, absence de documentation et d'inventaire. Les vieux endpoints ont souvent des contrôles plus faibles.

**Correctif / bonnes pratiques.** Versionner l'API (`/api/v1/`), documenter automatiquement (OpenAPI via **drf-spectacular**), tenir un inventaire des endpoints et environnements, retirer/mettre hors ligne les versions dépréciées, séparer prod et hors-prod.
```python
REST_FRAMEWORK = {
    "DEFAULT_VERSIONING_CLASS": "rest_framework.versioning.URLPathVersioning",
    "DEFAULT_VERSION": "v1", "ALLOWED_VERSIONS": ["v1"],
}
```

### API10:2023 — Unsafe Consumption of APIs
*(Top 10:2025 : A03 Software Supply Chain Failures / A08 Integrity)*

**Définition.** On fait davantage confiance aux données d'une **API tierce** qu'à celles de l'utilisateur. Un attaquant compromet le service intégré pour atteindre notre API.

**Correctif.** Traiter toute réponse externe comme **non fiable** :
```python
import requests
r = requests.get("https://fournisseur.example/api/hosts",
                 timeout=5,            # jamais d'appel sans timeout
                 verify=True)          # toujours vérifier le certificat TLS
r.raise_for_status()
donnees = r.json()
# -> VALIDER la structure/les types avant usage (ne pas insérer tel quel en base)
```
**Bonnes pratiques.** Timeouts systématiques, vérification TLS, validation stricte des réponses, liste blanche des services, limitation des redirections, journalisation des échecs.

---

## 4. Sujets transverses

- **Validation des entrées (lien A05 Injection).** Les sérialiseurs DRF valident et typent les données ; l'ORM Django paramètre les requêtes (anti-injection SQL). Ne jamais construire de requête par concaténation, ne jamais passer d'entrée à `eval`/`os.system`.
- **Authentification : sessions vs jetons vs JWT.** Sessions (cookie) pour un front couplé ; `TokenAuthentication` pour une API interne simple ; **JWT** (`access` court + `refresh` + rotation) pour le multi-service. Toujours HTTPS, expiration et révocation.
- **Journalisation & supervision (A09 : Security Logging and Alerting Failures).** Journaliser les échecs d'authentification/autorisation, les 4xx/5xx anormaux, les accès sensibles — **sans** journaliser secrets ni jetons. Mettre en place des **alertes** (pics de 401/403/429).
- **Secrets & configuration.** Variables d'environnement / gestionnaire de secrets ; `.env` hors du dépôt ; rotation des clés.
- **CORS & en-têtes.** Restreindre les origines ; HSTS, `nosniff`, `Referrer-Policy`, `X-Frame-Options`.
- **Chaîne d'approvisionnement (A03).** Figer les dépendances (`requirements.txt` versionné), auditer (`pip-audit`), mettre à jour régulièrement.
- **Gestion des erreurs (A10:2025 Mishandling of Exceptional Conditions).** Ne pas divulguer de détails techniques au client ; renvoyer des codes cohérents ; ne pas « échouer en mode ouvert » (un échec de contrôle doit refuser l'accès).

---

## 5. Checklist de durcissement d'une API REST

- [ ] HTTPS partout + HSTS ; redirection HTTP→HTTPS.
- [ ] `DEBUG = False`, `ALLOWED_HOSTS` explicite, `SECRET_KEY` via variable d'environnement.
- [ ] Authentification requise par défaut (`IsAuthenticated`) ; jetons à durée limitée.
- [ ] Autorisation au niveau **objet** (BOLA) et au niveau **action** (BFLA) sur chaque endpoint.
- [ ] Sérialiseurs à **champs explicites** ; `read_only_fields` ; aucun champ sensible exposé.
- [ ] Pagination + limitation de débit (throttling) + limite de taille de charge utile.
- [ ] Validation de toute URL sortante (anti-SSRF) ; timeouts et `verify=True` sur les appels externes.
- [ ] En-têtes de sécurité activés ; CORS restreint ; API navigable désactivée en prod.
- [ ] Versionnage + documentation (OpenAPI) + inventaire des endpoints ; retrait des versions obsolètes.
- [ ] Journalisation des échecs authn/authz + alerting ; pas de secret dans les logs.
- [ ] Dépendances figées et auditées (`pip-audit`).
- [ ] `python manage.py check --deploy` sans avertissement critique.

---

## 6. Correspondance API Security Top 10 (2023) ↔ OWASP Top 10:2025

| API Security Top 10 (2023) | OWASP Top 10:2025 (général) |
| :--- | :--- |
| API1 BOLA | A01 Broken Access Control |
| API2 Broken Authentication | A07 Authentication Failures |
| API3 BOPLA | A01 Broken Access Control |
| API4 Unrestricted Resource Consumption | A06 Insecure Design |
| API5 BFLA | A01 Broken Access Control |
| API6 Sensitive Business Flows | A06 Insecure Design |
| API7 SSRF | (dédiée A10:2021 ; intégrée au contrôle d'accès en 2025) |
| API8 Security Misconfiguration | A02 Security Misconfiguration |
| API9 Improper Inventory Management | A02 / gouvernance des actifs |
| API10 Unsafe Consumption of APIs | A03 Software Supply Chain Failures |
| (validation des entrées) | A05 Injection |
| (journalisation) | A09 Security Logging and Alerting Failures |

---

## Sources
- *OWASP API Security Top 10 — 2023* (owasp.org/API-Security/editions/2023/)
- *OWASP Top 10:2025* (owasp.org/Top10/2025/)
- *Documentation Django REST Framework — Authentication, Permissions, Throttling, Pagination* (django-rest-framework.org)
- *Documentation Django — Security & Deployment checklist* (docs.djangoproject.com/en/stable/topics/security/, .../howto/deployment/checklist/)
- *djangorestframework-simplejwt* (django-rest-framework-simplejwt.readthedocs.io) ; *django-cors-headers* ; *drf-spectacular* ; *pip-audit*
