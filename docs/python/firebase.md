# [Python Firebase SDK](https://firebase.google.com/docs/reference/admin/python/)

## Initialize an app
```python
import os
import firebase_admin
from firebase_admin import credentials
cred_file = os.environ.get("FIREBASE_CREDENTIALS")
database_url = os.environ.get("FIREBASE_DATABASE")
cred = credentials.Certificate(cred_file)
default_app = firebase_admin.initialize_app(cred, {
     'databaseURL': database_url
})
```

## Get Data
```python
from firebase_admin import db
watches = db.reference('/watches').get()
```

## Listen for events
```python
watches_ref = db.reference('/watches')
def process(event):
    print(event.event_type, event.data)
    
watches_ref.listen(process)
```

## Snnipets

* [Database queries](https://github.com/firebase/firebase-admin-python/blob/master/snippets/database/index.py)
