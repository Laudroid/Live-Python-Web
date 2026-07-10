## Consignes communes à tous les exercices

**Usage de l'IA — transparence obligatoire.** L'IA générative n'est **pas interdite**, mais son usage doit être **assumé et documenté**. Chaque rendu **doit contenir un fichier `USAGE-IA.md`** (sinon rendu **incomplet**) précisant : (1) les **outils** utilisés et pour quoi, (2) l'**aide obtenue** (avec un exemple de prompt), (3) ce que vous avez **accepté** et *pourquoi vous l'avez compris/validé*, (4) ce que vous avez **refusé ou corrigé** et *pourquoi*, (5) ce que vous avez **appris**. Aucune IA utilisée ? Déclarez-le.

> **Règle d'or (renforcée en sécurité) :** vous devez pouvoir **expliquer la faille, son impact et votre correctif**. En sécurité, copier un correctif sans le comprendre est un risque en soi.

**Pré-requis technique :** environnement virtuel, `pip install django djangorestframework`. Pour l'exercice 6, `pip install requests` (déjà vu au module 3).

**Livrables communs :** le code modifié (fichiers `.py`), un court `TESTS.md` montrant vos essais (commandes/`curl`/captures) et `USAGE-IA.md`. Gabarit de `USAGE-IA.md` en annexe.

---

## Exercice 1 — Exiger l'authentification (API2)

**Risque OWASP :** API2 Broken Authentication.

**Contexte :** l'API d'inventaire actuelle répond à tout le monde, y compris aux requêtes anonymes.

**Consignes :**
1. Activez l'authentification par **jeton** de DRF : ajoutez `rest_framework.authtoken` à `INSTALLED_APPS`, appliquez la migration, et configurez `DEFAULT_AUTHENTICATION_CLASSES` (`TokenAuthentication`).
2. Rendez l'`EquipementViewSet` accessible uniquement aux utilisateurs authentifiés (`IsAuthenticated`).
3. Créez un utilisateur et générez son jeton (`drf_create_token` ou via le shell).
 	python manage.py createsuperuser
	   Créer un utilisateur 'admin'
 	python manage.py drf_create_token admin
	   Generated token e94d4f34ead7c5ab074cf8b7cd7d7adc19ee441f for user admin
4. Exiger l'authentification dans views.py :
	permission_classes = [IsAuthenticated]

5. Vérifiez : une requête **sans** jeton est refusée (401/403), une requête **avec** l'en-tête `Authorization: Token <clé>` réussit (200).

**Critères :** authentification effective, endpoint protégé, démonstration avec et sans jeton.