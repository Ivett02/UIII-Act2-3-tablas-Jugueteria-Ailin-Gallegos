🎠 Proyecto: Juguetería 0494

Lenguaje: Python
Framework: Django
Editor: Visual Studio Code
Puerto del servidor: 8012
Creadora: Ailín Gallegos, CBTIS 128

📁 Estructura del Proyecto
    UIII_Jugueteria_0494/
    │
    ├── backend_Jugueteria/
    │   ├── __init__.py
    │   ├── asgi.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    │
    ├── app_Jugueteria/
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── models.py
    │   ├── urls.py
    │   ├── views.py
    │   └── templates/
    │       ├── base.html
    │       ├── header.html
    │       ├── navbar.html
    │       ├── footer.html
    │       ├── inicio.html
    │       └── sucursal/
    │           ├── agregar_sucursal.html
    │           ├── ver_sucursales.html
    │           ├── actualizar_sucursal.html
    │           └── borrar_sucursal.html
    │
    ├── manage.py
    └── .venv/

⚙️ Configuración del Proyecto Django
backend_Jugueteria/settings.py
    from pathlib import Path
    
    BASE_DIR = Path(__file__).resolve().parent.parent
    
    SECRET_KEY = 'django-insecure-clave-secreta'
    DEBUG = True
    ALLOWED_HOSTS = []
    
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'app_Jugueteria',
    ]
    
    MIDDLEWARE = [
        'django.middleware.security.SecurityMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.middleware.common.CommonMiddleware',
        'django.middleware.csrf.CsrfViewMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'django.contrib.messages.middleware.MessageMiddleware',
        'django.middleware.clickjacking.XFrameOptionsMiddleware',
    ]
    
    ROOT_URLCONF = 'backend_Jugueteria.urls'
    
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [BASE_DIR / 'app_Jugueteria' / 'templates'],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    
    WSGI_APPLICATION = 'backend_Jugueteria.wsgi.application'
    
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
        }
    }
    
    LANGUAGE_CODE = 'es-es'
    TIME_ZONE = 'America/Mexico_City'
    USE_I18N = True
    USE_TZ = True
    
    STATIC_URL = 'static/'
    DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
    
    backend_Jugueteria/urls.py
    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('app_Jugueteria.urls')),
    ]

🧠 Modelos de Datos
        app_Jugueteria/models.py
        
                from django.db import models
                # ==========================================
                # MODELO: SUCURSAL
                # ==========================================
                class Sucursal(models.Model):
                    nombre = models.CharField(max_length=100)
                    direccion = models.CharField(max_length=200)
                    ciudad = models.CharField(max_length=100)
                    estado = models.CharField(max_length=100)
                    telefono = models.CharField(max_length=20, blank=True, null=True)
                    codigo_postal = models.CharField(max_length=10, blank=True, null=True)
                    email = models.EmailField(blank=True, null=True)
                
                    def __str__(self):
                        return f"{self.nombre} - {self.ciudad}"
    
    # ==========================================
    # MODELO: CLIENTE
    # ==========================================
    class Cliente(models.Model):
        nombre = models.CharField(max_length=100)
        apellido = models.CharField(max_length=100)
        edad = models.PositiveIntegerField(blank=True, null=True)
        direccion = models.CharField(max_length=200, blank=True, null=True)
        telefono = models.CharField(max_length=20, blank=True, null=True)
        email = models.EmailField(blank=True, null=True)
        id_sucursal = models.ForeignKey(Sucursal, on_delete=models.CASCADE, related_name="clientes")
    
        def __str__(self):
            return f"{self.nombre} {self.apellido}"
    
    # ==========================================
    # MODELO: EMPLEADO
    # ==========================================
    class Empleado(models.Model):
        nombre = models.CharField(max_length=100)
        apellido = models.CharField(max_length=100)
        edad = models.PositiveIntegerField(blank=True, null=True)
        direccion = models.CharField(max_length=200, blank=True, null=True)
        puesto = models.CharField(max_length=100)
        salario = models.DecimalField(max_digits=10, decimal_places=2, blank=True, null=True)
        id_sucursal = models.ForeignKey(Sucursal, on_delete=models.CASCADE, related_name="empleados")
    
        def __str__(self):
            return f"{self.nombre} {self.apellido} - {self.puesto}"

