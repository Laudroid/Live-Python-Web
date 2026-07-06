Corrigé 1.1 — Rapport d'inventaire

```python
equipements = [
    {"hostname": "srv-web-01",  "ip": "192.168.1.10", "type": "serveur", "actif": True,  "latence_ms": 12.4},
    {"hostname": "sw-access-02", "ip": "192.168.1.2",  "type": "switch",  "actif": True,  "latence_ms": 1.2},
    {"hostname": "rtr-core-01",  "ip": "10.0.0.1",     "type": "routeur", "actif": False, "latence_ms": 3.5},
    {"hostname": "sw-access-03", "ip": "192.168.1.3",  "type": "switch",  "actif": True,  "latence_ms": 1.8},
]

# 1. Nombre d'équipements actifs
nb_actifs = 0
for e in equipements:
    if e["actif"]:
        nb_actifs += 1

# 2. Hostnames des switchs (liste en compréhension)
switchs = [e["hostname"] for e in equipements if e["type"] == "switch"]

# 3. Latence moyenne des équipements actifs
latences_actifs = [e["latence_ms"] for e in equipements if e["actif"]]
latence_moyenne = sum(latences_actifs) / len(latences_actifs)

# 4. Rapport
print(f"Équipements : {len(equipements)} | Actifs : {nb_actifs}")
print(f"Switchs : {', '.join(switchs)}")
print(f"Latence moyenne (actifs) : {latence_moyenne:.2f} ms")
```

**Sortie attendue :**
```text
Équipements : 4 | Actifs : 3
Switchs : sw-access-02, sw-access-03
Latence moyenne (actifs) : 5.13 ms
```

**Points clés :** compréhension de liste avec condition (`if`), `sum()/len()` pour la moyenne, formatage `:.2f`. `nb_actifs` peut aussi s'écrire `sum(1 for e in equipements if e["actif"])`.