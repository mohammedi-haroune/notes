# Flask
## Databases
Flask-SQLAlchemy, an extension that provides a Flask-friendly wrapper to the popular SQLAlchemy package, which is an Object Relational Mapper or ORM.
```bash
pip install flask-sqlalchemy
```

Flask-Migrate a database migration framework for SQLAlchemy.
```bash
pip install flask-migrate
```
### [Configurations](http://flask-sqlalchemy.pocoo.org/2.3/config/#configuration-keys)
Most important configuration is `SQLALCHEMY_DATABASE_URI`

### Models
#### `app/models.py`
```python
from datetime import datetime
from app import db

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), index=True, unique=True)
    email = db.Column(db.String(120), index=True, unique=True)
    password_hash = db.Column(db.String(128))
    posts = db.relationship('Post', backref='author', lazy='dynamic')

    def __repr__(self):
        return '<User {}>'.format(self.username)

class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    body = db.Column(db.String(140))
    timestamp = db.Column(db.DateTime, index=True, default=datetime.utcnow)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'))

    def __repr__(self):
        return '<Post {}>'.format(self.body)
```

### Database Migrations
!!! note
    `FLASK_APP` should be set for these commands to execute properly

#### Create the migrations repository
```bash
flask db init
```

#### Generate automatic migrations
```bash
flask db migrate
```
We can provide a `-m` optional argument that adds a short descriptive text to the migration.
```bash
flask db migrate -m "added users table"
```

#### Upgrade
The `flask db migrate` command does not make any changes to the database, it just generates the migration script. To apply the changes to the database, the `flask db upgrade` command must be used.

#### Downgrade
```bash
flask db downgrade
```

## Blueprints
Blueprints is the way flask achieve modularity, reusablity and extensibility.

Here's a simple hello world blueprint.

```
.
├── app
│   ├── hello
│   │   ├── __init__.py
│   │   ├── routes.py
│   │   └── templates
│   │       └── index.html
│   ├── __init__.py
├── flask_learning.py
```

### `app/__init__.py`
```python
from flask import Flask
from app.hello import blueprint

def create_app():
    app = Flask(__name__)
    app.register_blueprint(blueprint)
    return app
```


### `app/hello/__init__.py`
```python
from flask import Blueprint

blueprint = Blueprint('hello_blueprint', __name__, template_folder='templates')

from app.hello import routes
```

### `app/hello/routes.py`
```python
from flask import render_template

from app.hello import blueprint


@blueprint.route("/<string:name>")
def index(name):
    return render_template("index.html", name=name)
```

### `flask_learning.py`
```python
from app import create_app

app = create_app()
```

## Configuring the app
Flask provides many ways of loading configurations to the application.

One way is to define a configuration object and load configuration from there using `app.config.from_object` method.

!!! note
    `from_object` loads only the uppercase attributes of the module/class

Using objects for configuration we can separate different configuration for **developpement**, **testing** and **production**

```python
class Config(object):
    SOME_CONFIG = 'default-value'

class Devlopement(Config):
    pass

class Production(Config):
    SOME_CONFIG = 'production-value'

class Testing(Config):
    SOME_CONFIG = 'testing-config'
```

## Jija
### Basic
```python
template_dir = os.path.dirname(template_path)
template_name = os.path.basename(template_path)
env = Environment(loader=FileSystemLoader(template_dir), **env_kwargs)
template = env.get_template(template_name)

```
### Raise exception
```python
def raise_from_jinja(*args, **kwargs):
    raise Exception(*args, **kwargs)

# And add it to the globals of the template
template.globals.update({'raise': raise_from_jinja})
```
