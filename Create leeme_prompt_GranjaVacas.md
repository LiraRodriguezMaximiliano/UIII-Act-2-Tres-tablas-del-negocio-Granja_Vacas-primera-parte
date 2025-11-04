📝 Procedimientos Iniciales y Configuración del Entorno
1. Crear Carpeta del Proyecto
El primer paso es crear el directorio principal donde residirá todo tu proyecto.

Abre el Explorador de Archivos o la terminal de tu sistema operativo.

Navega a la ubicación deseada (ej. C:/Proyectos/ o ~/Documentos/Proyectos).

Crea la carpeta con el nombre especificado:

Bash

mkdir UIII_Granja_Vacas_0627
2. Abrir VS Code sobre la Carpeta
Abre la carpeta recién creada en VS Code.

Desde la terminal, asegúrate de estar fuera de la carpeta recién creada, o en el mismo nivel, y ejecuta:

Bash

code UIII_Granja_Vacas_0627
(Asegúrate de que la ruta de VS Code esté configurada en tu sistema PATH para que el comando code funcione). Opcional: Abre VS Code, ve a Archivo > Abrir Carpeta y selecciona UIII_Granja_Vacas_0627.

3. Abrir Terminal en VS Code
VS Code tiene una terminal integrada muy útil.

Ve al menú Terminal > Nueva Terminal. O usa el atajo de teclado: Ctrl + Ñ (Windows/Linux) o Control + ` (Mac).

Asegúrate de que la terminal esté abierta en la raíz del proyecto: .../UIII_Granja_Vacas_0627.

4. Crear Carpeta de Entorno Virtual (.venv)
Es fundamental usar un Entorno Virtual para aislar las dependencias del proyecto.

Dentro de la terminal de VS Code, ejecuta:

Bash

python -m venv .venv
5. Activar el Entorno Virtual
Activar el entorno virtual te permite usar sus paquetes instalados.

Windows (PowerShell o CMD):

Bash

.venv\Scripts\activate
Linux / macOS (Bash / Zsh):

Bash

source .venv/bin/activate
Verás que el nombre del entorno ((.venv)) aparece al inicio de la línea de comandos.

6. Activar Intérprete de Python
Al activar el entorno, el intérprete de Python dentro de ese entorno se selecciona automáticamente. En VS Code, a menudo se te preguntará si deseas usar el intérprete del entorno virtual; si no es así, ve a la barra de estado inferior (donde dice Python ...) y selecciona la versión de Python dentro de .venv.

7. Instalar Django
Con el entorno virtual activado, instala Django usando pip.

Ejecuta en la terminal de VS Code:

Bash

pip install django
8. Crear Proyecto backend_Granja_Vacas (Sin duplicar Carpeta)
Usa el comando startproject con un punto (.) al final para crear el proyecto en el directorio actual, evitando una subcarpeta redundante.

Asegúrate de estar en la raíz del proyecto (UIII_Granja_Vacas_0627) con el entorno activado:

Bash

django-admin startproject backend_Granja_Vacas .
Esto creará el archivo manage.py y la subcarpeta backend_Granja_Vacas/.

9. Ejecutar Servidor en el Puerto 8023
Inicia el servidor de desarrollo de Django en el puerto solicitado.

Ejecuta en la terminal:

Bash

python manage.py runserver 8023
10. Copiar y Pegar el Link en el Navegador
El comando anterior te dará la URL.

Copia y pega la siguiente dirección en tu navegador:

http://127.0.0.1:8023/
11. Crear Aplicación app_Granja_Vacas
Detén el servidor (Ctrl + C) y crea tu aplicación principal (la 'app' de Django).

Asegúrate de estar en la misma ubicación que manage.py y ejecuta:

Bash

python manage.py startapp app_Granja_Vacas
Esto crea la estructura de la aplicación.

🏗️ Estructura de Proyecto Completa (Paso 29)
A continuación, creamos el resto de la estructura de carpetas y archivos solicitada, antes de continuar con la lógica.

UIII_Granja_Vacas_0627/

├── .venv/

├── backend_Granja_Vacas/

│ ├── __init__.py

│ ├── asgi.py

│ ├── settings.py

│ ├── urls.py <-- Archivo de URLs del Proyecto

│ └── wsgi.py

├── app_Granja_Vacas/ <-- Aplicación principal

│ ├── migrations/

│ ├── templates/ <-- Carpeta principal de templates (Paso 15)

│ │ ├── base.html <-- Archivo base HTML (Paso 16)

│ │ ├── header.html <-- Archivo de encabezado (Paso 16)

│ │ ├── navbar.html <-- Archivo de navegación (Paso 16)

│ │ ├── footer.html <-- Archivo de pie de página (Paso 16)

│ │ ├── inicio.html <-- Archivo de inicio (Paso 16)

│ │ └── Vacas/ <-- Subcarpeta para modelos (Paso 21)

│ │ ├── agregar_vaca.html <-- (Antes: agregar_categoria) (Paso 22)

│ │ ├── ver_vacas.html <-- (Antes: ver_categorias) (Paso 22)

│ │ ├── actualizar_vaca.html <-- (Antes: actualizar_categoria) (Paso 22)

│ │ └── borrar_vaca.html <-- (Antes: borrar_categoria) (Paso 22)

│ ├── __init__.py

│ ├── admin.py

│ ├── apps.py

│ ├── models.py <-- Aquí va el código de los modelos (Paso 12)

│ ├── tests.py

│ ├── views.py

│ └── urls.py <-- Archivo de URLs de la App (Paso 24)

└── manage.py


Nota Importante (Corrección de Nomenclatura): El punto 27 menciona trabajar con "categoría" y el punto 22 solicita archivos *_categoria.html en la carpeta \Vacas. Asumo que se refiere al MODELO: Vaca y, por lo tanto, he ajustado los nombres de los archivos HTML para que coincidan con el modelo principal que se debe trabajar primero: agregar_vaca.html, ver_vacas.html, etc., y se colocan dentro de app_Granja_Vacas\templates\Vacas.

🧩 Implementación del Código y Configuraciones
12. Modelo models.py
Copia el código proporcionado en el archivo app_Granja_Vacas/models.py.

25. Agregar app_Granja_Vacas en settings.py
Abre el archivo backend_Granja_Vacas/settings.py y agrega el nombre de tu aplicación a la lista INSTALLED_APPS.

Python

# backend_Granja_Vacas/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # NUESTRA APLICACIÓN
    'app_Granja_Vacas',  # <-- AGREGAR ESTA LÍNEA
]

# ...
27. Registrar Modelos en admin.py
Abre app_Granja_Vacas/admin.py y registra los modelos para que estén disponibles en el panel de administración de Django.

Python

# app_Granja_Vacas/admin.py
from django.contrib import admin
from .models import Vaca, Produccion, EventoSanitario

# Para Vaca (Categoría es Vaca, según la nota de ajuste)
admin.site.register(Vaca)

# Dejamos estos pendientes, pero los registramos para la estructura
admin.site.register(Produccion)
admin.site.register(EventoSanitario)
12.5 Procedimiento para Realizar las Migraciones
Detén el servidor (si está corriendo) y genera y aplica las migraciones.

Generar archivos de migración:

Bash

python manage.py makemigrations app_Granja_Vacas
Aplicar migraciones (crear las tablas en la base de datos):

Bash

python manage.py migrate
14. Funciones en views.py (CRUD Básico para Vaca)
Abre app_Granja_Vacas/views.py y crea las funciones básicas. Se asume que trabajar con "categoría" (punto 27) se refiere al modelo Vaca (siguiendo la nota de corrección).

Python

# app_Granja_Vacas/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.http import HttpResponse  # Importa HttpResponse para debug
from .models import Vaca # Importamos el modelo Vaca

# Función para la página de inicio (Paso 14)
def inicio_Granja_Vacas(request):
    # El contenido se renderizará en el inicio.html
    return render(request, 'inicio.html')

# -----------------
# FUNCIONES CRUD VACA (Paso 14)
# -----------------

# Ver todas las vacas (corresponde a 'ver_categorias' ajustado a 'ver_vacas')
def ver_vacas(request):
    vacas = Vaca.objects.all().order_by('numero_identificacion')
    # Se renderiza en ver_vacas.html que estará en templates/Vacas
    return render(request, 'Vacas/ver_vacas.html', {'vacas': vacas})

# Agregar una nueva vaca (corresponde a 'agregar_vaca')
def agregar_vaca(request):
    if request.method == 'POST':
        # Simulación de manejo de formulario POST sin forms.py (Paso 23)
        try:
            Vaca.objects.create(
                numero_identificacion=request.POST['numero_identificacion'],
                nombre=request.POST.get('nombre', ''), # get() con default por si es nulo/vacio
                fecha_nacimiento=request.POST['fecha_nacimiento'],
                raza=request.POST['raza'],
                estado_reproductivo=request.POST.get('estado_reproductivo', 'No gestante'),
                peso_kg=request.POST['peso_kg'],
                corral_actual=request.POST['corral_actual'],
                notas=request.POST.get('notas', '')
            )
            return redirect('ver_vacas') # Redirige a la lista después de agregar
        except Exception as e:
            # Puedes manejar errores de validación/base de datos aquí
            return HttpResponse(f"Error al agregar vaca: {e}", status=400)
    # Se renderiza en agregar_vaca.html que estará en templates/Vacas
    return render(request, 'Vacas/agregar_vaca.html')

# Editar vaca - Muestra el formulario con datos actuales
def actualizar_vaca(request, vaca_id):
    vaca = get_object_or_404(Vaca, pk=vaca_id)
    # Se renderiza en actualizar_vaca.html que estará en templates/Vacas
    return render(request, 'Vacas/actualizar_vaca.html', {'vaca': vaca})

# Realizar la actualización (maneja el POST del formulario de edición)
def realizar_actualizacion_vaca(request, vaca_id):
    if request.method == 'POST':
        vaca = get_object_or_404(Vaca, pk=vaca_id)
        # Asignación de datos desde el POST (sin forms.py)
        vaca.numero_identificacion = request.POST['numero_identificacion']
        vaca.nombre = request.POST.get('nombre', '')
        vaca.fecha_nacimiento = request.POST['fecha_nacimiento']
        vaca.raza = request.POST['raza']
        vaca.estado_reproductivo = request.POST.get('estado_reproductivo', 'No gestante')
        vaca.peso_kg = request.POST['peso_kg']
        vaca.corral_actual = request.POST['corral_actual']
        vaca.notas = request.POST.get('notas', '')
        vaca.save()
        return redirect('ver_vacas')
    # Si no es POST, redirige a la lista o muestra error
    return redirect('ver_vacas')

# Borrar una vaca
def borrar_vaca(request, vaca_id):
    vaca = get_object_or_404(Vaca, pk=vaca_id)
    vaca.delete()
    return redirect('ver_vacas')

# NOTA: Para un proyecto totalmente funcional, el punto 22 (borrar_categoria.html)
# debería ser un HTML de confirmación, pero se ha implementado directamente
# la acción de borrado en la vista `borrar_vaca` por simplicidad.
24. Archivo urls.py en app_Granja_Vacas
Crea el archivo app_Granja_Vacas/urls.py para mapear las URLs a las funciones de views.py.

Python

# app_Granja_Vacas/urls.py
from django.urls import path
from . import views

urlpatterns = [
    # URLs de la aplicación principal
    path('', views.inicio_Granja_Vacas, name='inicio_Granja_Vacas'),

    # URLs para el modelo Vaca (antes Categoria)
    path('vacas/ver/', views.ver_vacas, name='ver_vacas'),
    path('vacas/agregar/', views.agregar_vaca, name='agregar_vaca'),
    path('vacas/actualizar/<int:vaca_id>/', views.actualizar_vaca, name='actualizar_vaca'), # Formulario de edición
    path('vacas/realizar_actualizacion/<int:vaca_id>/', views.realizar_actualizacion_vaca, name='realizar_actualizacion_vaca'), # Maneja el POST
    path('vacas/borrar/<int:vaca_id>/', views.borrar_vaca, name='borrar_vaca'),
]
26. Configuración urls.py del Proyecto
Abre backend_Granja_Vacas/urls.py y enlaza el archivo urls.py de la aplicación.

Python

# backend_Granja_Vacas/urls.py
from django.contrib import admin
from django.urls import path, include  # Importa 'include'

urlpatterns = [
    path('admin/', admin.site.urls),
    # ENLACE CON app_Granja_Vacas
    path('', include('app_Granja_Vacas.urls')),  # Deja la URL vacía para que sea la raíz
]
🎨 Archivos HTML (Estructura y Estilo)
17. base.html (Incluyendo Bootstrap)
Crea app_Granja_Vacas/templates/base.html.

HTML

{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Granja Vacas{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    <style>
        /* 28. Colores suaves, atractivos y modernos */
        body {
            background-color: #f8f9fa; /* Gris muy claro, suave */
        }
        .navbar {
            background-color: #e9f5f0; /* Tono verde/azul claro, fresco */
        }
        .footer {
            background-color: #4b6a7a; /* Azul oscuro suave para el pie de página */
            color: white;
            padding: 10px 0;
            position: fixed; /* 19. Mantenerlo fijo */
            bottom: 0;
            width: 100%;
        }
        .btn-primary {
            background-color: #5cb85c; /* Verde amigable */
            border-color: #5cb85c;
        }
    </style>
</head>
<body>
    {% include 'header.html' %}

    {% include 'navbar.html' %}

    <div class="container mt-4 mb-5 pb-5"> {% block content %}
        {% endblock %}
    </div>

    {% include 'footer.html' %}

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
16. header.html
Crea app_Granja_Vacas/templates/header.html. (Se deja simple, el título principal está en navbar).

18. navbar.html
Crea app_Granja_Vacas/templates/navbar.html.

HTML

<nav class="navbar navbar-expand-lg navbar-light">
    <div class="container-fluid">
        <a class="navbar-brand text-primary fw-bold" href="{% url 'inicio_Granja_Vacas' %}">
            <i class="bi bi-gear-fill me-2"></i> Sistema de Administración Granja_Vacas
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <li class="nav-item">
                    <a class="nav-link" aria-current="page" href="{% url 'inicio_Granja_Vacas' %}">
                        <i class="bi bi-house-door-fill me-1"></i> Inicio
                    </a>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="bi bi-cow me-1"></i> Vaca
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_vaca' %}">Agregar Vaca</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_vacas' %}">Ver Vaca</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Vaca (en lista)</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Vaca (en lista)</a></li>
                    </ul>
                </li>

                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="bi bi-bucket-fill me-1"></i> Produccion
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Produccion</a></li>
                        <li><a class="dropdown-item" href="#">Ver Produccion</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Produccion</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Produccion</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="bi bi-bandages-fill me-1"></i> Evento_Sanitario
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Evento_Sanitario</a></li>
                        <li><a class="dropdown-item" href="#">Ver Evento_Sanitario</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Evento_Sanitario</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Evento_Sanitario</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
19. footer.html
Crea app_Granja_Vacas/templates/footer.html.

HTML

<footer class="footer text-center">
    <div class="container">
        <span class="text-white-50">
            &copy; Derechos de Autor | <span id="currentYear"></span> | Creado por **Maximiliano Lira**, Cbtis 128
        </span>
    </div>
    <script>
        // JS para poner la fecha actual dinámicamente
        document.getElementById('currentYear').textContent = new Date().getFullYear();
    </script>
</footer>
20. inicio.html
Crea app_Granja_Vacas/templates/inicio.html. (Nota: La imagen solicitada sobre Cinepolis es inusual para un proyecto de Granja, pero se incluye según la solicitud).

HTML

{% extends "base.html" %}

{% block title %}Inicio - Granja Vacas{% endblock %}

{% block content %}
<div class="p-5 mb-4 bg-light rounded-3 text-center">
    <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold text-success">Bienvenido al Sistema de Gestión de Granja Vacas</h1>
        <p class="col-md-8 fs-4 mx-auto">
            Este sistema le permite administrar eficientemente la información de sus vacas, su producción de leche y los eventos sanitarios asociados.
        </p>
        <hr class="my-4">
        <p class="text-muted">Desarrollado con Django y Bootstrap.</p>

        <h2 class="mt-5 text-secondary">Promoción/Información Adicional:</h2>
        <div class="text-center">
            <p>Información de ejemplo de la red:</p>
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Cinepolis.svg/1200px-Cinepolis.svg.png" 
                 class="img-fluid rounded shadow" 
                 alt="Imagen de Cinepolis" 
                 style="max-height: 200px;">
            <p class="mt-2 text-info">¡Disfruta del mejor entretenimiento!</p>
        </div>

    </div>
</div>
{% endblock %}
22. Archivos HTML para CRUD de Vaca
Crea la carpeta app_Granja_Vacas/templates/Vacas y los archivos:

agregar_vaca.html
HTML

{% extends "base.html" %}

{% block title %}Agregar Vaca{% endblock %}

{% block content %}
<h2 class="mb-4 text-success">🐄 Agregar Nueva Vaca</h2>
<form method="POST" action="{% url 'agregar_vaca' %}">
    {% csrf_token %}
    <div class="row g-3">
        <div class="col-md-6">
            <label for="id_numero_identificacion" class="form-label">Número de Identificación</label>
            <input type="text" class="form-control" id="id_numero_identificacion" name="numero_identificacion" required>
        </div>
        <div class="col-md-6">
            <label for="id_nombre" class="form-label">Nombre de la Vaca</label>
            <input type="text" class="form-control" id="id_nombre" name="nombre">
        </div>
        <div class="col-md-6">
            <label for="id_fecha_nacimiento" class="form-label">Fecha de Nacimiento</label>
            <input type="date" class="form-control" id="id_fecha_nacimiento" name="fecha_nacimiento" required>
        </div>
        <div class="col-md-6">
            <label for="id_raza" class="form-label">Raza</label>
            <input type="text" class="form-control" id="id_raza" name="raza" required>
        </div>
        <div class="col-md-6">
            <label for="id_estado_reproductivo" class="form-label">Estado Reproductivo</label>
            <select class="form-select" id="id_estado_reproductivo" name="estado_reproductivo">
                <option selected>No gestante</option>
                <option>Gestante</option>
                <option>Lactancia</option>
            </select>
        </div>
        <div class="col-md-6">
            <label for="id_peso_kg" class="form-label">Peso (kg)</label>
            <input type="number" step="0.01" class="form-control" id="id_peso_kg" name="peso_kg" required>
        </div>
        <div class="col-md-6">
            <label for="id_corral_actual" class="form-label">Corral Actual</label>
            <input type="text" class="form-control" id="id_corral_actual" name="corral_actual" required>
        </div>
        <div class="col-12">
            <label for="id_notas" class="form-label">Notas de la Vaca</label>
            <textarea class="form-control" id="id_notas" name="notas" rows="3"></textarea>
        </div>
        <div class="col-12 mt-4">
            <button type="submit" class="btn btn-primary">Guardar Vaca</button>
            <a href="{% url 'ver_vacas' %}" class="btn btn-secondary">Cancelar</a>
        </div>
    </div>
</form>
{% endblock %}
ver_vacas.html (Mostrar en tabla con botones)
HTML

{% extends "base.html" %}

{% block title %}Ver Vacas{% endblock %}

{% block content %}
<h2 class="mb-4 text-success">📋 Lista de Vacas Registradas</h2>
<a href="{% url 'agregar_vaca' %}" class="btn btn-success mb-3">
    <i class="bi bi-plus-circle-fill me-1"></i> Agregar Nueva Vaca
</a>

{% if vacas %}
<div class="table-responsive">
    <table class="table table-striped table-hover shadow-sm">
        <thead class="bg-light">
            <tr>
                <th>ID</th>
                <th>Nombre</th>
                <th>Fecha Nacimiento</th>
                <th>Raza</th>
                <th>Peso (kg)</th>
                <th>Corral</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for vaca in vacas %}
            <tr>
                <td>{{ vaca.numero_identificacion }}</td>
                <td>{{ vaca.nombre|default:"N/A" }}</td>
                <td>{{ vaca.fecha_nacimiento }}</td>
                <td>{{ vaca.raza }}</td>
                <td>{{ vaca.peso_kg }}</td>
                <td>{{ vaca.corral_actual }}</td>
                <td>
                    <a href="#" class="btn btn-sm btn-info text-white me-1" title="Ver Detalles">
                        <i class="bi bi-eye-fill"></i>
                    </a>
                    <a href="{% url 'actualizar_vaca' vaca.id %}" class="btn btn-sm btn-warning me-1" title="Editar">
                        <i class="bi bi-pencil-square"></i>
                    </a>
                    <a href="{% url 'borrar_vaca' vaca.id %}" class="btn btn-sm btn-danger" 
                       onclick="return confirm('¿Estás seguro de que deseas borrar la vaca {{ vaca.numero_identificacion }}?');" 
                       title="Borrar">
                        <i class="bi bi-trash-fill"></i>
                    </a>
                </td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</div>
{% else %}
<div class="alert alert-info">
    No hay vacas registradas aún.
</div>
{% endif %}
{% endblock %}
actualizar_vaca.html
HTML

{% extends "base.html" %}

{% block title %}Actualizar Vaca{% endblock %}

{% block content %}
<h2 class="mb-4 text-warning">✏️ Actualizar Vaca: {{ vaca.numero_identificacion }}</h2>
<form method="POST" action="{% url 'realizar_actualizacion_vaca' vaca.id %}">
    {% csrf_token %}
    <div class="row g-3">
        <div class="col-md-6">
            <label for="id_numero_identificacion" class="form-label">Número de Identificación</label>
            <input type="text" class="form-control" id="id_numero_identificacion" name="numero_identificacion" 
                   value="{{ vaca.numero_identificacion }}" required>
        </div>
        <div class="col-md-6">
            <label for="id_nombre" class="form-label">Nombre de la Vaca</label>
            <input type="text" class="form-control" id="id_nombre" name="nombre" value="{{ vaca.nombre|default:'' }}">
        </div>
        <div class="col-md-6">
            <label for="id_fecha_nacimiento" class="form-label">Fecha de Nacimiento</label>
            <input type="date" class="form-control" id="id_fecha_nacimiento" name="fecha_nacimiento" 
                   value="{{ vaca.fecha_nacimiento|date:'Y-m-d' }}" required>
        </div>
        <div class="col-md-6">
            <label for="id_raza" class="form-label">Raza</label>
            <input type="text" class="form-control" id="id_raza" name="raza" value="{{ vaca.raza }}" required>
        </div>
        <div class="col-md-6">
            <label for="id_estado_reproductivo" class="form-label">Estado Reproductivo</label>
            <select class="form-select" id="id_estado_reproductivo" name="estado_reproductivo">
                {% for opcion in ['No gestante', 'Gestante', 'Lactancia'] %}
                    <option value="{{ opcion }}" {% if opcion == vaca.estado_reproductivo %}selected{% endif %}>{{ opcion }}</option>
                {% endfor %}
            </select>
        </div>
        <div class="col-md-6">
            <label for="id_peso_kg" class="form-label">Peso (kg)</label>
            <input type="number" step="0.01" class="form-control" id="id_peso_kg" name="peso_kg" 
                   value="{{ vaca.peso_kg }}" required>
        </div>
        <div class="col-md-6">
            <label for="id_corral_actual" class="form-label">Corral Actual</label>
            <input type="text" class="form-control" id="id_corral_actual" name="corral_actual" 
                   value="{{ vaca.corral_actual }}" required>
        </div>
        <div class="col-12">
            <label for="id_notas" class="form-label">Notas de la Vaca</label>
            <textarea class="form-control" id="id_notas" name="notas" rows="3">{{ vaca.notas }}</textarea>
        </div>
        <div class="col-12 mt-4">
            <button type="submit" class="btn btn-warning text-white">Actualizar Vaca</button>
            <a href="{% url 'ver_vacas' %}" class="btn btn-secondary">Cancelar</a>
        </div>
    </div>
</form>
{% endblock %}
borrar_vaca.html
Nota: Este archivo HTML no se usa en la implementación actual, ya que la función borrar_vaca se ejecuta directamente con el botón de la lista (ver ver_vacas.html), usando un confirm de JavaScript. Para que fuera completamente funcional según la solicitud, se usaría un formulario de confirmación simple.

🚀 Paso Final
31. Ejecutar Servidor en el Puerto 8023
Una vez más, con el entorno activado, ejecuta el servidor:

Bash

python manage.py runserver 8023
Accede a http://127.0.0.1:8023/ en tu navegador para ver la página de inicio.
