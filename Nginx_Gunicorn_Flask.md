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

### Virtual environment
```shell
$ cd /var/www/flaskapp

$ python3 -m venv venv
$ source venv/bin/activate

$ pip install flask gunicorn
```

### Gunicorn systemd service (Production mode)
> Gunicorn-ды systemd арқылы басқару — ең дұрыс әдіс  

Service файлды құру және конфигурациялау
```shell
$ sudo nano /etc/systemd/system/flaskapp.service

[Unit]
Description=Gunicorn service for Flask App
After=network.target

[Service]
User=www-data
Group=www-data
WorkingDirectory=/var/www/flaskapp
Environment="PATH=/var/www/flaskapp/venv/bin"
ExecStart=/var/www/flaskapp/venv/bin/gunicorn --workers 4 --bind unix:/var/www/flaskapp/flaskapp.sock wsgi:app

[Install]
WantedBy=multi-user.target
```
> Мұндағы, 4 worker — орташа сервер үшін жақсы  

```shell
```

```shell
```
