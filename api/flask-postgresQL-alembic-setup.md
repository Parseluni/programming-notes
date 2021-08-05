Flask web application workflow documentation:  
https://flask.palletsprojects.com/en/1.1.x/  
https://flask.palletsprojects.com/en/1.1.x/#api-reference  

## Project Structure ##  
  
        .
        ├── app
        │   ├── models                  # holds data models
        │   │   └── __init__.py     
        │   ├── __init__.py             # holds the start-up logic for the Flask server (locates/applies any app configuration, and gets server ready to receive requests)      
        │   └── routes.py               # holds the endpoints
        ├── README.md
        └── requirements.txt

## Initial Set-up ##
  
1. Project setup (or fork/clone from github repo): 

       $ mkdir my-app-name
       $ cd my-app-name

2. Virtual environment:

       $ python3 -m venv venv
       $ source venv/bin/activate    
    
3. Install dependencies:   

       (venv) $ pip install flask flask-sqlalchemy flask-cors ariadne (and others as needed)
       (venv) $ pip freeze > requirements.txt
       
4. Create database using the Postgres interactive terminal as the user named postgres:      

       $ psql -U postgres
       $ CREATE DATABASE my_database_name;

5. Server actions:  

Building an API means that we're building a web server. A web server needs to be running in order to be accessible to clients. Running a web server makes it available to respond to HTTP requests at a particular address and port.
  
Command | Action
--- | ---
`(venv) $ flask run` | runs a Flask server
`(venv) $ FLASK_ENV=development flask run` | runs a Flask server in Debug mode so that we don't need to restart the server after each change 
`(venv) $ FLASK_ENV="development" && flask run` | starts Flask in development and debugging mode (only needed once)
`(venv) $ export FLASK_ENV=development` | switches to dev mode if Flask is already running (then type $ flask run again)
`ctrl` + `C` | stops a Flask server

## Database configuration ##  

We need to tell Flask where to find our new database. We do this by supplying a connection string that identifies where it is, and how to connect to it:  

       postgresql+psycopg2://postgres:postgres@localhost:5432/my_database_name

This tells Flask to connect to our database using the `psycopg2` package we installed from our `requirements.txt`. It connects using the `postgres` protocol using the `postgres` user on the local machine running at port `5432`.

1. Configure the connection string in `app/__init__.py` with the corresponding imports:  

        from flask import Flask
        from flask_sqlalchemy import SQLAlchemy
        from flask_migrate import Migrate     # companion package to SQLAlchemy

        # Sets up db and migrate, which are conventional variables that give us access to database operations
        db = SQLAlchemy()
        migrate = Migrate()

        def create_app(test_config=None):
            app = Flask(__name__)

            # Configures the app to include two new SQLAlchemy settings
            app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
            app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql+psycopg2://postgres:postgres@localhost:5432/my_database_name'

            # Connects db and migrate to our Flask app
            db.init_app(app)
            migrate.init_app(app, db)

            return app

2. Connect database to Flask (once we have created our database and configured the connection string, we can do this one-time setup command):  

       (venv) $ flask db init

## Models setup ##  

        .
        ├── app
        │   ├── models
        │   │   ├── __init__.py
        │   │   └── book.py
        │   ├── __init__.py
        │   └── routes.py
        ├── README.md
        └── requirements.txt
    
1. Create a file for the model `Book` following the pattern of creating a file for every model (see above):  

      (venv) $ touch app/models/book.py
      
2. Create a class for each model, here `Book`, to define the model's state and behavior:

        from app import db            

        class Book(db.Model):
            id = db.Column(db.Integer, primary_key=True, autoincrement=True)
            title = db.Column(db.String)
            description = db.Column(db.String)

The id, title and description attributes correspond to columns in the table. The id will be generated and incremented automatically. 

3. Make the `Book` model visible during the `app` start-up by adding this code to  `__init__.py` (it ensures that the Book model will be available to the app when we update our database next):

        def create_app(test_config=None):
                # app = Flask(__name__)
                # ... app is configured with SQLAlchemy settings
                # ... db and migrate are initialized with app

                # must place the following AFTER db and migrate are initialized, but BEFORE the return app statement:
                from app.models.book import Book

            return app

Further models will need to be imported here as well, following the pattern of `from app.models.model import Model`.

4. Update local database by generating and running the migration files with Alembic after each model change (the migration files are instructions on how to set up the structure of the database with tables, columns, their names, etc):  

       (venv) $ flask db migrate -m "descriptive message"
       (venv) $ flask db upgrade
    
The generated migrations are now placed in a new folder, the `migrations` folder:  

        .
        ├── app
        │   ├── models
        │   │   ├── __init__.py
        │   │   └── book.py
        │   ├── __init__.py
        │   └── routes.py
        ├── migrations
        │   ├── alembic.ini
        │   ├── env.py
        │   ├── README
        │   ├── script.py.mako
        │   └── versions
        ├── README.md
        └── requirements.txt
        
        
5. Confirm the Migration with `psql -U postgres`

Command | Action
--- | ---
`\c my_database_name` | reconnects to the database
`\dt` | lists the tables in the database (here book and alembic_version)
`\d book` | displays the columns of the book table (here id, title and description)
`ctrl` + `D`, `\q` + `Enter`, `exit` or `quit` | exits psql  

## Dev Workflow ##

1. `cd` into a project root folder
2. Activate a virtual environment  

       $ source venv/bin/activate 
        
3. Check git status/pull team commits
4. Start the server  

       (venv) $ flask run
        
5. Cycle frequently between:
    - writing code
    - checking git status and making git commits
    - debugging with Postman, server logs, VS Code, etc.
    - updating dependencies (after any updates to the requirements file):  
    
          (venv) $ pip freeze > requirements.txt
       
    - updating local database by generating and running the migration files with Alembic after each model change:  

          (venv) $ flask db migrate -m "descriptive message"
          (venv) $ flask db upgrade
              
6. Stop the server with `ctrl` + `C`
          
7. Deactivate the virtual environment  
            
       (venv) $ deactivate
