# Alembic

- init: it will create a directory named "alembic" contains all the needed configuration/code files for alembic
```
alembic init alembic
```
- configure sqlalchemy url
  - Change it in `alembic.ini`
  Or
  - Change the way it's read in `alembic/env.py` if you want to read it from environment variables or any other custom business logic you need.
  
- 
