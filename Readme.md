# FastAPI Overview and Best Practices
- ### [Organization](#Organization)
- ### [Serializers](#Serializers)
- ### [Authentification](#Authentification)
- ### [Authorization](#Authorization)
- ### [Swaggers](#Swagger)
- ### [Interact with database](#Serializers)
- ### [API Versioning](#Versioning)
- ### [API Routing](#Routing)
- ### [Factory Pattern](#Factory_Pattern)




## Organization

```
app/
├── manage.py
├── app
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── 1st_app
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── views.py
│   ├── urls.py
│   └── migrations
│       ├── migration_file1.py
│       └── ...
├── 2nd_app
│   └── ...
└── ...
```

## Versioning

Versioning is an effective communication around changes to your API. It will help to develop new features on your API without breaking the client's applications when new updates are deployed.

To do versioning with Django, you can create a new app with `python manage.py startapp <app_name>`. You can create a new app per version you need, if the versions need to be alive together.


## Routing 
In Django, the routing is processed through the urls given into the `urls.py` files.

The main file, `app/app/urls.py`, contains linked to the urls files of the different apps of the project as:

TODO: put example

Each app's urls file - `app/1st_app/urls.py` for example - is linking each route to its associated view. You can also define a name for each route to be able to retrieve them easily during tests.

TODO: put example


## Factory_Pattern

<b>1) Initiating Flask in __init__.py </b>

Our fastAPI object should be created in ```application/__init__.py```:

```
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_redis import FlaskRedis


# Globally accessible libraries
db = SQLAlchemy()
r = FlaskRedis()


def init_app():
    """Initialize the core application."""
    app = Flask(__name__, instance_relative_config=False)
    app.config.from_object('config.Config')

    # Initialize Plugins
    db.init_app(app)
    r.init_app(app)

    with app.app_context():
        # Include our Routes
        from . import routes

        # Register Blueprints
        app.register_blueprint(auth.auth_bp)
        app.register_blueprint(admin.admin_bp)

        return app
```

The ```wsgi.py``` simply imports the ```init_app()``` method to serve as our app gateway:

<b>2) Use wsgi.py as a gateway  </b>

wsgi.py simply imports this file to serve as our app gateway:

```
from application import init_app


app = init_app()

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```

## Swagger


## Serializers
- response_model
- BaseModel
- Typing 

## Authentification
- token: OAuth2, jwt
- cookies

TODO

## Authorization

TODO 


## Interact with database

talk about ORM


## Sources
- https://hackersandslackers.com/flask-application-factory/
- https://github.com/DeanWay/fastapi-versioning
