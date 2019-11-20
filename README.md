# Flask-PostgreSQL Boilerplate

This is an minimal boilerplate meant for building out simple REST APIs. It is written in Python 3.6 with Postgres 10 as the chosen data persistence. Current default deployment medium is Heroku or Zeit but it can be deployed with services like AWS, Google Cloud, or DigitalOcean with Gunicorn and Nginx.

## Goal

The goal of this boilerplate is to allow developers to quickly write their API with code structured to best practices while giving them flexibility to easily add/change features. Core problem this is trying to solve:

1.  **Enforce standard for Flask development.** With Flask, you can write your application in any structure you like, even in one file. With a plethora of tutorials with different instructions & application structures available online this boilerplate sets a unifying guide across our own Flask services.

## Goal

Some basic environment setup can be done using `mac_setup.sh`.

## Usage

Here are some quickstart instructions, which presume pre-existing environment setup (e.g. full Docker setup, installed PostgreSQL instance, pipenv, etc).

First start a postgres docker container and persist the data with a volume `flask-app-db`:

```
make start_dev_db
```

Another option is to create a PostgreSQL instance on a cloud service like elephantsql and connect it to this app. Remember to change the postgres url and don't hard code it in!

Then, start your virtual environment

```
$ pip3 install virtualenv
$ virtualenv venv
$ source venv/bin/activate
```
Now, install the python dependencies and run the server:
```
(venv) $ pip install -r requirements.txt
(venv) $ pip install -r requirements-dev.txt
(venv) $ python manage.py recreate_db
(venv) $ python manage.py runserver
```

To exit the virtual environment:
```
(venv) $ deactivate
$
```

For ease of setup, I have hard-coded postgres URLs for development and docker configurations. If you are using a separate postgres instance as mentioned above, _do not hardcode_ the postgres url including the credentials to your code. Instead, create a file called `creds.ini` in the same directory level as `manage.py` and write something like this:

```
[pg_creds]
pg_url = postgresql://testusr:password@127.0.0.1:5432/testdb
```
Note: you will need to call `api.core.get_pg_url` in the Config file.

For production, you should do something similar with the flask `SECRET_KEY`.

#### Easier setup

This boilerplate includes a makefile to make this entire process easier. To use that option:
```
$ make setup
```

If you like to destroy your docker postgres database and start over, run:
```
$ make recreate_db
```
This is under the assumption that you have only set up one postgres container that's linked to the `flask-app-db` volume.

#### Deployment

With AWS being our likely target deployment environment these instructions are TBD.

### Repository Contents

- `api/views/` - Holds files that define your endpoints.
- `api/models/` - Holds files that defines your database schema.
- `api/__init__.py` - What is initially ran when you start your application.
- `api/utils.py` - Utility functions and classes.
- `api/core.py` - Includes core functionality including error handlers and logger.
- `api/wsgi.py` - App reference for gunicorn.
- `tests/` - Folder holding tests.

#### Others

- `config.py` - Provides Configuration for the application. There are two configurations: one for development and one for production using Heroku.
- `manage.py` - Command line interface that allows you to perform common functions with a command.
- `requirements.txt` - A list of python package dependencies the application requires as a baseline.
- `runtime.txt` & `Procfile` - Configuration for Heroku.
- `Dockerfile` - Instructions for Docker to build the Flask app.
- `docker-compose.yml` - Config to setup this Flask app and a Database.
- `migrations/` - Holds migration files – doesn't exist until you `python manage.py db init` if you decide to not use docker.

### MISC

To remove the **pycache** files:

```
find . | grep -E "(__pycache__|\.pyc|\.pyo$)" | xargs rm -rf
```

### Additional Documentation

- [Flask](http://flask.pocoo.org/) - Flask Documentation.
- [Flask Tutorial](http://flask.pocoo.org/docs/1.0/tutorial/) - Source of many Flak patterns.
- [Flask SQLAlchemy](http://flask-sqlalchemy.pocoo.org/2.3/) - The ORM for the database.
- [Heroku](https://devcenter.heroku.com/articles/getting-started-with-python#introduction) - Deployment using Heroku.
- [Learn Python](https://www.learnpython.org/) - Learning Python3.
- [Relational Databases](https://www.ntu.edu.sg/home/ehchua/programming/sql/Relational_Database_Design.html) - Designing a database schema.
- [REST API](http://www.restapitutorial.com/lessons/restquicktips.html) - Making an API RESTful.
- [Docker Docs](https://docs.docker.com/get-started/) - Docker documentation.
