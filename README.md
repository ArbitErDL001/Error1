Excellent — that’s a complete transcript of a **Django REST Framework (DRF)** beginner tutorial.
Let’s turn it into a **reviewer-style handout** that’s clear, structured, and visually engaging — with **titles, code blocks, visuals, emojis, and short explanations per concept.**

---

# 🧠 Django REST Framework (DRF) — Quick Reviewer

## 📺 *Based on the tutorial video transcript (00:00–09:50)*

## 🎯 What You’ll Learn

✅ Build a simple API using Django REST Framework
✅ Create API endpoints for displaying and adding data
✅ Understand serializers and function-based API views

---

## ⚙️ 1. What is Django REST Framework?

* **Django** → Web framework for full-stack web apps.
* **Django REST Framework (DRF)** → Extension library that helps you build APIs easily.
* DRF is ideal for:

  * Building **public APIs**
  * Sharing data between your **own applications**

---

## 🏗️ 2. Project Setup

### 🧩 Install Django

```bash
pip install django
```

### 🧱 Create a New Project

```bash
django-admin startproject myproject
```

### ⚙️ Open in your text editor and install DRF

```bash
pip install djangorestframework
```

Then in `settings.py`, add this to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'rest_framework',
    'api',
]
```

---

## 📂 3. Create API Folder

Inside your project root, create a new folder named `api` and add:

```
api/
 ├── __init__.py
 ├── views.py
 └── urls.py
```

✅ The double underscores in `__init__.py` are required — `__init__.py`.

---

## 🧠 4. Create Views

### File: `api/views.py`

Import the required classes:

```python
from rest_framework.response import Response
from rest_framework.decorators import api_view
```

Define your first view:

```python
@api_view(['GET'])
def get_data(request):
    data = {'name': 'Arbiter', 'framework': 'Django REST Framework'}
    return Response(data)
```

---

## 🌐 5. Set Up URLs

### File: `api/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.get_data),
]
```

### Connect to main `urls.py`:

```python
from django.urls import path, include

urlpatterns = [
    path('api/', include('api.urls')),
]
```

---

## 🧩 6. Run the Server

```bash
python manage.py runserver
```

Open your browser:
➡️ `http://127.0.0.1:8000/api/`

You should see:

```
{
  "name": "Arbiter",
  "framework": "Django REST Framework"
}
```

💡 You can switch the format to **JSON** in the top right corner to see raw data.

---

## 🧱 7. Add a Model

### Create a new app:

```bash
python manage.py startapp base
```

### Register it:

In `settings.py`:

```python
INSTALLED_APPS = ['base', 'rest_framework']
```

### File: `base/models.py`

```python
from django.db import models

class Item(models.Model):
    name = models.CharField(max_length=100)
    created = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name
```

Run migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🧮 8. Add Sample Data

Open the Django shell:

```bash
python manage.py shell
```

Add some items:

```python
from base.models import Item
Item.objects.create(name="Item 1")
Item.objects.create(name="Item 2")
Item.objects.create(name="Item 3")
```

---

## 🔄 9. Create a Serializer

### File: `api/serializers.py`

```python
from rest_framework import serializers
from base.models import Item

class ItemSerializer(serializers.ModelSerializer):
    class Meta:
        model = Item
        fields = '__all__'
```

---

## 📡 10. Return Real Data

Update your `views.py`:

```python
from base.models import Item
from .serializers import ItemSerializer

@api_view(['GET'])
def get_data(request):
    items = Item.objects.all()
    serializer = ItemSerializer(items, many=True)
    return Response(serializer.data)
```

🧾 Output:

```json
[
  {"id": 1, "name": "Item 1", "created": "2025-11-13T00:00:00Z"},
  {"id": 2, "name": "Item 2", "created": "2025-11-13T00:01:00Z"},
  {"id": 3, "name": "Item 3", "created": "2025-11-13T00:02:00Z"}
]
```

---

## ➕ 11. Add Data via API (POST Request)

In `views.py`:

```python
@api_view(['POST'])
def add_item(request):
    serializer = ItemSerializer(data=request.data)
    if serializer.is_valid():
        serializer.save()
        return Response(serializer.data)
```

Add this route to `api/urls.py`:

```python
path('add/', views.add_item),
```

✅ Test it:
Use the DRF interface to send a `POST` request:

```json
{
  "name": "New Item"
}
```

Response:

```json
{
  "id": 4,
  "name": "New Item",
  "created": "2025-11-13T00:05:00Z"
}
```

---

## 🎉 12. Summary

| Step | Action               | Purpose               |
| ---- | -------------------- | --------------------- |
| 1    | Install Django & DRF | Setup framework       |
| 2    | Create Project & App | Organize code         |
| 3    | Create Views & URLs  | Handle requests       |
| 4    | Create Model         | Store data            |
| 5    | Make Serializers     | Convert model to JSON |
| 6    | Add Data             | Test database         |
| 7    | Create Endpoints     | GET & POST methods    |
| 8    | Test Output          | Verify JSON response  |

---

## 💡 Tips for Review

* Always register new apps in `settings.py`.
* Use serializers to transform data.
* Run `makemigrations` + `migrate` after editing models.
* `api_view(['GET'])` = read-only
* `api_view(['POST'])` = add data

---

## 📘 Practice Exercise

1. Add another field to the `Item` model (e.g., `description`).
2. Update the serializer to include it.
3. Try making a `PUT` request to update an existing item.

---

Would you like me to **turn this reviewer into a downloadable PDF** (with emojis, code blocks, and title styling like the previous one)?
