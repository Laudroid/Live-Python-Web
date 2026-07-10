## Exercice 6 — Capstone : audit et durcissement d'une API vulnérable *(Avancé — 45 min)*

**Risques OWASP visés :** API1, API2, API3, API7 (SSRF), API8, API10.

**Contexte :** on vous fournit l'extrait suivant d'une API DRF de supervision. Elle contient **plusieurs failles**, dont un endpoint qui teste une URL de webhook fournie par l'utilisateur.

```python
# audit/views.py  (API VOLONTAIREMENT VULNÉRABLE — à auditer)
import requests
from rest_framework import viewsets, serializers
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.permissions import AllowAny
from .models import Equipement

class EquipementSerializer(serializers.ModelSerializer):
    class Meta:
        model = Equipement
        fields = "__all__"

class EquipementViewSet(viewsets.ModelViewSet):
    queryset = Equipement.objects.all()
    serializer_class = EquipementSerializer
    permission_classes = [AllowAny]

    @action(detail=False, methods=["post"])
    def tester_webhook(self, request):
        url = request.data["url"]
        reponse = requests.get(url)
        return Response({"status": reponse.status_code, "corps": reponse.text})
```

**Consignes :**
1. **Auditez** cet extrait : dressez la liste des failles en les rattachant à leur identifiant OWASP (API1, API2, API3, API7, API8, API10). Rédigez cet audit dans `AUDIT.md`.
2. **Corrigez** le code :
   - authentification requise et autorisation au niveau objet (cloisonnement par propriétaire) ;
   - sérialiseur à champs explicites, sans champ sensible, sans mass assignment ;
   - endpoint `tester_webhook` : **valider l'URL** (schéma autorisé, blocage des IP privées / loopback / link-local comme `169.254.169.254`), ajouter un `timeout`, ne pas suivre aveuglément les redirections, et **ne pas** renvoyer tel quel le corps distant ;
   - signalez au moins un point de configuration à durcir (`DEBUG`, `AllowAny` global, etc.).
3. Vérifiez que votre validateur d'URL refuse `http://169.254.169.254/`, `http://127.0.0.1/`, `http://10.0.0.5/` et accepte une URL publique légitime.

**Livrables :** `AUDIT.md` (failles + OWASP), le code corrigé, `TESTS.md`, `USAGE-IA.md`.
