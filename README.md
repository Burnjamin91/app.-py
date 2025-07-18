# app.py
from flask import Flask, jsonify, request
import json
import os

app = Flask(__name__)

DATA_FILE = "data.json"

# Root route to check if server is running
@app.route('/')
def home():
    return "✅ Hunt Royale Backend läuft!"

# API route to fetch stats by user ID
@app.route('/stats/<user_id>', methods=['GET'])
def get_stats(user_id):
    if not os.path.exists(DATA_FILE):
        return jsonify({"error": "Keine Daten gefunden."}), 404

    with open(DATA_FILE, 'r') as f:
        data = json.load(f)

    stats = data.get(user_id)
    if stats:
        return jsonify(stats)
    else:
        return jsonify({"error": "Kein Profil für diese ID gefunden."}), 404

# Optional: POST route to update stats
@app.route('/update/<user_id>', methods=['POST'])
def update_stats(user_id):
    new_stats = request.json

    if not os.path.exists(DATA_FILE):
        data = {}
    else:
        with open(DATA_FILE, 'r') as f:
            data = json.load(f)

    data[user_id] = new_stats

    with open(DATA_FILE, 'w') as f:
        json.dump(data, f, indent=2)

    return jsonify({"status": "ok", "message": f"Profil {user_id} aktualisiert."})

if __name__ == '__main__':
    app.run(debug=True)from flask import Flask, jsonify, request
import json
import os

app = Flask(__name__)

DATA_FILE = "data.json"

# Root route to check if server is running
@app.route('/')
def home():
    return "✅ Hunt Royale Backend läuft!"

# API route to fetch stats by user ID
@app.route('/stats/<user_id>', methods=['GET'])
def get_stats(user_id):
    if not os.path.exists(DATA_FILE):
        return jsonify({"error": "Keine Daten gefunden."}), 404

    with open(DATA_FILE, 'r') as f:
        data = json.load(f)

    stats = data.get(user_id)
    if stats:
        return jsonify(stats)
    else:
        return jsonify({"error": "Kein Profil für diese ID gefunden."}), 404

# Optional: POST route to update stats
@app.route('/update/<user_id>', methods=['POST'])
def update_stats(user_id):
    new_stats = request.json

    if not os.path.exists(DATA_FILE):
        data = {}
    else:
        with open(DATA_FILE, 'r') as f:
            data = json.load(f)

    data[user_id] = new_stats

    with open(DATA_FILE, 'w') as f:
        json.dump(data, f, indent=2)

    return jsonify({"status": "ok", "message": f"Profil {user_id} aktualisiert."})

if __name__ == '__main__':
    app.run(debug=True)
