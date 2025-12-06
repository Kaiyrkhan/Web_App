# Production ортада Python App (Flask/Django) конфигурациялау

Client → Nginx → Gunicorn (WSGI server) → Python App (WSGI application)  

Nginx — HTTP server + Reverse proxy  
Gunicorn — WSGI server / Application server  
Python App — WSGI application  
Flask/Django — Python Framework  

### Жоба құрылымы
```shell
/var/www/flaskapp/
│
├── venv/
├── app/
│   ├── __init__.py
│   └── main.py
│
├── wsgi.py
└── requirements.txt
```

### Flask application

**app/main.py**
```shell
from flask import Flask

def create_app():
    app = Flask(__name__)

    @app.route("/")
    def index():
        return "Hello from Flask via Gunicorn & Nginx!"

    return app
```

### WSGI application

**wsgi.py**
```shell
from app.main import create_app

app = create_app()

if __name__ == "__main__":
    app.run()
```

```shell
```

```shell
```

```shell
```
