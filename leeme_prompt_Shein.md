1. Procedimiento para crear carpeta del Proyecto: UIII_Shein_0630

Abre la terminal en tu sistema y usa el siguiente comando para crear la carpeta:

mkdir UIII_Shein_0630

2. Procedimiento para abrir VS Code sobre la carpeta UIII_Shein_0630

Una vez que la carpeta esté creada, navega a ella y abre VS Code desde la terminal:

cd UIII_Shein_0630
code .

3. Procedimiento para abrir terminal en VS Code

Dentro de VS Code, ve a la barra superior y selecciona Terminal > New Terminal, o usa el atajo de teclado Ctrl + (tecla junto a 1 en la mayoría de teclados).

4. Procedimiento para crear carpeta entorno virtual .venv desde terminal de VS Code

En la terminal de VS Code, ejecuta el siguiente comando para crear un entorno virtual:

python -m venv .venv

5. Procedimiento para activar el entorno virtual

Para activar el entorno virtual, usa el siguiente comando según tu sistema operativo:

Windows:

.venv\Scripts\activate


MacOS/Linux:

source .venv/bin/activate

6. Procedimiento para activar el intérprete de Python

En VS Code, si ya tienes el entorno virtual activado, el editor debería reconocerlo automáticamente. Si no, ve a la barra de comandos (Ctrl + Shift + P) y busca Python: Select Interpreter, luego selecciona el intérprete correspondiente a tu entorno virtual .venv.

7. Procedimiento para instalar Django

Con el entorno virtual activado, instala Django utilizando pip:

pip install django

8. Procedimiento para crear proyecto backend_Shein sin duplicar carpeta

Para crear el proyecto en Django, usa el siguiente comando:

django-admin startproject backend_Shein .


El punto (.) al final asegura que se cree dentro de la carpeta actual, sin duplicar la estructura de carpetas.

9. Procedimiento para ejecutar servidor en el puerto 8036

Ejecuta el servidor de desarrollo de Django en el puerto 8036 usando este comando:

python manage.py runserver 8036

10. Procedimiento para copiar y pegar el link en el navegador

En la terminal aparecerá una URL, algo como:

Starting development server at http://127.0.0.1:8036/


Abre tu navegador y pega http://127.0.0.1:8036/ para ver la página de inicio de Django.

11. Procedimiento para crear aplicación app_Shein

Crea la aplicación dentro de tu proyecto Django usando este comando:

python manage.py startapp app_Shein

12. Modelo en models.py

El código que proporcionas para los modelos en models.py de la aplicación app_Shein es el siguiente:

from django.db import models

# Modelo: USUARIOS
class Usuarios(models.Model):
    nombre = models.CharField(max_length=255)
    apellido = models.CharField(max_length=255)
    correo_electronico = models.CharField(max_length=255, unique=True)
    numero_telefonico = models.CharField(max_length=20)
    id_usuario = models.IntegerField(primary_key=True)

# Modelo: PRODUCTOS
class Productos(models.Model):
    producto_id = models.IntegerField(primary_key=True)
    nombre_producto = models.CharField(max_length=255)
    descripcion = models.TextField(blank=True, null=True)
    precio = models.FloatField()
    cantidad = models.IntegerField()

# Modelo: PEDIDOS
class Pedidos(models.Model):
    id_pedido = models.IntegerField(primary_key=True)
    estado_pedido = models.CharField(max_length=50)
    direccion = models.CharField(max_length=255)
    fecha_pedido = models.DateTimeField()
    id_usuario = models.IntegerField()  # FK a Usuarios
    id_producto = models.IntegerField()  # FK a Productos

12.5. Procedimiento para realizar migraciones (makemigrations y migrate)

Realiza las migraciones para que Django cree las tablas correspondientes en la base de datos:

python manage.py makemigrations
python manage.py migrate

13. Trabajar con el modelo Usuarios

El primer modelo que debes trabajar es el de Usuarios. Asegúrate de que el código esté correctamente implementado en el archivo models.py.

14. Funciones en views.py de app_Shein

En el archivo views.py, crea las funciones correspondientes para las operaciones CRUD en Usuarios. Aquí hay un ejemplo básico:

from django.shortcuts import render, redirect
from .models import Usuarios

# Vista para ver todos los usuarios
def ver_usuarios(request):
    usuarios = Usuarios.objects.all()
    return render(request, 'usuarios/ver_usuarios.html', {'usuarios': usuarios})

# Vista para agregar un usuario
def agregar_usuario(request):
    if request.method == "POST":
        nombre = request.POST['nombre']
        apellido = request.POST['apellido']
        correo = request.POST['correo']
        telefono = request.POST['telefono']
        usuario = Usuarios(nombre=nombre, apellido=apellido, correo_electronico=correo, numero_telefonico=telefono)
        usuario.save()
        return redirect('ver_usuarios')
    return render(request, 'usuarios/agregar_usuario.html')

