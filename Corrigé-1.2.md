## Corrigé 1.2 — POO & sous-réseau

```python
class Equipement:
    def __init__(self, hostname, ip, latence_ms, actif=True):
        self.hostname = hostname
        self.ip = ip
        self.latence_ms = latence_ms
        self.actif = actif

    def __str__(self):
        etat = "actif" if self.actif else "inactif"
        return f"{self.hostname} ({self.ip}) - {etat} - {self.latence_ms} ms"

    def est_operationnel(self, seuil_ms=100):
        """Opérationnel si l'équipement est actif ET sa latence sous le seuil."""
        return self.actif and self.latence_ms < seuil_ms


def hotes_disponibles(bits_hote):
    """Nombre d'adresses hôtes utilisables dans un sous-réseau.

    On retire l'adresse réseau et l'adresse de broadcast.
    """
    return (2 ** bits_hote) - 2


def trier_par_latence(equipements):
    """Retourne les équipements triés par latence croissante."""
    return sorted(equipements, key=lambda e: e.latence_ms)


if __name__ == "__main__":
    parc = [
        Equipement("srv-web-01", "192.168.1.10", 12.4),
        Equipement("sw-access-02", "192.168.1.2", 1.2),
        Equipement("rtr-core-01", "10.0.0.1", 250.0, actif=False),
    ]

    for e in trier_par_latence(parc):
        print(e, "| opérationnel:", e.est_operationnel())

    print("Hôtes disponibles dans un /24 :", hotes_disponibles(8))
```

**Sortie attendue :**
```text
sw-access-02 (192.168.1.2) - actif - 1.2 ms | opérationnel: True
srv-web-01 (192.168.1.10) - actif - 12.4 ms | opérationnel: True
rtr-core-01 (10.0.0.1) - inactif - 250.0 ms | opérationnel: False
Hôtes disponibles dans un /24 : 254
```

**Points clés :** `est_operationnel` combine bien les deux conditions (`and`) ; le tri utilise `sorted(..., key=lambda e: e.latence_ms)` (ne modifie pas la liste d'origine) ; `2 ** 8 - 2 = 254`. Piège fréquent : oublier que `rtr-core-01` est **inactif** → non opérationnel même si on regardait la latence.