🔧 Administración de Django
app_Jugueteria/admin.py
from django.contrib import admin
from .models import Sucursal, Cliente, Empleado

admin.site.register(Sucursal)
admin.site.register(Cliente)
admin.site.register(Empleado)

🧩 Vistas (Views)
app_Jugueteria/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Sucursal

def inicio_jugueteria(request):
    return render(request, 'inicio.html')

def ver_sucursales(request):
    sucursales = Sucursal.objects.all()
    return render(request, 'sucursal/ver_sucursales.html', {'sucursales': sucursales})

def agregar_sucursal(request):
    if request.method == 'POST':
        Sucursal.objects.create(
            nombre=request.POST['nombre'],
            direccion=request.POST['direccion'],
            ciudad=request.POST['ciudad'],
            estado=request.POST['estado'],
            telefono=request.POST['telefono'],
            codigo_postal=request.POST['codigo_postal'],
            email=request.POST['email']
        )
        return redirect('ver_sucursales')
    return render(request, 'sucursal/agregar_sucursal.html')

def actualizar_sucursal(request, id):
    sucursal = get_object_or_404(Sucursal, id=id)
    return render(request, 'sucursal/actualizar_sucursal.html', {'sucursal': sucursal})

def realizar_actualizacion_sucursal(request, id):
    sucursal = get_object_or_404(Sucursal, id=id)
    sucursal.nombre = request.POST['nombre']
    sucursal.direccion = request.POST['direccion']
    sucursal.ciudad = request.POST['ciudad']
    sucursal.estado = request.POST['estado']
    sucursal.telefono = request.POST['telefono']
    sucursal.codigo_postal = request.POST['codigo_postal']
    sucursal.email = request.POST['email']
    sucursal.save()
    return redirect('ver_sucursales')

def borrar_sucursal(request, id):
    sucursal = get_object_or_404(Sucursal, id=id)
    sucursal.delete()
    return redirect('ver_sucursales')

🌐 Enrutamiento
app_Jugueteria/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_jugueteria, name='inicio'),
    path('sucursales/', views.ver_sucursales, name='ver_sucursales'),
    path('sucursales/agregar/', views.agregar_sucursal, name='agregar_sucursal'),
    path('sucursales/actualizar/<int:id>/', views.actualizar_sucursal, name='actualizar_sucursal'),
    path('sucursales/realizar_actualizacion/<int:id>/', views.realizar_actualizacion_sucursal, name='realizar_actualizacion_sucursal'),
    path('sucursales/borrar/<int:id>/', views.borrar_sucursal, name='borrar_sucursal'),
]

🎨 Plantillas HTML

Se utiliza Bootstrap 5, colores suaves y diseño moderno.

templates/base.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juguetería 0494</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">

    {% include 'header.html' %}
    {% include 'navbar.html' %}

    <main class="container mt-4 mb-5">
        {% block content %}{% endblock %}
    </main>

    {% include 'footer.html' %}

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>

templates/header.html
<header class="bg-primary text-white text-center py-3 shadow-sm">
    <h1>Sistema de Administración Juguetería 0494</h1>
</header>

templates/navbar.html
<nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
  <div class="container">
    <a class="navbar-brand text-primary fw-bold" href="/">🏠 Inicio</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">🏢 Sucursales</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_sucursal' %}">Agregar Sucursal</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_sucursales' %}">Ver Sucursales</a></li>
          </ul>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">👥 Clientes</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Cliente</a></li>
            <li><a class="dropdown-item" href="#">Ver Clientes</a></li>
          </ul>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">👔 Empleados</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Empleado</a></li>
            <li><a class="dropdown-item" href="#">Ver Empleados</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>

templates/footer.html
<footer class="bg-dark text-white text-center py-3 fixed-bottom">
    <p>© {{ now|date:"Y" }} Creado por Ailin Gallegos, Cbtis 128</p>
</footer>

templates/inicio.html
{% extends 'base.html' %}
{% block content %}
<div class="text-center">
    <h2 class="text-primary mb-4">Bienvenido al Sistema de Administración de la Juguetería 0494</h2>
    <p class="lead">Administra fácilmente tus sucursales, clientes y empleados.</p>
    <img src="https://cdn.pixabay.com/photo/2016/03/31/19/14/toys-1291369_1280.png" 
         alt="Juguetería" class="img-fluid mt-3 rounded shadow">
