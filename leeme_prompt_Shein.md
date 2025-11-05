Perfecto — te doy **un procedimiento completo, paso a paso (en español)** para crear el proyecto Django `UIII_Shein_0630` en VS Code, crear la app `app_Shein`, el modelo `Usuarios`, vistas CRUD (sin `forms.py`), plantillas con Bootstrap y todo lo necesario para ejecutar el servidor (al final incluyo cómo ejecutarlo en los puertos que mencionaste: `8036` y `8030`). Voy a seguir tu numeración y añadir el código necesario. ⚙️

> Nota rápida: hiciste dos solicitudes distintas para el puerto del servidor (paso 9 → **8036**, paso 31 → **8030**). Te muestro cómo ejecutar en **ambos** puertos; al final arrancamos en **8030** como pediste en el paso final.

---

# 1 — Crear la carpeta del proyecto `UIII_Shein_0630`

En tu sistema (explorer/terminal):

Windows (PowerShell):

```powershell
cd C:\ruta\donde\quieres\proyecto
mkdir UIII_Shein_0630
cd UIII_Shein_0630
```

Linux / macOS:

```bash
cd ~/ruta/donde/quieres/proyecto
mkdir UIII_Shein_0630
cd UIII_Shein_0630
```

---

# 2 — Abrir VS Code sobre la carpeta `UIII_Shein_0630`

En terminal:

```bash
code .
```

(Esto abrirá VS Code con la carpeta actual como workspace).

---

# 3 — Abrir terminal en VS Code

