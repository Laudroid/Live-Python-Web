## Corrigé 3.1 — Première API Flask (lecture)

```python
from flask import Flask, jsonify

app = Flask(__name__)

equipements = [
    {"id": 1, "hostname": "srv-web-01", "ip": "192.168.1.10"},
    {"id": 2, "hostname": "sw-access-02", "ip": "192.168.1.2"},
]


@app.route("/api/equipements", methods=["GET"])
def liste_equipements():
    return jsonify({"equipements": equipements})


@app.route("/api/equipements/<int:equipement_id>", methods=["GET"])
def detail_equipement(equipement_id):
    equipement = next((e for e in equipements if e["id"] == equipement_id), None)
    if equipement is None:
        return jsonify({"erreur": "Équipement non trouvé"}), 404
    return jsonify(equipement)


if __name__ == "__main__":
    app.run(debug=True)
```

**Points clés :** convertisseur `<int:...>` dans la route dynamique ; renvoyer un tuple `(payload, code)` pour forcer le **404** ; `jsonify` fixe le bon `Content-Type: application/json`.

**Barème (/20) :** route liste **5**, route détail **5**, 404 géré **4**, JSON correct **2**, `USAGE-IA.md` **4**.
