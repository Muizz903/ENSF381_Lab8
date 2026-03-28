from copy import deepcopy
from flask import Flask, jsonify, request
from flask_cors import CORS

SEEDED_USERS = {
    "1": {"id": "1", "first_name": "Ava", "user_group": 11},
    "2": {"id": "2", "first_name": "Ben", "user_group": 22},
    "3": {"id": "3", "first_name": "Chloe", "user_group": 33},
    "4": {"id": "4", "first_name": "Diego", "user_group": 44},
    "5": {"id": "5", "first_name": "Ella", "user_group": 55},
}

app = Flask(__name__)
CORS(app)

users = deepcopy(SEEDED_USERS)


def get_json_body():
    data = request.get_json(silent=True)
    if not isinstance(data, dict):
        return None
    return data


def clean_string(value):
    return str(value).strip()


@app.route("/users", methods=["GET"])
def get_users():
    return jsonify(list(users.values())), 200


@app.route("/users", methods=["POST"])
def create_user():
    data = get_json_body()
    if data is None:
        return jsonify({"message": "Request body must be valid JSON."}), 400

    user_id = clean_string(data.get("id", ""))
    first_name = clean_string(data.get("first_name", ""))
    user_group = data.get("user_group")

    if not user_id or not first_name or user_group in (None, ""):
        return jsonify({"message": "id, first_name, and user_group are required."}), 400

    try:
        user_group = int(user_group)
    except (TypeError, ValueError):
        return jsonify({"message": "user_group must be a number."}), 400

    if user_id in users:
        return jsonify({"message": f"User {user_id} already exists."}), 409

    users[user_id] = {
        "id": user_id,
        "first_name": first_name,
        "user_group": user_group,
    }

    return jsonify({
        "id": user_id,
        "first_name": first_name,
        "user_group": user_group,
        "message": f"Created user {user_id}.",
    }), 201


@app.route("/users/<user_id>", methods=["PUT"])
def update_user(user_id):
    user_id = str(user_id)

    if user_id not in users:
        return jsonify({"message": f"User {user_id} was not found."}), 404

    data = get_json_body()
    if data is None:
        return jsonify({"message": "Request body must be valid JSON."}), 400

    first_name = clean_string(data.get("first_name", ""))
    user_group = data.get("user_group")

    if not first_name or user_group in (None, ""):
        return jsonify({"message": "first_name and user_group are required."}), 400

    try:
        user_group = int(user_group)
    except (TypeError, ValueError):
        return jsonify({"message": "user_group must be a number."}), 400

    users[user_id] = {
        "id": user_id,
        "first_name": first_name,
        "user_group": user_group,
    }

    return jsonify({
        "id": user_id,
        "first_name": first_name,
        "user_group": user_group,
        "message": f"Updated user {user_id}.",
    }), 200


@app.route("/users/<user_id>", methods=["DELETE"])
def delete_user(user_id):
    user_id = str(user_id)

    if user_id not in users:
        return jsonify({"message": f"User {user_id} was not found."}), 404

    del users[user_id]
    return jsonify({"message": f"Deleted user {user_id}."}), 200


if __name__ == "__main__":
    app.run(debug=True, port=5050)
