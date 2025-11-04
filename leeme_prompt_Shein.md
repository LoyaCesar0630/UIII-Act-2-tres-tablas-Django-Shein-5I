UIII_Shein_0630/
│
├── backend_Shein/                        # Carpeta principal del proyecto Django
│   ├── __init__.py                       # Indica que esta carpeta es un paquete Python
│   ├── settings.py                       # Configuración principal de Django (base de datos, rutas, etc.)
│   ├── urls.py                           # Enlace de las URLs de la aplicación con el proyecto principal
│   ├── asgi.py                           # Configuración para ASGI (Asynchronous Server Gateway Interface)
│   ├── wsgi.py                           # Configuración para WSGI (Web Server Gateway Interface)
│   └── manage.py                         # Comando principal para interactuar con el proyecto Django
│
├── app_Shein/                            # Aplicación principal dentro del proyecto
│   ├── migrations/                       # Carpeta donde Django guarda los archivos de migración de la base de datos
│   │   └── __init__.py                   # Indica que esta carpeta es un paquete Python
│   ├── templates/                        # Carpeta que contiene las plantillas HTML
│   │   ├── base.html                     # Plantilla base que contiene la estructura común de la web (cabecera, pie, etc.)
│   │   ├── header.html                   # Contiene el encabezado con elementos como el logo y título
│   │   ├── navbar.html                   # Barra de navegación con enlaces a las diferentes secciones
│   │   ├── footer.html                   # Pie de página con los derechos de autor
│   │   ├── inicio.html                   # Página de inicio con información sobre el sistema
│   │   └── usuarios/                     # Subcarpeta que contiene las plantillas para gestión de usuarios
│   │       ├── agregar_usuario.html      # Formulario para agregar un nuevo usuario
│   │       ├── ver_usuarios.html         # Vista para mostrar todos los usuarios
│   │       ├── actualizar_usuario.html   # Formulario para actualizar un usuario existente
│   │       └── borrar_usuario.html       # Confirmación para borrar un usuario
│   ├── admin.py                          # Configuración del panel de administración de Django para gestionar modelos
│   ├── apps.py                           # Configuración de la aplicación dentro de Django
│   ├── models.py                         # Definición de los modelos de datos para la base de datos
│   ├── views.py                          # Funciones de vista que gestionan las operaciones CRUD
│   ├── urls.py                           # Archivo que contiene las rutas para las vistas de usuarios
│   └── __init__.py                       # Indica que esta carpeta es un paquete Python
│
└── .venv/                                # Carpeta que contiene el entorno virtual con las dependencias del proyecto
    └── ...                               # Archivos y carpetas del entorno virtual de Python
