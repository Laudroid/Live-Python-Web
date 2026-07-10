## Corrigé 6 — Capstone : audit et durcissement

### a) Audit des failles (`AUDIT.md`)

| # | Faille | OWASP | Détail |
| --- | --- | --- | --- |
| 1 | `permission_classes = [AllowAny]` | **API2** | Aucune authentification : tout est public. |
| 2 | `queryset = Equipement.objects.all()` | **API1 (BOLA)** | Chaque client accède aux équipements de tous. |
| 3 | `fields = "__all__"` | **API3 (BOPLA)** | Exposition de champs sensibles + mass assignment. |
| 4 | `requests.get(url)` sur URL fournie | **API7 (SSRF)** | Accès aux services internes / métadonnées cloud. |
| 5 | Pas de `timeout`, corps distant renvoyé tel quel | **API10 / API4** | Consommation non sûre, blocage possible, fuite. |
| 6 | `AllowAny` global, `DEBUG` probable | **API8** | Configuration non durcie. |

### b) Code corrigé

```python
# audit/views.py (corrigé)
import ipaddress, socket
from urllib.parse import urlparse

import requests
from rest_framework import viewsets, serializers, status
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated, BasePermission
from .models import Equipement


class IsProprietaire(BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.proprietaire_id == request.user.id


class EquipementSerializer(serializers.ModelSerializer):
    class Meta:
        model = Equipement
        fields = ["id", "hostname", "ip", "actif", "proprietaire"]   # pas de champ sensible
        read_only_fields = ["proprietaire"]


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


class EquipementViewSet(viewsets.ModelViewSet):
    serializer_class = EquipementSerializer
    permission_classes = [IsAuthenticated, IsProprietaire]      # API2 + API1

    def get_queryset(self):                                     # API1 (BOLA)
        return Equipement.objects.filter(proprietaire=self.request.user)

    def perform_create(self, serializer):
        serializer.save(proprietaire=self.request.user)

    @action(detail=False, methods=["post"])
    def tester_webhook(self, request):
        url = request.data.get("url", "")
        if not url_sortante_autorisee(url):                     # API7 (SSRF)
            return Response({"erreur": "URL non autorisée"}, status=status.HTTP_400_BAD_REQUEST)
        try:
            reponse = requests.get(url, timeout=5, allow_redirects=False)  # API10
        except requests.RequestException:
            return Response({"erreur": "hôte injoignable"}, status=status.HTTP_502_BAD_GATEWAY)
        # on ne renvoie PAS le corps distant tel quel, seulement une info maîtrisée
        return Response({"status": reponse.status_code})
```
Et côté configuration (`AUDIT.md` / `settings.py`) : `DEBUG = False`, authentification globale, en-têtes de sécurité (cf. corrigé 5).

**Points clés :** l'endpoint SSRF est le cœur de l'exercice — valider le schéma, résoudre le DNS et **bloquer les IP internes** (dont `169.254.169.254`), poser un `timeout`, désactiver le suivi de redirection et ne pas relayer le corps distant.
*(Vérifié : le validateur refuse `127.0.0.1`, `169.254.169.254`, `10.0.0.5`, les schémas non http(s), et accepte une IP publique.)*