1. Crear carpeta del proyecto

En tu terminal o explorador:

mkdir UIII_Granja_Vacas_0627
cd UIII_Granja_Vacas_0627

2. Abrir VS Code sobre la carpeta

Opción GUI: abrir VS Code → File > Open Folder... → seleccionar UIII_Granja_Vacas_0627.

Opción terminal (si code está en PATH):

code .


(esto abre VS Code en la carpeta actual).

3. Abrir terminal en VS Code

En VS Code: menú View > Terminal o atajo Ctrl+ ` (Ctrl + backtick).

4. Crear entorno virtual .venv desde terminal de VS Code

En la terminal dentro de VS Code, en la carpeta del proyecto:

Windows:

python -m venv .venv


macOS / Linux:

python3 -m venv .venv


Esto crea la carpeta .venv dentro de UIII_Granja_Vacas_0627.

5. Activar el entorno virtual

Windows (PowerShell):

.venv\Scripts\Activate.ps1


si da error en PowerShell por ejecución de scripts:

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.venv\Scripts\Activate.ps1


Windows (cmd):

.venv\Scripts\activate


macOS / Linux:

source .venv/bin/activate


Al activarlo verás algo como (.venv) al inicio de la línea.

6. Activar intérprete de Python en VS Code

En VS Code: Ctrl+Shift+P → escribir Python: Select Interpreter → elegir el intérprete ubicado en UIII_Granja_Vacas_0627/.venv/... (ej. .venv\Scripts\python.exe o .venv/bin/python).

7. Instalar Django

Con el entorno activado:

pip install --upgrade pip
pip install django


(Esto instala la última versión compatible; si necesitás una versión específica: pip install django==4.2.7 por ejemplo.)

8. Crear proyecto backend_Granja_Vacas sin duplicar carpeta

Para evitar crear una subcarpeta duplicada, usa el comando startproject con el punto . o con la opción correcta:

Dentro de UIII_Granja_Vacas_0627 (con el venv activo):

django-admin startproject backend_Granja_Vacas .


IMPORTANTE: el . final indica “crear el proyecto en la carpeta actual” y evita anidar otra carpeta con el mismo nombre.

Estructura resultante mínima:

UIII_Granja_Vacas_0627/
  .venv/
  backend_Granja_Vacas/
    __init__.py
    settings.py
    urls.py
    wsgi.py
    asgi.py
  manage.py

9. Ejecutar servidor en el puerto 8023

Primero migraciones (ver punto 12), luego:

python manage.py runserver 8023


Esto inicia el servidor en http://127.0.0.1:8023/.

10. Copiar y pegar el link en el navegador

Abrir navegador y pegar:

http://127.0.0.1:8023/


(si usás otra IP, por ejemplo 0.0.0.0:8023 para acceso desde otras máquinas, adaptá la URL).

11. Crear aplicación app_Granja_Vacas

En la carpeta del proyecto:

python manage.py startapp app_Granja_Vacas


Estructura ahora:

app_Granja_Vacas/
  migrations/
  __init__.py
  admin.py
  apps.py
  models.py
  tests.py
  views.py

12. models.py (ya lo proporcionaste)

Pegá exactamente este código dentro de app_Granja_Vacas/models.py (ya lo incluiste, lo dejo tal cual):

from django.db import models

# =-=-=-=-=-
# MODELO: VACA
# =-=-=-=-=-
class Vaca(models.Model):
    numero_identificacion = models.CharField(max_length=20, unique=True, verbose_name="Número de Identificación") 
    nombre = models.CharField(max_length=100, blank=True, null=True, verbose_name="Nombre de la Vaca")
    fecha_nacimiento = models.DateField(verbose_name="Fecha de Nacimiento")
    raza = models.CharField(max_length=50, verbose_name="Raza") 
    estado_reproductivo = models.CharField(max_length=50, default="No gestante", verbose_name="Estado Reproductivo")
    peso_kg = models.DecimalField(max_digits=6, decimal_places=2, verbose_name="Peso (kg)")
    corral_actual = models.CharField(max_length=30, verbose_name="Corral Actual")
    notas = models.TextField(blank=True, verbose_name="Notas de la Vaca") 

    def __str__(self):
        return f"{self.nombre or 'Vaca'} {self.numero_identificacion}"

# =-=-=-=-=-
# MODELO: PRODUCCION (Relación 1:N con Vaca)
# =-=-=-=-=-
class Produccion(models.Model):
    vaca = models.ForeignKey(Vaca, on_delete=models.CASCADE, related_name='producciones', verbose_name="Vaca asociada") 
    fecha_registro = models.DateField(verbose_name="Fecha de Registro")
    cantidad_litros = models.DecimalField(max_digits=5, decimal_places=2, verbose_name="Cantidad (Litros)")
    turno = models.CharField(max_length=20, verbose_name="Turno")
    calidad = models.CharField(max_length=100, verbose_name="Detalles de Calidad") 
    empleado_registro = models.CharField(max_length=100, verbose_name="Empleado que Registra")
    equipo_usado = models.CharField(max_length=100, blank=True, verbose_name="Equipo Usado")

    def __str__(self):
        return f"Prod. {self.cantidad_litros}L de {self.vaca.numero_identificacion}"

# =-=-=-=-=-
# MODELO: EVENTOSANITARIO (Relación N:M con Vaca)
# =-=-=-=-=-
class EventoSanitario(models.Model):
    vacas_afectadas = models.ManyToManyField(Vaca, related_name='eventos_sanitarios', verbose_name="Vacas Afectadas") 
    tipo_evento = models.CharField(max_length=100, verbose_name="Tipo de Evento")
    fecha_evento = models.DateField(verbose_name="Fecha del Evento")
    tratamiento = models.TextField(verbose_name="Tratamiento Aplicado")
    veterinario = models.CharField(max_length=100, verbose_name="Veterinario Responsable")
    costo = models.DecimalField(max_digits=7, decimal_places=2, default=0.00, verbose_name="Costo Total (€/$)")
    dias_retiro = models.IntegerField(default=0, verbose_name="Días de Retiro")

    def __str__(self):
        return f"{self.tipo_evento} el {self.fecha_evento}"


Según tu indicación, por ahora trabajaremos solo con Vaca (dejamos Produccion y EventoSanitario para después).

12.5 Procedimiento para realizar migraciones (makemigrations y migrate)

Añadí la app al settings.py (ver punto 25 abajo).

Ejecutá:

python manage.py makemigrations
python manage.py migrate

13. Primero trabajamos con el MODELO: Vaca

Nos enfocamos en Vaca (CRUD básico).

14. views.py de app_Granja_Vacas — funciones para Vaca

Pegá este código en app_Granja_Vacas/views.py:

from django.shortcuts import render, redirect, get_object_or_404
from .models import Vaca
from django.utils import timezone

# Página de inicio del sistema
def inicio_Granja_Vacas(request):
    contexto = {
        'fecha_sistema': timezone.now(),
        'total_vacas': Vaca.objects.count(),
    }
    return render(request, 'inicio.html', contexto)

# Agregar vaca (mostrar formulario y procesar POST)
def agregar_vaca(request):
    if request.method == 'POST':
        numero_identificacion = request.POST.get('numero_identificacion')
        nombre = request.POST.get('nombre') or None
        fecha_nacimiento = request.POST.get('fecha_nacimiento')
        raza = request.POST.get('raza')
        estado_reproductivo = request.POST.get('estado_reproductivo') or "No gestante"
        peso_kg = request.POST.get('peso_kg') or 0
        corral_actual = request.POST.get('corral_actual')
        notas = request.POST.get('notas') or ''
        Vaca.objects.create(
            numero_identificacion=numero_identificacion,
            nombre=nombre,
            fecha_nacimiento=fecha_nacimiento,
            raza=raza,
            estado_reproductivo=estado_reproductivo,
            peso_kg=peso_kg,
            corral_actual=corral_actual,
            notas=notas
        )
        return redirect('ver_vacas')
    return render(request, 'Vacas/agregar_vaca.html')

# Ver vacas
def ver_vacas(request):
    vacas = Vaca.objects.all().order_by('numero_identificacion')
    return render(request, 'Vacas/ver_vacas.html', {'vacas': vacas})

# Mostrar formulario para actualizar (GET)
def actualizar_vaca(request, vaca_id):
    vaca = get_object_or_404(Vaca, id=vaca_id)
    return render(request, 'Vacas/actualizar_vaca.html', {'vaca': vaca})

# Procesar actualización (POST)
def realizar_actualizacion_vaca(request, vaca_id):
    vaca = get_object_or_404(Vaca, id=vaca_id)
    if request.method == 'POST':
        vaca.numero_identificacion = request.POST.get('numero_identificacion')
        vaca.nombre = request.POST.get('nombre') or None
        vaca.fecha_nacimiento = request.POST.get('fecha_nacimiento')
        vaca.raza = request.POST.get('raza')
        vaca.estado_reproductivo = request.POST.get('estado_reproductivo') or "No gestante"
        vaca.peso_kg = request.POST.get('peso_kg') or 0
        vaca.corral_actual = request.POST.get('corral_actual')
        vaca.notas = request.POST.get('notas') or ''
        vaca.save()
        return redirect('ver_vacas')
    return redirect('ver_vacas')

# Borrar vaca (confirmación y borrado)
def borrar_vaca(request, vaca_id):
    vaca = get_object_or_404(Vaca, id=vaca_id)
    if request.method == 'POST':
        vaca.delete()
        return redirect('ver_vacas')
    return render(request, 'Vacas/borrar_vaca.html', {'vaca': vaca})

15. Crear carpeta templates dentro de app_Granja_Vacas

Estructura sugerida:

app_Granja_Vacas/
  templates/
    base.html
    header.html
    navbar.html
    footer.html
    inicio.html
    Vaca/         (o "Vacas")
      agregar_vaca.html
      ver_vacas.html
      actualizar_vaca.html
      borrar_vaca.html
    categoria/    (según pediste más adelante)
      agregar_categoria.html
      ver_categorias.html
      actualizar_categoria.html
      borrar_categoria.html


NOTA: pediste app_Granja_Vacas\templates\Vacas y también carpeta categoria. Adapta nombres con mayúscula/minúscula coherente.

16–17. base.html con Bootstrap (CSS y JS)

Crea app_Granja_Vacas/templates/base.html:

<!doctype html>
<html lang="es">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{% block title %}Granja Vacas{% endblock %}</title>
    <!-- Bootstrap CSS (CDN) -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
      /* Colores suaves, modernos */
      :root {
        --primary-soft: #6aa6d6;
        --accent-soft: #f7f9fb;
      }
      body { background: var(--accent-soft); }
      footer { position: fixed; bottom: 0; width: 100%; }
      .brand { font-weight: 700; color: #074a6c; }
    </style>
    {% block extra_head %}{% endblock %}
  </head>
  <body>
    {% include 'navbar.html' %}
    <div class="container mt-4 mb-5">
      {% block content %}{% endblock %}
    </div>

    {% include 'footer.html' %}

    <!-- Bootstrap JS (Popper + JS) -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    {% block extra_js %}{% endblock %}
  </body>
</html>

18. navbar.html (con menú y submenús)

Crea app_Granja_Vacas/templates/navbar.html:

<nav class="navbar navbar-expand-lg" style="background-color: var(--primary-soft);">
  <div class="container">
    <a class="navbar-brand text-white brand" href="{% url 'inicio' %}">Sistema de Administración Granja_Vacas</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navMenu">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navMenu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link text-white" href="{% url 'inicio' %}">Inicio</a></li>

        <!-- Vacas -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle text-white" href="#" data-bs-toggle="dropdown">🐄 Vacas</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_vaca' %}">Agregar Vaca</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_vacas' %}">Ver Vacas</a></li>
            <!-- Actualizar y borrar van en Ver Vacas como acciones por fila -->
          </ul>
        </li>

        <!-- Producción -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle text-white" href="#" data-bs-toggle="dropdown">🥛 Producción</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Producción</a></li>
            <li><a class="dropdown-item" href="#">Ver Producción</a></li>
          </ul>
        </li>

        <!-- Evento Sanitario -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle text-white" href="#" data-bs-toggle="dropdown">🩺 Evento_Sanitario</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Evento_Sanitario</a></li>
            <li><a class="dropdown-item" href="#">Ver Evento_Sanitario</a></li>
          </ul>
        </li>

      </ul>
    </div>
  </div>
</nav>


Incluí iconos simples (emoji) en las opciones principales, no en submenú — tal como pediste.

19. footer.html (fija al final, con derechos)

Crea app_Granja_Vacas/templates/footer.html:

<footer class="bg-light text-center py-2 border-top">
  <div class="container">
    <small>
      &copy; {{ fecha_actual|default:None }} - Derechos reservados. Creado por Maximiliano Lira, Cbtis 128.
    </small>
  </div>
</footer>


En inicio_Granja_Vacas envíamos fecha_sistema como contexto; si querés que siempre aparezca la fecha actual, podés usar una plantilla tag o enviar {'fecha_actual': fecha_sistema.date} desde la vista.

20. inicio.html (información del sistema + imagen desde la red sobre cinepolis)

Crea app_Granja_Vacas/templates/inicio.html:

{% extends 'base.html' %}
{% block title %}Inicio - Granja Vacas{% endblock %}
{% block content %}
  <div class="row">
    <div class="col-md-8">
      <h1>Bienvenido al Sistema de Administración - Granja Vacas</h1>
      <p class="lead">Total de vacas registradas: <strong>{{ total_vacas }}</strong></p>
      <p>Fecha del sistema: {{ fecha_sistema }}</p>
      <p>Este sistema permite administrar vacas, producciones y eventos sanitarios.</p>
    </div>
    <div class="col-md-4">
      <div class="card">
        <img src="https://www.cinepolis.com.mx/Content/Images/cinepolis-home.jpg" class="card-img-top" alt="Imagen Cinepolis">
        <div class="card-body">
          <p class="card-text">Imagen tomada desde la red sobre Cinepolis (ejemplo).</p>
        </div>
      </div>
    </div>
  </div>
{% endblock %}


Reemplazá la URL de la imagen si querés otra. Esa es solo un ejemplo.

21–22. Subcarpeta categoria y archivos para categorías

Crea app_Granja_Vacas/templates/categoria/ y dentro los archivos:

agregar_categoria.html

ver_categorias.html

actualizar_categoria.html

borrar_categoria.html

(Como pediste, los creamos aunque ahora estamos trabajando con Vacas. Los archivos pueden contener formularios simples y tablas similares a los de Vaca.)

23. No usar forms.py

El código anterior utiliza request.POST directamente (cumple esto).

24. Procedimiento para crear urls.py en la app (app_Granja_Vacas/urls.py)

Crea app_Granja_Vacas/urls.py con este contenido:

from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_Granja_Vacas, name='inicio'),
    # Vaca CRUD
    path('vacas/agregar/', views.agregar_vaca, name='agregar_vaca'),
    path('vacas/', views.ver_vacas, name='ver_vacas'),
    path('vacas/actualizar/<int:vaca_id>/', views.actualizar_vaca, name='actualizar_vaca'),
    path('vacas/actualizar/procesar/<int:vaca_id>/', views.realizar_actualizacion_vaca, name='realizar_actualizacion_vaca'),
    path('vacas/borrar/<int:vaca_id>/', views.borrar_vaca, name='borrar_vaca'),
    # (Más rutas para producción/eventos y categorías se agregan después)
]

25. Agregar app_Granja_Vacas en settings.py

En backend_Granja_Vacas/settings.py, dentro de INSTALLED_APPS agrega:

INSTALLED_APPS = [
    # apps por defecto ...
    'app_Granja_Vacas',
    # otras apps...
]


También, asegurate que TEMPLATES pueda encontrar tus plantillas (si usás la estructura propuesta no deberías necesitar más cambios, pero podés agregar BASE_DIR / 'app_Granja_Vacas' / 'templates' en DIRS):

import os
from pathlib import Path
BASE_DIR = Path(__file__).resolve().parent.parent

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [ BASE_DIR / 'app_Granja_Vacas' / 'templates' ],
        ...
    },
]


Y para archivos estáticos (si necesitás):

STATIC_URL = '/static/'
STATICFILES_DIRS = [ BASE_DIR / 'app_Granja_Vacas' / 'static' ]

26. Configurar urls.py del proyecto para enlazar con la app

En backend_Granja_Vacas/urls.py:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Granja_Vacas.urls')),  # rutas de la app
]

27. Registrar modelos en admin.py y volver a migrar

En app_Granja_Vacas/admin.py:

from django.contrib import admin
from .models import Vaca, Produccion, EventoSanitario

@admin.register(Vaca)
class VacaAdmin(admin.ModelAdmin):
    list_display = ('numero_identificacion', 'nombre', 'raza', 'corral_actual', 'peso_kg')
    search_fields = ('numero_identificacion', 'nombre', 'raza')

@admin.register(Produccion)
class ProduccionAdmin(admin.ModelAdmin):
    list_display = ('vaca', 'fecha_registro', 'cantidad_litros', 'turno')

@admin.register(EventoSanitario)
class EventoSanitarioAdmin(admin.ModelAdmin):
    list_display = ('tipo_evento', 'fecha_evento', 'veterinario', 'costo')


Luego:

python manage.py makemigrations
python manage.py migrate


Y crear superusuario para acceder al admin:

python manage.py createsuperuser
# seguí las indicaciones (usuario/email/password)

28. Diseño: colores suaves y páginas sencillas

Las plantillas usan Bootstrap + variables CSS con colores suaves (ver base.html > :root). Podés cambiar --primary-soft a cualquier color pastel que prefieras.

29. Crear la estructura completa de carpetas y archivos al inicio

Resumen de estructura mínima que conviene crear desde el principio:

UIII_Granja_Vacas_0627/
  .venv/
  backend_Granja_Vacas/
    settings.py
    urls.py
    ...
  manage.py
  app_Granja_Vacas/
    migrations/
    templates/
      base.html
      navbar.html
      footer.html
      inicio.html
      Vaca/
        agregar_vaca.html
        ver_vacas.html
        actualizar_vaca.html
        borrar_vaca.html
      categoria/
        agregar_categoria.html
        ver_categorias.html
        actualizar_categoria.html
        borrar_categoria.html
    static/
      css/
      js/
    models.py
    views.py
    urls.py
    admin.py

30. Proyecto totalmente funcional (qué probás)

Activás el venv.

python manage.py makemigrations → migrate.

python manage.py createsuperuser (opcional).

python manage.py runserver 8023.

Visitar http://127.0.0.1:8023/ → Debe mostrar la página inicio.

Ir a http://127.0.0.1:8023/vacas/ → lista de vacas (vacía al principio).

http://127.0.0.1:8023/vacas/agregar/ → formulario para crear vacas.

31. Ejecutar servidor en el puerto 8023 (repetido)

Comando:

python manage.py runserver 8023


Copiá http://127.0.0.1:8023/ en tu navegador.

Plantillas para CRUD de Vaca (ejemplos rápidos)

app_Granja_Vacas/templates/Vacas/agregar_vaca.html:

{% extends 'base.html' %}
{% block content %}
<h2>Agregar Vaca</h2>
<form method="POST">
  {% csrf_token %}
  <div class="mb-3">
    <label class="form-label">Número de Identificación</label>
    <input class="form-control" name="numero_identificacion" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Nombre</label>
    <input class="form-control" name="nombre">
  </div>
  <div class="mb-3">
    <label class="form-label">Fecha de Nacimiento</label>
    <input type="date" class="form-control" name="fecha_nacimiento" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Raza</label>
    <input class="form-control" name="raza">
  </div>
  <div class="mb-3">
    <label class="form-label">Peso (kg)</label>
    <input class="form-control" name="peso_kg" step="0.01">
  </div>
  <div class="mb-3">
    <label class="form-label">Corral Actual</label>
    <input class="form-control" name="corral_actual">
  </div>
  <div class="mb-3">
    <label class="form-label">Notas</label>
    <textarea class="form-control" name="notas"></textarea>
  </div>
  <button class="btn btn-primary" type="submit">Guardar</button>
  <a class="btn btn-secondary" href="{% url 'ver_vacas' %}">Volver</a>
</form>
{% endblock %}


app_Granja_Vacas/templates/Vacas/ver_vacas.html:

{% extends 'base.html' %}
{% block content %}
<h2>Listado de Vacas</h2>
<table class="table table-striped">
  <thead>
    <tr>
      <th>ID</th>
      <th>Numero</th>
      <th>Nombre</th>
      <th>Raza</th>
      <th>Peso (kg)</th>
      <th>Corral</th>
      <th>Acciones</th>
    </tr>
  </thead>
  <tbody>
    {% for vaca in vacas %}
    <tr>
      <td>{{ vaca.id }}</td>
      <td>{{ vaca.numero_identificacion }}</td>
      <td>{{ vaca.nombre }}</td>
      <td>{{ vaca.raza }}</td>
      <td>{{ vaca.peso_kg }}</td>
      <td>{{ vaca.corral_actual }}</td>
      <td>
        <a class="btn btn-sm btn-info" href="{% url 'actualizar_vaca' vaca.id %}">Editar</a>
        <form style="display:inline" method="post" action="{% url 'borrar_vaca' vaca.id %}">
          {% csrf_token %}
          <button class="btn btn-sm btn-danger" type="submit">Borrar</button>
        </form>
      </td>
    </tr>
    {% empty %}
    <tr><td colspan="7">No hay vacas registradas.</td></tr>
    {% endfor %}
  </tbody>
</table>
<a class="btn btn-primary" href="{% url 'agregar_vaca' %}">Agregar Vaca</a>
{% endblock %}


app_Granja_Vacas/templates/Vacas/actualizar_vaca.html:

{% extends 'base.html' %}
{% block content %}
<h2>Actualizar Vaca: {{ vaca.numero_identificacion }}</h2>
<form method="post" action="{% url 'realizar_actualizacion_vaca' vaca.id %}">
  {% csrf_token %}
  <div class="mb-3">
    <label class="form-label">Número de Identificación</label>
    <input class="form-control" name="numero_identificacion" value="{{ vaca.numero_identificacion }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Nombre</label>
    <input class="form-control" name="nombre" value="{{ vaca.nombre }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Fecha de Nacimiento</label>
    <input type="date" class="form-control" name="fecha_nacimiento" value="{{ vaca.fecha_nacimiento }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Raza</label>
    <input class="form-control" name="raza" value="{{ vaca.raza }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Peso (kg)</label>
    <input class="form-control" name="peso_kg" value="{{ vaca.peso_kg }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Corral Actual</label>
    <input class="form-control" name="corral_actual" value="{{ vaca.corral_actual }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Notas</label>
    <textarea class="form-control" name="notas">{{ vaca.notas }}</textarea>
  </div>
  <button class="btn btn-success" type="submit">Guardar cambios</button>
  <a class="btn btn-secondary" href="{% url 'ver_vacas' %}">Cancelar</a>
</form>
{% endblock %}


app_Granja_Vacas/templates/Vacas/borrar_vaca.html:

{% extends 'base.html' %}
{% block content %}
<h2>Confirmar borrado</h2>
<p>¿Seguro que querés borrar la vaca {{ vaca.numero_identificacion }} - {{ vaca.nombre }}?</p>
<form method="post">
  {% csrf_token %}
  <button class="btn btn-danger" type="submit">Sí, borrar</button>
  <a class="btn btn-secondary" href="{% url 'ver_vacas' %}">Cancelar</a>
</form>
{% endblock %}

Comandos resumen para arrancar desde cero (rápido)
mkdir UIII_Granja_Vacas_0627
cd UIII_Granja_Vacas_0627
python -m venv .venv
# activar venv (Windows PowerShell ejemplo)
.venv\Scripts\Activate.ps1
pip install --upgrade pip
pip install django
django-admin startproject backend_Granja_Vacas .
python manage.py startapp app_Granja_Vacas
# editar settings.py -> agregar 'app_Granja_Vacas', templates DIRS
# crear models.py (pegar contenido), admin.py, views.py, urls.py, templates...
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver 8023

Bash

python manage.py runserver 8023
Accede a http://127.0.0.1:8023/ en tu navegador para ver la página de inicio.

## 🐄 Proyecto: UIII_Granja_Vacas_0627
**Lenguaje:** Python  
**Framework:** Django  
**Editor:** Visual Studio Code  

---

### 📂 Estructura del Proyecto

UIII_Granja_Vacas_0627/

│

├── .venv/ ← Entorno virtual de Python

│

├── backend_Granja_Vacas/ ← Proyecto principal de Django

│ ├── init.py

│ ├── asgi.py

│ ├── settings.py

│ ├── urls.py ← Aquí se enlaza con las URLs de la app

│ ├── wsgi.py

│ └── manage.py

│
├── app_Granja_Vacas/ ← Aplicación principal

│ ├── init.py

│ ├── admin.py ← Registrar modelos aquí

│ ├── apps.py

│ ├── models.py ← Contiene los modelos: Vaca, Produccion, EventoSanitario

│ ├── views.py ← Funciones CRUD: inicio, agregar, ver, actualizar, borrar vaca

│ ├── urls.py ← Rutas de la app (CRUD de Vaca)

│ ├── migrations/ ← Migraciones de la base de datos

│ │ └── init.py

│ │

│ └── templates/ ← Carpeta de plantillas HTML

│ ├── base.html ← Contiene Bootstrap, header y footer incluidos

│ ├── header.html

│ ├── navbar.html

│ ├── footer.html

│ ├── inicio.html ← Página principal con información e imagen

│ │

│ └── Vacas/ ← Subcarpeta para CRUD de Vacas

│ ├── agregar_vaca.html

│ ├── ver_vacas.html ← Muestra las vacas en tabla con botones Ver, Editar, Borrar

│ ├── actualizar_vaca.html

│ ├── borrar_vaca.html

├── db.sqlite3 ← Base de datos de Django

│

└── requirements.txt ← (Opcional) Dependencias del proyecto, ej. Django==5.x