En VS Code: menú `Ver` → `Terminal` (o `Ctrl+ñ` / ``Ctrl+` `` según tu teclado). Aparecerá una terminal integrada (PowerShell en Windows o bash/zsh en macOS/Linux).

---

# 4 — Crear carpeta entorno virtual `.venv` desde terminal de VS Code

Dentro de la carpeta del proyecto en la terminal:

Windows (PowerShell):

```powershell
python -m venv .venv
```

Linux / macOS:

```bash
python3 -m venv .venv
```

Esto crea la carpeta `.venv` dentro de `UIII_Shein_0630`.

---

# 5 — Activar el entorno virtual

Windows (PowerShell):

```powershell
# Si PowerShell no permite ejecutar scripts, ejecutar como administrador o:
.\.venv\Scripts\Activate.ps1
# O con cmd:
.\.venv\Scripts\activate
```

Linux / macOS:

```bash
source .venv/bin/activate
```

Al activarlo verás el prompt con `(.venv)`.

---

# 6 — Activar intérprete de Python en VS Code

1. En VS Code: `Ctrl+Shift+P` → escribe `Python: Select Interpreter`.
2. Selecciona la ruta del intérprete dentro de tu `.venv` (algo como `.venv\Scripts\python.exe` o `./.venv/bin/python`).
   Esto asegura que VS Code use el entorno virtual.

---

# 7 — Instalar Django

Con el entorno activado:

```bash
pip install --upgrade pip
pip install django
```

Puedes comprobar versión:

```bash
python -m django --version
```

---

# 8 — Crear proyecto `backend_Shein` sin duplicar carpeta

Para evitar crear una carpeta dentro de otra (estructura limpia):

Dentro de `UIII_Shein_0630` (y con el venv activado), ejecuta:

```bash
django-admin startproject backend_Shein .
```

**Importante**: el punto final `.` indica crear el proyecto en la carpeta actual (evita crear `UIII_Shein_0630/backend_Shein/backend_Shein`). Ahora tendrás `manage.py` y la carpeta `backend_Shein/` con `settings.py`, `urls.py`, etc.

Estructura inicial esperada:

```
UIII_Shein_0630/
├─ .venv/
├─ manage.py
├─ backend_Shein/
│  ├─ __init__.py
│  ├─ settings.py
│  ├─ urls.py
│  └─ wsgi.py
```

---

# 9 — Ejecutar servidor en el puerto `8036`

Para arrancar en `8036`:

```bash
python manage.py runserver 0.0.0.0:8036
```

(ó `python manage.py runserver :8036` funciona también). Luego abrirás el link en el navegador (ver paso 10).

> Nota: más abajo te muestro cómo arrancar en `8030` (paso final).

---

# 10 — Copiar y pegar el link en el navegador

Cuando ejecutes `runserver` verás en la terminal algo como:

```
Starting development server at http://0.0.0.0:8036/
```

Copia `http://127.0.0.1:8036/` o `http://localhost:8036/` y pégalo en el navegador.

---

# 11 — Crear aplicación `app_Shein`

Desde la raíz del proyecto (donde está `manage.py`) y con venv activado:

```bash
python manage.py startapp app_Shein
```

Estructura:

```
UIII_Shein_0630/
├─ app_Shein/
│  ├─ migrations/
│  ├─ templates/
│  ├─ admin.py
│  ├─ apps.py
│  ├─ models.py
│  ├─ views.py
│  ├─ urls.py  # lo crearemos (ve paso 24)
```

---

# 12 — Aquí el `models.py` (solo **Usuarios** por ahora)

Como pediste que por ahora trabajemos solo con “usuarios” y dejar pendientes productos/pedidos, en `app_Shein/models.py` añade lo siguiente:

```python
from django.db import models

class Usuarios(models.Model):
    id_usuario = models.AutoField(primary_key=True)  # mejor AutoField que IntegerField para PK autoincremental
    nombre = models.CharField(max_length=255)
    Apellido = models.CharField(max_length=255)
    Correo_Electronico = models.CharField(max_length=255, unique=True)
    Numero_Telefonico = models.CharField(max_length=20)

    def __str__(self):
        return f"{self.nombre} {self.Apellido} - {self.Correo_Electronico}"
```

> Comentario: uso `AutoField` para `id_usuario` para que Django gestione la PK autoincremental. Si deseas explícitamente usar `IntegerField` y asignar manualmente, me lo indicas, pero la práctica estándar es `AutoField` o dejar que Django ponga `id`.

(Deja comentados los modelos de Productos y Pedidos para futura implementación o guárdalos en otro archivo como referencia).

---

# 12.5 — Procedimiento para realizar las migraciones (makemigrations y migrate)

1. Crear migraciones para `app_Shein`:

```bash
python manage.py makemigrations app_Shein
```

2. Aplicar migraciones:

```bash
python manage.py migrate
```

Si quieres crear superusuario para acceder al admin:

```bash
python manage.py createsuperuser
```

Sigue las indicaciones.

---

# 13 — Primero trabajamos con el MODELO: USUARIOS

Hecho: `models.py` contiene `Usuarios`. Continuamos con vistas y templates.

---

# 14 — En `views.py` de `app_Shein` crear funciones:

Abre `app_Shein/views.py` y pega lo siguiente (sin `forms.py`, uso `request.POST` crudo — sin validación como pediste):

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Usuarios
from django.utils import timezone

def inicio_cinepolis(request):
    # Página de inicio (puedes cambiar contenido)
    return render(request, 'inicio.html')

def agregar_usuario(request):
    if request.method == 'POST':
        nombre = request.POST.get('nombre')
        Apellido = request.POST.get('Apellido')
        Correo_Electronico = request.POST.get('Correo_Electronico')
        Numero_Telefonico = request.POST.get('Numero_Telefonico')

        Usuarios.objects.create(
            nombre=nombre,
            Apellido=Apellido,
            Correo_Electronico=Correo_Electronico,
            Numero_Telefonico=Numero_Telefonico
        )
        return redirect('ver_usuarios')

    return render(request, 'usuarios/agregar_usuario.html')

def ver_usuarios(request):
    usuarios = Usuarios.objects.all().order_by('id_usuario')
    return render(request, 'usuarios/ver_usuarios.html', {'usuarios': usuarios})

def actualizar_usuario(request, id_usuario):
    usuario = get_object_or_404(Usuarios, id_usuario=id_usuario)
    return render(request, 'usuarios/actualizar_usuario.html', {'usuario': usuario})

def realizar_actualizacion_usuario(request, id_usuario):
    usuario = get_object_or_404(Usuarios, id_usuario=id_usuario)
    if request.method == 'POST':
        usuario.nombre = request.POST.get('nombre')
        usuario.Apellido = request.POST.get('Apellido')
        usuario.Correo_Electronico = request.POST.get('Correo_Electronico')
        usuario.Numero_Telefonico = request.POST.get('Numero_Telefonico')
        usuario.save()
        return redirect('ver_usuarios')
    return redirect('actualizar_usuario', id_usuario=id_usuario)

def borrar_usuario(request, id_usuario):
    usuario = get_object_or_404(Usuarios, id_usuario=id_usuario)
    if request.method == 'POST':
        usuario.delete()
        return redirect('ver_usuarios')
    return render(request, 'usuarios/borrar_usuario.html', {'usuario': usuario})
```

Nombres de vistas (para usar en `urls.py`): `inicio_cinepolis`, `agregar_usuario`, `ver_usuarios`, `actualizar_usuario`, `realizar_actualizacion_usuario`, `borrar_usuario`.

---

# 15 — Crear la carpeta `templates` dentro de `app_Shein`

En `app_Shein/` crea carpeta `templates/`. Dentro de `templates/` crear archivos de layout y subcarpetas. Estructura:

```
app_Shein/
└─ templates/
   ├─ base.html
   ├─ header.html
   ├─ navbar.html
   ├─ footer.html
   ├─ inicio.html
   └─ usuarios/
      ├─ agregar_usuario.html
      ├─ ver_usuarios.html
      ├─ actualizar_usuario.html
      └─ borrar_usuario.html
```

---

# 16 — Crear archivos HTML (base, header, navbar, footer, inicio)

Te doy el código básico y sencillo, con Bootstrap incluido.

`app_Shein/templates/base.html`

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Sistema de Administración Shein</title>
    <!-- Bootstrap CSS (CDN) -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
  </head>
  <body class="d-flex flex-column min-vh-100">
    {% include 'navbar.html' %}
    <main class="container my-4">
      {% block content %}{% endblock %}
    </main>
    {% include 'footer.html' %}
    <!-- Bootstrap JS (popper + bundle) -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
  </body>
</html>
```

`app_Shein/templates/navbar.html`

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light shadow-sm">
  <div class="container">
    <a class="navbar-brand d-flex align-items-center" href="{% url 'inicio' %}">
      <svg width="28" height="28" class="me-2" viewBox="0 0 16 16" fill="currentColor" xmlns="http://www.w3.org/2000/svg"><rect width="100%" height="100%" rx="4" /></svg>
      Sistema de Administración Shein
    </a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navMenu">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navMenu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="{% url 'inicio' %}">Inicio</a></li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">Usuarios</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_usuario' %}">Agregar Usuario</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_usuarios' %}">Ver Usuarios</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#">Productos</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Producto</a></li>
            <li><a class="dropdown-item" href="#">Ver Productos</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#">Pedidos</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Pedido</a></li>
            <li><a class="dropdown-item" href="#">Ver Pedidos</a></li>
          </ul>
        </li>

      </ul>
    </div>
  </div>