# Vista para actualizar un usuario
def actualizar_usuario(request, id_usuario):
    usuario = Usuarios.objects.get(id_usuario=id_usuario)
    if request.method == "POST":
        usuario.nombre = request.POST['nombre']
        usuario.apellido = request.POST['apellido']
        usuario.correo_electronico = request.POST['correo']
        usuario.numero_telefonico = request.POST['telefono']
        usuario.save()
        return redirect('ver_usuarios')
    return render(request, 'usuarios/actualizar_usuario.html', {'usuario': usuario})

# Vista para borrar un usuario
def borrar_usuario(request, id_usuario):
    usuario = Usuarios.objects.get(id_usuario=id_usuario)
    usuario.delete()
    return redirect('ver_usuarios')

15. Crear la carpeta templates dentro de app_Shein

Dentro de la carpeta app_Shein, crea la carpeta templates, así:

app_Shein/
    └── templates/

16. Crear los archivos HTML en templates

Dentro de la carpeta templates, crea los siguientes archivos:

base.html: Diseño base con Bootstrap.

header.html: Contiene la cabecera del sitio.

navbar.html: Barra de navegación con los enlaces solicitados.

footer.html: Contiene los derechos de autor y la información del creador.

inicio.html: Información general del sistema y una imagen de Shein.

17. Agregar Bootstrap en base.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Administración Shein</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    {% include 'header.html' %}
    {% include 'navbar.html' %}
    <div class="container mt-4">
        {% block content %}
        {% endblock %}
    </div>
    {% include 'footer.html' %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>

18. Crear navbar.html con opciones y submenús

El archivo navbar.html tendrá algo como esto:

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Sistema de Administración Shein</a>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav">
      <li class="nav-item">
        <a class="nav-link" href="#">Inicio</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Usuarios</a>
        <ul>
          <li><a href="#">Agregar Usuario</a></li>
          <li><a href="#">Ver Usuarios</a></li>
          <li><a href="#">Actualizar Usuario</a></li>
          <li><a href="#">Borrar Usuario</a></li>
        </ul>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Productos</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Pedidos</a>
      </li>
    </ul>
  </div>
</nav>

19. Incluir derechos de autor en footer.html
<footer class="fixed-bottom bg-light text-center p-3">
    <p>&copy; {{ current_year }} | Creado por Ing. Cesar Loya, Cbtis 128</p>
</footer> 
20. Crear inicio.html con información sobre el sistema y una imagen de Shein

Dentro del archivo inicio.html, puedes agregar un diseño básico con una imagen tomada desde la red y una pequeña descripción sobre el sistema. Aquí te dejo un ejemplo:

{% extends 'base.html' %}

{% block content %}
  <div class="jumbotron">
    <h1 class="display-4">Bienvenido al Sistema de Administración Shein</h1>
    <p class="lead">Este es un sistema de administración para gestionar usuarios, productos y pedidos de Shein.</p>
    <hr class="my-4">
    <p>En este sistema podrás agregar, actualizar, eliminar y ver detalles de usuarios, productos y pedidos.</p>
    <img src="https://static.shein.com/images/sheinlogo.png" alt="Shein Logo" class="img-fluid" style="max-width: 100%; height: auto;">
  </div>
{% endblock %}

21. Crear la subcarpeta usuarios dentro de app_Shein/templates

Dentro de app_Shein/templates, crea una subcarpeta llamada usuarios que contendrá los archivos HTML relacionados con la gestión de usuarios:

app_Shein/
    └── templates/
        └── usuarios/
            ├── agregar_usuario.html
            ├── ver_usuarios.html
            ├── actualizar_usuario.html
            └── borrar_usuario.html

22. Crear los archivos HTML correspondientes dentro de app_Shein/templates/usuarios
agregar_usuario.html
{% extends 'base.html' %}

{% block content %}
  <h2>Agregar Usuario</h2>
  <form method="POST">
    {% csrf_token %}
    <div class="form-group">
      <label for="nombre">Nombre:</label>
      <input type="text" class="form-control" id="nombre" name="nombre" required>
    </div>
    <div class="form-group">
      <label for="apellido">Apellido:</label>
      <input type="text" class="form-control" id="apellido" name="apellido" required>
    </div>
    <div class="form-group">
      <label for="correo">Correo Electrónico:</label>
      <input type="email" class="form-control" id="correo" name="correo" required>
    </div>
    <div class="form-group">
      <label for="telefono">Número Telefónico:</label>
      <input type="text" class="form-control" id="telefono" name="telefono" required>
    </div>
    <button type="submit" class="btn btn-primary">Agregar Usuario</button>
  </form>
{% endblock %}

ver_usuarios.html
{% extends 'base.html' %}

{% block content %}
  <h2>Ver Usuarios</h2>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Apellido</th>
        <th>Correo Electrónico</th>
        <th>Número Telefónico</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      {% for usuario in usuarios %}
        <tr>
          <td>{{ usuario.id_usuario }}</td>
          <td>{{ usuario.nombre }}</td>
          <td>{{ usuario.apellido }}</td>
          <td>{{ usuario.correo_electronico }}</td>
          <td>{{ usuario.numero_telefonico }}</td>
          <td>
            <a href="{% url 'actualizar_usuario' usuario.id_usuario %}" class="btn btn-warning btn-sm">Editar</a>
            <a href="{% url 'borrar_usuario' usuario.id_usuario %}" class="btn btn-danger btn-sm">Borrar</a>
          </td>
        </tr>
      {% endfor %}
    </tbody>
  </table>
{% endblock %}

actualizar_usuario.html
{% extends 'base.html' %}

{% block content %}
  <h2>Actualizar Usuario</h2>
  <form method="POST">
    {% csrf_token %}
    <div class="form-group">
      <label for="nombre">Nombre:</label>
      <input type="text" class="form-control" id="nombre" name="nombre" value="{{ usuario.nombre }}" required>
    </div>
    <div class="form-group">
      <label for="apellido">Apellido:</label>
      <input type="text" class="form-control" id="apellido" name="apellido" value="{{ usuario.apellido }}" required>
    </div>
    <div class="form-group">
      <label for="correo">Correo Electrónico:</label>
      <input type="email" class="form-control" id="correo" name="correo" value="{{ usuario.correo_electronico }}" required>
    </div>
    <div class="form-group">
      <label for="telefono">Número Telefónico:</label>
      <input type="text" class="form-control" id="telefono" name="telefono" value="{{ usuario.numero_telefonico }}" required>
    </div>
    <button type="submit" class="btn btn-primary">Actualizar Usuario</button>
  </form>
{% endblock %}

borrar_usuario.html
{% extends 'base.html' %}

{% block content %}
  <h2>¿Estás seguro de que deseas borrar este usuario?</h2>
  <p><strong>Nombre:</strong> {{ usuario.nombre }} {{ usuario.apellido }}</p>
  <p><strong>Correo:</strong> {{ usuario.correo_electronico }}</p>
  <p><strong>Teléfono:</strong> {{ usuario.numero_telefonico }}</p>
  <form method="POST">
    {% csrf_token %}
    <button type="submit" class="btn btn-danger">Borrar Usuario</button>
  </form>
{% endblock %}

23. No utilizar forms.py

En este caso, no necesitas crear formularios en un archivo forms.py. Estás manejando los formularios directamente en las vistas y plantillas HTML, como se muestra en los ejemplos anteriores.

24. Crear el archivo urls.py en app_Shein para operaciones CRUD de usuarios

Dentro de la carpeta de la aplicación app_Shein, crea un archivo llamado urls.py y agrega las rutas para las vistas correspondientes:

from django.urls import path
from . import views

urlpatterns = [
    path('', views.ver_usuarios, name='ver_usuarios'),
    path('agregar/', views.agregar_usuario, name='agregar_usuario'),
    path('actualizar/<int:id_usuario>/', views.actualizar_usuario, name='actualizar_usuario'),
    path('borrar/<int:id_usuario>/', views.borrar_usuario, name='borrar_usuario'),
]

25. Agregar app_Shein en settings.py de backend_Shein

En el archivo settings.py de backend_Shein, agrega app_Shein a la lista de INSTALLED_APPS:

INSTALLED_APPS = [
    ...
    'app_Shein',
]

26. Configurar las rutas en urls.py de backend_Shein

En backend_Shein/urls.py, enlaza las rutas de app_Shein con el proyecto principal:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('usuarios/', include('app_Shein.urls')),
    path('', include('app_Shein.urls')),  # Esto también puedes ponerlo si quieres que la página de inicio sea de `app_Shein`
]

