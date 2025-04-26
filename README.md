# Django First App: Tu primera app web usando Django (Hola Mundo + CRUD de Estudiantes)

## Parte 1: Hola Mundo en Django

### 1. Creación de un entorno virtual
**(Windows)**

- Abre el **CMD** y ubícate en la carpeta donde quieres trabajar.
- Ejecuta:
  ```bash
  python -m venv venv
  ```
- Activa el entorno:
  ```bash
  venv\Scripts\activate
  ```

> **Nota:** Es recomendable nombrar tu entorno como `venv` o `env`. También puedes activar usando `venv\Scripts\activate.bat`.

---

### 2. Instalación de Django
Instala el framework ejecutando:
```bash
pip install django
```

---

### 3. Creación de un proyecto
Crea un nuevo proyecto llamado `base`:
```bash
django-admin startproject base
cd base
```

---

### 4. Creación de una aplicación
Dentro del proyecto, crea una aplicación llamada `myapp`:
```bash
python manage.py startapp myapp
```

Registra tu app en `base/settings.py`:
```python
INSTALLED_APPS = [
    ...
    'myapp',  # <- Añade aquí tu nueva app
]
```

---

### 5. Crear una vista "Hola Mundo"
En `myapp/views.py`, crea esta vista básica:

```python
from django.http import HttpResponse

def hola_mundo(request):
    return HttpResponse("Hola mundo con django")
```

Ahora configura una URL para esta vista.  
En `base/urls.py`:

```python
from django.contrib import admin
from django.urls import path
from myapp.views import hola_mundo

urlpatterns = [
    path('admin/', admin.site.urls),
    path('hola/', hola_mundo),
]
```

Levanta el servidor:

```bash
python manage.py runserver
```

Visita en tu navegador:  
`http://127.0.0.1:8000/hola/`




## Parte 2: Mini Proyecto de Estudiantes (con HTML y Admin Panel)

### 6. Crear el Modelo Estudiante
En `myapp/models.py`:

```python
from django.db import models

class Estudiante(models.Model):
    nombre = models.CharField(max_length=100)
    edad = models.IntegerField()
    correo = models.EmailField()

    def __str__(self):
        return self.nombre
```


### 7. Migrar el modelo a la base de datos
Desde consola:

```bash
python manage.py makemigrations myapp
python manage.py migrate
```

> **Tip:** Puedes ver la base de datos `db.sqlite3` creada automáticamente


### 8. Registrar el Modelo en el Admin
En `myapp/admin.py`:

```python
from django.contrib import admin
from .models import Estudiante

admin.site.register(Estudiante)
```


### 9. Crear superusuario para acceder al Admin
Detén el servidor (`Ctrl + C`) y ejecuta:

```bash
python manage.py createsuperuser
```

Llena los datos solicitados: usuario, correo y contraseña.


### 10. Levantar servidor y entrar al Admin
Corre de nuevo el servidor:

```bash
python manage.py runserver
```

Visita:  
 `http://127.0.0.1:8000/admin/`

Inicia sesión con tu usuario.  
Ahora puedes **crear, editar y eliminar estudiantes** desde el Admin Panel.


### 11. Crear una vista para listar estudiantes
En `myapp/views.py`, añade:

```python
from django.shortcuts import render
from .models import Estudiante

def listar_estudiantes(request):
    estudiantes = Estudiante.objects.all()
    return render(request, 'listar_estudiantes.html', {'estudiantes': estudiantes})
```


### 12. Crear plantilla HTML
En tu app `myapp/`, crea la carpeta `templates/` y dentro crea `listar_estudiantes.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Lista de Estudiantes</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 50%; border-collapse: collapse; }
        th, td { border: 1px solid black; padding: 8px; text-align: left; }
    </style>
</head>
<body>
    <h1>Lista de Estudiantes</h1>
    <table>
        <tr>
            <th>Nombre</th>
            <th>Edad</th>
            <th>Correo</th>
        </tr>
        {% for estudiante in estudiantes %}
        <tr>
            <td>{{ estudiante.nombre }}</td>
            <td>{{ estudiante.edad }}</td>
            <td>{{ estudiante.correo }}</td>
        </tr>
        {% empty %}
        <tr>
            <td colspan="3">No hay estudiantes registrados.</td>
        </tr>
        {% endfor %}
    </table>
</body>
</html>
```


### 13. Configurar la URL para la nueva vista
En `base/urls.py`:

```python
from myapp.views import listar_estudiantes

urlpatterns = [
    path('admin/', admin.site.urls),
    path('hola/', hola_mundo),
    path('estudiantes/', listar_estudiantes),
]
```

Ahora en tu navegador ve a:  
`http://127.0.0.1:8000/estudiantes/`

Verás una **tabla bonita** listando los estudiantes creados.