</nav>
```

`app_Shein/templates/footer.html`

```html
<footer class="mt-auto bg-light text-center py-3 shadow-sm">
  <div class="container">
    <small>
      &copy; {{ now.year }} - Todos los derechos reservados. Creado por Ing. Cesar Loya, Cbtis 128
    </small>
  </div>
</footer>
```

> En las vistas, para que `now.year` funcione, en el contexto global puedes usar un `context_processor` o simplemente enviar la fecha desde la vista `inicio`. Para simplicidad, en `settings.py` habilitaremos el context processor `django.template.context_processors.request` (viene por defecto) y en cada template puedes usar `{% now "Y" as YEAR %}` si lo prefieres. Ejemplo simple: en `footer.html` cambia `{{ now.year }}` por `{% now "Y" %}`. (Más abajo usaré `{% now "Y" %}`).

Actualiza `footer.html` a:

```html
<footer class="mt-auto bg-light text-center py-3 shadow-sm">
  <div class="container">
    <small>
      &copy; {% now "Y" %} - Todos los derechos reservados. Creado por Ing. Cesar Loya, Cbtis 128
    </small>
  </div>
</footer>
```

`app_Shein/templates/inicio.html`

```html
{% extends 'base.html' %}
{% block content %}
<div class="row">
  <div class="col-md-8">
    <h1>Bienvenido al Sistema Shein</h1>
    <p>Sistema de administración sencillo para gestionar usuarios (primer módulo).</p>
  </div>
  <div class="col-md-4">
    <img src="https://images.unsplash.com/photo-1541099649105-f69ad21f3246?w=800&q=80" class="img-fluid rounded" alt="Shein">
  </div>
