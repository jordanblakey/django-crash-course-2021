# Django Crash Course

## Getting Started

```sh
. django_env/bin/activate.fish # Use virtual environment
pip install django # Install Django module
pip install pylint # Install pylint module
python -m django --version # Confirm Django installation
python -m pylint --version # Confirm Pylint installation
django-admin startproject django_project # Scaffold Django project
sudo apt install tree # Install utility to graph directory trees
tree . -L 2 # See directory tree
```

## Project Structure

```sh
.
├── django_project
│   ├── asgi.py # ASGI (Asynchronous Server Gateway Interface)
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py # WSGI is the Web Server Gateway Interface
├── manage.py
└── README.md
```

## Django Commands

```sh
django-admin startproject <project-name> # Create a new project
python manage.py runserver # Start the development server
python manage.py startapp <app-name> # Create a new app
python manage.py createsuperuser # Create super user for admin panel

# When changes are made to models, Django can handle updating database
# schema using "migrations".
python manage.py makemigrations # Stage DB migrations
python manage.py showmigrations # List the DB migrations to be executed
python manage.py sqlmigrate blog 0001 # Show the SQL code for a migration
python manage.py migrate # Make pending DB migrations
python manage.py shell # Inter Django prompt
```

### Database Migration

Example SQL created in a migration (Adding a model in this case)

```sql
CREATE TABLE "blog_post" (
  "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
  "title" varchar(100) NOT NULL,
  "content" text NOT NULL,
  "date_posted" datetime NOT NULL,
  "author_id" integer NOT NULL REFERENCES "auth_user" ("id") DEFERRABLE INITIALLY DEFERRED);
COMMIT;
```

### SQLite3 Basics

```sh
.databases # Show attached databases
.show # Show global config
.tables # Show tables in DB
.mode <mode> # ascii column csv html insert line list quote tabs tcl
.separator <char> # Character to separate column values
.schema <table> # Show table definition
.quit # Exit sqlite3
PRAGMA table_info(blog_post) # Get column names and types in a compact format
SELECT * FROM <table> LIMIT 10; # Standard SQL query
```

### Django shell

With `python manage.py shell` it's easy to query the DB using models:

```python
>>> from blog.models import Post
>>> from django.contrib.auth.models import User
>>> User.objects.all()
>>> User.objects.first()
>>> User.objects.last()
>>> User.objects.filter(username="TestUser") # Returns a QuerySet object
>>> User.objects.filter(username="TestUser").first() # Returns the first raw User instance from the QuerySet
>>> user = User.objects.filter(username="TestUser").first()
>>> user
>>> user = User.objects.get(id=1)
>>> post_1 = Post(title="Blog 1", content="First Post Content!", author=user)
>>> Post.objects.all() # <QuerySet []>
>>> post_1.save()
>>> Post.objects.all() # <QuerySet [<Post: Post object(1)>]>
>>> user.post_set # Queryable RelatedManager object
>>> user.post_set.all() # <QuerySet [<Post: Blog 2>]>
>>> user.post_set.create(title='Blog 3', content='Third Post Content!')
```
