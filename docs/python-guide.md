# Quickstart: Integrating Protekt Authentication with Python (Flask)

## Prerequisites
- Python 3.9+ installed
- A Protekt account + API key
- Basic knowledge of Flask

---

### Step 1 – Install SDK
```bash
    pip install protekt-sdk flask
```

### Step 2 – Initialize Protekt in Your Flask App
```python
from flask import Flask, request, jsonify
from protekt_sdk import Protekt

app = Flask(__name__)
protekt = Protekt(api_key="YOUR_API_KEY")
```

### Step 3 – Add Login Endpoint
```python
@app.route('/login', methods=['POST'])
def login():
    data = request.get_json()
    email = data.get('email')
    password = data.get('password')
    try:
        user = protekt.auth.login(email, password)
        return jsonify({"token": user.token}), 200
    except Exception as e:
        return jsonify({"error": "Invalid credentials"}), 401
```

### Step 4 – Protect a Route
```python
@app.route('/profile')
@protekt.auth.verify_token
def profile(user):
    return jsonify({"user": user}), 200
```

### Step 5 – Run and Test
Start the server
```bash
python app.py
```
Login request:
```bash
curl -X POST http://127.0.0.1:5000/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password"}'
```

## Next Steps
- Add Multi-Factor Authentication (MFA) for stronger security.
- Integrate OAuth2 login with providers like Google/GitHub.
- Monitor API usage via the Protekt Dashboard.