</div>
{% endblock %}
```

(La imagen enlazada es desde Unsplash, libre para pruebas; puedes cambiarla).

---

# 21 — Crear subcarpeta `usuarios` dentro de `app_Shein/templates`

Ya incluida en la estructura anterior: `app_Shein/templates/usuarios/`

---

# 22 — Archivos HTML para usuarios (agregar, ver, actualizar, borrar)

`app_Shein/templates/usuarios/agregar_usuario.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Agregar Usuario</h2>
<form method="post">
  {% csrf_token %}
  <div class="mb-3">
    <label class="form-label">Nombre</label>
    <input class="form-control" name="nombre" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Apellido</label>
    <input class="form-control" name="Apellido" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Correo Electrónico</label>
    <input class="form-control" name="Correo_Electronico" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Número Telefónico</label>
    <input class="form-control" name="Numero_Telefonico" required>
  </div>
  <button class="btn btn-primary" type="submit">Agregar</button>
  <a class="btn btn-secondary" href="{% url 'ver_usuarios' %}">Cancelar</a>
</form>
{% endblock %}
```

`app_Shein/templates/usuarios/ver_usuarios.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Usuarios</h2>
<a class="btn btn-success mb-3" href="{% url 'agregar_usuario' %}">+ Agregar Usuario</a>
<table class="table table-striped">
  <thead>
    <tr>
      <th>ID</th><th>Nombre</th><th>Apellido</th><th>Correo</th><th>Teléfono</th><th>Acciones</th>
    </tr>
  </thead>
  <tbody>
    {% for u in usuarios %}
    <tr>
      <td>{{ u.id_usuario }}</td>
      <td>{{ u.nombre }}</td>
      <td>{{ u.Apellido }}</td>
      <td>{{ u.Correo_Electronico }}</td>
      <td>{{ u.Numero_Telefonico }}</td>
      <td>
        <a class="btn btn-sm btn-info" href="{% url 'actualizar_usuario' u.id_usuario %}">Editar</a>
        <a class="btn btn-sm btn-danger" href="{% url 'borrar_usuario' u.id_usuario %}">Borrar</a>
      </td>
    </tr>
    {% empty %}
    <tr><td colspan="6">No hay usuarios.</td></tr>
    {% endfor %}
  </tbody>
</table>
{% endblock %}
```

`app_Shein/templates/usuarios/actualizar_usuario.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Actualizar Usuario</h2>
<form method="post" action="{% url 'realizar_actualizacion_usuario' usuario.id_usuario %}">
  {% csrf_token %}
  <div class="mb-3">
    <label class="form-label">Nombre</label>
    <input class="form-control" name="nombre" value="{{ usuario.nombre }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Apellido</label>
    <input class="form-control" name="Apellido" value="{{ usuario.Apellido }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Correo Electrónico</label>
    <input class="form-control" name="Correo_Electronico" value="{{ usuario.Correo_Electronico }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Número Telefónico</label>
    <input class="form-control" name="Numero_Telefonico" value="{{ usuario.Numero_Telefonico }}">
  </div>
  <button class="btn btn-primary" type="submit">Guardar</button>
  <a class="btn btn-secondary" href="{% url 'ver_usuarios' %}">Cancelar</a>
</form>
{% endblock %}
```

`app_Shein/templates/usuarios/borrar_usuario.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Confirmar eliminación</h2>
<p>¿Deseas eliminar a <strong>{{ usuario.nombre }} {{ usuario.Apellido }}</strong> ({{ usuario.Correo_Electronico }})?</p>
<form method="post">
  {% csrf_token %}
  <button class="btn btn-danger" type="submit">Sí, eliminar</button>
  <a class="btn btn-secondary" href="{% url 'ver_usuarios' %}">No, regresar</a>
</form>
{% endblock %}
```

---

# 23 — No utilizar `forms.py`

Cumplido: las vistas usan `request.POST` directamente y los templates HTML envían datos sin `forms.py`.

---

# 24 — Crear `urls.py` en `app_Shein` y añadir rutas

`app_Shein/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_cinepolis, name='inicio'),
    path('usuarios/agregar/', views.agregar_usuario, name='agregar_usuario'),
    path('usuarios/', views.ver_usuarios, name='ver_usuarios'),
    path('usuarios/actualizar/<int:id_usuario>/', views.actualizar_usuario, name='actualizar_usuario'),
    path('usuarios/realizar_actualizacion/<int:id_usuario>/', views.realizar_actualizacion_usuario, name='realizar_actualizacion_usuario'),
    path('usuarios/borrar/<int:id_usuario>/', views.borrar_usuario, name='borrar_usuario'),
]
```

---

# 25 — Agregar `app_Shein` en `settings.py` de `backend_Shein`

En `backend_Shein/settings.py`, dentro de `INSTALLED_APPS` añade `'app_Shein',` y asegúrate de que `django.template.context_processors` incluye los procesadores por defecto (viene por defecto).

Ejemplo:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    # ...
    'app_Shein',
]
```

