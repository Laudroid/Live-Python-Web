## Corrigé 3.2 — API CRUD Flask complète

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

equipements = [
    {"id": 1, "hostname": "srv-web-01", "ip": "192.168.1.10", "actif": True},
    {"id": 2, "hostname": "sw-access-02", "ip": "192.168.1.2", "actif": True},
]


def _trouver(equipement_id):
    return next((e for e in equipements if e["id"] == equipement_id), None)


@app.route("/api/equipements", methods=["GET"])
def liste_equipements():
    return jsonify({"equipements": equipements}), 200


@app.route("/api/equipements/<int:equipement_id>", methods=["GET"])
def detail_equipement(equipement_id):
    equipement = _trouver(equipement_id)
    if equipement is None:
        return jsonify({"erreur": "Équipement non trouvé"}), 404
    return jsonify(equipement), 200


@app.route("/api/equipements", methods=["POST"])
def creer_equipement():
    donnees = request.get_json(silent=True)
    if not donnees or "hostname" not in donnees:
        return jsonify({"erreur": "Le champ hostname est obligatoire"}), 400
    nouvel_equipement = {
        "id": max((e["id"] for e in equipements), default=0) + 1,
        "hostname": donnees["hostname"],
        "ip": donnees.get("ip", ""),
        "actif": True,
    }
    equipements.append(nouvel_equipement)
    return jsonify(nouvel_equipement), 201


@app.route("/api/equipements/<int:equipement_id>", methods=["PUT"])
def modifier_equipement(equipement_id):
    equipement = _trouver(equipement_id)
    if equipement is None:
        return jsonify({"erreur": "Équipement non trouvé"}), 404
    donnees = request.get_json(silent=True) or {}
    for champ in ("hostname", "ip", "actif"):
        if champ in donnees:
            equipement[champ] = donnees[champ]
    return jsonify(equipement), 200


@app.route("/api/equipements/<int:equipement_id>", methods=["DELETE"])
def supprimer_equipement(equipement_id):
    global equipements
    if _trouver(equipement_id) is None:
        return jsonify({"erreur": "Équipement non trouvé"}), 404
    equipements = [e for e in equipements if e["id"] != equipement_id]
    return jsonify({"message": "Équipement supprimé"}), 200


if __name__ == "__main__":
    app.run(debug=True)
```

**Exemples de test (`TESTS.md`) :**
```bash
curl -X POST http://127.0.0.1:5000/api/equipements \
     -H "Content-Type: application/json" -d '{"hostname":"srv-dns-01","ip":"192.168.1.20"}'   # 201
curl -X POST http://127.0.0.1:5000/api/equipements \
     -H "Content-Type: application/json" -d '{"ip":"1.2.3.4"}'                                  # 400
curl -X PUT http://127.0.0.1:5000/api/equipements/1 \
     -H "Content-Type: application/json" -d '{"actif":false}'                                   # 200
curl -X DELETE http://127.0.0.1:5000/api/equipements/1                                          # 200
curl -X DELETE http://127.0.0.1:5000/api/equipements/1                                          # 404
```

**Points clés :** `request.get_json(silent=True)` évite une erreur 400 si le corps n'est pas du JSON ; `hostname` obligatoire → **400** ; `id` généré via `max(...) + 1` ; `DELETE` refait par filtrage de la liste.