27. Registrar los modelos en admin.py y realizar migraciones

Dentro de app_Shein/admin.py, registra el modelo Usuarios para que puedas gestionar los usuarios desde el panel de administración de Django:

from django.contrib import admin
from .models import Usuarios, Productos, Pedidos

admin.site.register(Usuarios)
admin.site.register(Productos)
admin.site.register(Pedidos)


Luego, realiza las migraciones nuevamente para asegurarte de que todos los cambios se reflejen en la base de datos:

python manage.py makemigrations
python manage.py migrate

28. Usar colores suaves, atractivos y modernos

Para que el diseño sea atractivo y moderno, asegúrate de usar clases de Bootstrap adecuadas y personalizar los estilos cuando sea necesario. Puedes modificar los archivos CSS o agregar un archivo personalizado para personalizar la paleta de colores y otros estilos visuales.

29. Crear la estructura completa de carpetas y archivos

Al finalizar los pasos anteriores, tu estructura de carpetas debe lucir así:

UIII_Shein_0630/
    ├── backend_Shein/
    ├── app_Shein/
    │   ├── migrations/
    │   ├── templates/
    │   │   ├── base.html
    │   │   ├── footer.html
    │   │   ├── header.html
    │   │   ├── navbar.html
    │   │   ├── inicio.html
    │   │   └── usuarios/
    │   │       ├── agregar_usuario.html
    │   │       ├── ver_usuarios.html
    │   │       ├── actualizar_usuario.html
    │   │      