También asegúrate de `TEMPLATES` tenga `'APP_DIRS': True,`.

---

# 26 — Configurar `backend_Shein/urls.py` para enlazar con `app_Shein`

En `backend_Shein/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Shein.urls')),  # rutas de la app
]
```

---

# 27 — Registrar los modelos en `admin.py` y volver a realizar migraciones

En `app_Shein/admin.py`:

```python
from django.contrib import admin
from .models import Usuarios

@admin.register(Usuarios)
class UsuariosAdmin(admin.ModelAdmin):
    list_display = ('id_usuario', 'nombre', 'Apellido', 'Correo_Electronico', 'Numero_Telefonico')
    search_fields = ('nombre', 'Apellido', 'Correo_Electronico')
```

Luego:

```bash
python manage.py makemigrations
python manage.py migrate
```

(ya hicimos migraciones antes, pero si agregaste cambios, ejecuta de nuevo).

---

# 27 (nota): Por ahora solo usuarios — Productos y Pedidos se dejan pendientes

He dejado los modelos de Productos y Pedidos para más adelante. Cuando quieras los añadimos (con `ForeignKey` adecuados y vistas).

---

# 28 — Estética: colores suaves y diseño sencillo

Los templates usan Bootstrap por defecto (paleta suave). Si quieres paleta personalizada, puedo darte un archivo CSS simple o clases Tailwind — pero pediste simplicidad, así que con Bootstrap queda moderno y suave.

---

# 29 — Al inicio crear la estructura completa de carpetas y archivos

Resumen estructura de carpetas que deberías tener completada:

```
UIII_Shein_0630/
├─ .venv/
├─ manage.py
├─ backend_Shein/
│  ├─ settings.py
│  └─ urls.py
├─ app_Shein/
│  ├─ migrations/
│  ├─ templates/
│  │  ├─ base.html
│  │  ├─ navbar.html
│  │  ├─ footer.html
│  │  ├─ inicio.html
│  │  └─ usuarios/
│  │     ├─ agregar_usuario.html
│  │     ├─ ver_usuarios.html
│  │     ├─ actualizar_usuario.html
│  │     └─ borrar_usuario.html
│  ├─ admin.py
│  ├─ apps.py
│  ├─ models.py
│  ├─ views.py
│  └─ urls.py
```

---

# 30 — Proyecto totalmente funcional

Con lo anterior, tu proyecto es funcional y permite:

* Abrir `/` → página de inicio.
* Añadir usuarios, ver usuarios, editar, eliminar.
* Acceso al admin (`/admin/`) con superusuario creado.

---

# 31 — Finalmente ejecutar servidor en el puerto `8030`

Para ejecutar en `8030` (como pediste al final):

```bash
python manage.py runserver 0.0.0.0:8030
```

Abre: `http://127.0.0.1:8030/` en tu navegador.

---

## Comandos útiles resumidos

```bash
# crear venv (ejemplo)
python -m venv .venv
# activar
source .venv/bin/activate   # linux/mac
# o Windows PowerShell
.\.venv\Scripts\Activate.ps1

pip install django

# crear proyecto en la carpeta actual (sin duplicar)
django-admin startproject backend_Shein .

# crear app
python manage.py startapp app_Shein

# migraciones
python manage.py makemigrations
python manage.py migrate

# crear superusuario
python manage.py createsuperuser

# ejecutar servidor (8036)
python manage.py runserver 0.0.0.0:8036

# ejecutar servidor (8030)
python manage.py runserver 0.0.0.0:8030
```

---

## Notas finales / recomendaciones

* No validamos datos por diseño (tal como pediste). Para producción **sí** habría que validar y sanitizar.
* Para mantener el footer siempre pegado al final se usa `min-vh-100` en `body` con diseño de bootstrap (ya lo incluí en `base.html`).
* Si quieres, te puedo:

  * Proveer el proyecto empaquetado en un .zip listo para abrir en VS Code.
  * Añadir las plantillas de Productos y Pedidos (cuando quieras).
  * Convertir los templates a un estilo más personalizado (colores específicos), o un CSS propio.
  * Añadir paginación, búsqueda o export de usuarios.

¿Quieres que te entregue ahora todos los archivos listos para descargar (zip) o prefieres que empiece creando los modelos Productos y Pedidos también?