</div>
{% endblock %}

templates/sucursal/agregar_sucursal.html
{% extends 'base.html' %}
{% block content %}
<h3 class="text-center text-primary">Agregar Sucursal</h3>
<form method="POST" class="card p-4 shadow-sm bg-white">
    {% csrf_token %}
    <input name="nombre" class="form-control mb-2" placeholder="Nombre" required>
    <input name="direccion" class="form-control mb-2" placeholder="Dirección" required>
    <input name="ciudad" class="form-control mb-2" placeholder="Ciudad" required>
    <input name="estado" class="form-control mb-2" placeholder="Estado" required>
    <input name="telefono" class="form-control mb-2" placeholder="Teléfono">
    <input name="codigo_postal" class="form-control mb-2" placeholder="Código Postal">
    <input name="email" class="form-control mb-2" placeholder="Correo electrónico">
    <button class="btn btn-success">Guardar</button>
</form>
{% endblock %}

templates/sucursal/ver_sucursales.html
{% extends 'base.html' %}
{% block content %}
<h3 class="text-center text-primary mb-3">Listado de Sucursales</h3>

<table class="table table-striped table-hover shadow-sm bg-white">
    <thead class="table-primary">
        <tr>
            <th>Nombre</th><th>Ciudad</th><th>Estado</th><th>Teléfono</th><th>Correo</th><th>Acciones</th>
        </tr>
    </thead>
    <tbody>
        {% for sucursal in sucursales %}
        <tr>
            <td>{{ sucursal.nombre }}</td>
            <td>{{ sucursal.ciudad }}</td>
            <td>{{ sucursal.estado }}</td>
            <td>{{ sucursal.telefono }}</td>
            <td>{{ sucursal.email }}</td>
            <td>
                <a href="{% url 'actualizar_sucursal' sucursal.id %}" class="btn btn-sm btn-warning">Editar</a>
                <a href="{% url 'borrar_sucursal' sucursal.id %}" class="btn btn-sm btn-danger">Borrar</a>
            </td>
        </tr>
        {% empty %}
        <tr><td colspan="6" class="text-center">No hay sucursales registradas.</td></tr>
        {% endfor %}
    </tbody>
</table>
{% endblock %}

templates/sucursal/actualizar_sucursal.html
{% extends 'base.html' %}
{% block content %}
<h3 class="text-center text-primary">Actualizar Sucursal</h3>
<form method="POST" action="{% url 'realizar_actualizacion_sucursal' sucursal.id %}" class="card p-4 shadow-sm bg-white">
    {% csrf_token %}
    <input name="nombre" value="{{ sucursal.nombre }}" class="form-control mb-2">
    <input name="direccion" value="{{ sucursal.direccion }}" class="form-control mb-2">
    <input name="ciudad" value="{{ sucursal.ciudad }}" class="form-control mb-2">
    <input name="estado" value="{{ sucursal.estado }}" class="form-control mb-2">
    <input name="telefono" value="{{ sucursal.telefono }}" class="form-control mb-2">
    <input name="codigo_postal" value="{{ sucursal.codigo_postal }}" class="form-control mb-2">
    <input name="email" value="{{ sucursal.email }}" class="form-control mb-2">
    <button class="btn btn-primary">Actualizar</button>
</form>
{% endblock %}

templates/sucursal/borrar_sucursal.html
{% extends 'base.html' %}
{% block content %}
<h3 class="text-center text-danger">Borrar Sucursal</h3>
<p class="text-center">¿Seguro que deseas eliminar esta sucursal?</p>
<form method="POST" class="text-center">
    {% csrf_token %}
    <button class="btn btn-danger">Confirmar</button>
    <a href="{% url 'ver_sucursales' %}" class="btn btn-secondary">Cancelar</a>
</form>
{% endblock %}

🚀 Ejecución del Proyecto
python manage.py makemigrations
python manage.py migrate
python manage.py runserver 8012


📍 Abre en tu navegador:
👉 http://127.0.0.1:8012/

💡 Características

✅ CRUD funcional de sucursales
✅ Bootstrap 5 integrado
✅ Diseño moderno y colores suaves
✅ Navbar, footer fijo y encabezado con íconos
✅ Proyecto completamente funcional sin forms.py
✅ Base para ampliar con Clientes y Empleados
