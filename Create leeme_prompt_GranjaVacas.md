Perfecto — te doy **un procedimiento paso a paso (comandos + código listo para pegar)** para completar toda la *Primera parte* de tu proyecto **UIII_Granja_Vacas_0627** en **Python / Django** usando **VS Code**. Sigo el orden que pediste (1 → 31). Copia y pega los fragmentos donde corresponda.

---

# 1 — Crear carpeta del proyecto

En tu ubicación preferida (Explorador / Terminal):

```bash
mkdir UIII_Granja_Vacas_0627
cd UIII_Granja_Vacas_0627
```

# 2 — Abrir VS Code en esa carpeta

Desde la terminal:

```bash
code .
```

(o desde el explorador: botón derecho → *Open with Code*).

# 3 — Abrir terminal en VS Code

En VS Code: menú **Ver → Terminal** (o `Ctrl+ñ` / `Ctrl+` ` (Windows)`) — se abrirá integrado en la carpeta actual.

# 4 — Crear carpeta entorno virtual `.venv` desde la terminal de VS Code

**Windows (PowerShell):**

```powershell
python -m venv .venv
```

**Linux / macOS:**

```bash
python3 -m venv .venv
```

Se creará la carpeta `.venv` en la raíz del proyecto.

# 5 — Activar el entorno virtual

**PowerShell (Windows):**

```powershell
.\.venv\Scripts\Activate.ps1
```

(Si error por políticas de ejecución, ejecutar `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` como administrador o usar el comando `.venv\Scripts\activate` en cmd)

**cmd (Windows):**

```cmd
.\.venv\Scripts\activate
```

**Linux / macOS:**

```bash
source .venv/bin/activate
```

Al activarse verás `(.venv)` al inicio de la línea.

# 6 — Activar intérprete de Python en VS Code

En VS Code: `Ctrl+Shift+P` → *Python: Select Interpreter* → selecciona la ruta `.../UIII_Granja_Vacas_0627/.venv/...` (el intérprete del entorno).

# 7 — Instalar Django

Con el entorno activo:

```bash
pip install django
```

(Puedes fijar versión: `pip install "django>=4.2,<5"` si quieres).

# 8 — Crear proyecto `backend_Granja_Vacas` sin duplicar carpeta

Para **evitar crear una carpeta adicional** dentro de `UIII_Granja_Vacas_0627`, ejecuta desde la raíz:

```bash
django-admin startproject backend_Granja_Vacas .
```

> Nota: el `.` al final crea el proyecto en la carpeta actual (no crea `backend_Granja_Vacas/backend_Granja_Vacas` duplicado).

Estructura resultante inicial:

```
UIII_Granja_Vacas_0627/
├─ backend_Granja_Vacas/
│  ├─ __init__.py
│  ├─ settings.py
│  ├─ urls.py
│  └─ wsgi.py
├─ manage.py
└─ .venv/
```

# 9 — Ejecutar servidor en el puerto **8023**

```bash
python manage.py runserver 8023
```

(o `python3 manage.py runserver 0.0.0.0:8023` si quieres aceptar conexiones externas).

# 10 — Copiar y pegar el link en el navegador

Abre en el navegador:

```
http://127.0.0.1:8023/
```

ó

```
http://localhost:8023/
```

# 11 — Crear la aplicación `app_Granja_Vacas`

Con el entorno activo y en la raíz del proyecto:

```bash
python manage.py startapp app_Granja_Vacas
```

Estructura:

```
app_Granja_Vacas/
 ├─ migrations/
 ├─ admin.py
 ├─ apps.py
 ├─ models.py
 ├─ views.py
 ├─ urls.py   <-- (lo crearás)
 └─ templates/ (lo crearás)
```

# 12 — Pegar/usar el `models.py` que compartiste

Copia tu contenido (el que enviaste) a `app_Granja_Vacas/models.py`. (No hace falta modificarlo ahora.)

> Recuadro: tu `models.py` (ya lo diste). Asegúrate de que `app_Granja_Vacas` está presente en INSTALLED_APPS antes de migrar.

# 12.5 — Procedimiento para realizar migraciones (`makemigrations` y `migrate`)

1. Asegúrate de que en `backend_Granja_Vacas/settings.py` esté añadida la app (ver punto 25).
2. Ejecuta:

```bash
python manage.py makemigrations
python manage.py migrate
```

Esto crea las tablas en la DB sqlite por defecto.

# 13 — Primero trabajamos con el MODELO: **Vaca**

Nos concentraremos en CRUD de `Vaca`. Producción y EventoSanitario quedan pendientes (como pediste).

# 14 — `views.py` de `app_Granja_Vacas`: funciones y código

Sustituye / añade en `app_Granja_Vacas/views.py` el siguiente código (funciones: `inicio_Granja_Vacas`, `agregar_vaca`, `actualizar_vaca`, `realizar_actualizacion_vaca`, `borrar_vaca`):

```python
# app_Granja_Vacas/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Vaca
from django.urls import reverse

def inicio_Granja_Vacas(request):
    # muestra información del sistema + imagen
    return render(request, 'inicio.html', {})

def agregar_vaca(request):
    if request.method == 'POST':
        # sin validación (según tu instrucción)
        Vaca.objects.create(
            numero_identificacion = request.POST.get('numero_identificacion'),
            nombre = request.POST.get('nombre') or None,
            fecha_nacimiento = request.POST.get('fecha_nacimiento'),
            raza = request.POST.get('raza'),
            estado_reproductivo = request.POST.get('estado_reproductivo') or "No gestante",
            peso_kg = request.POST.get('peso_kg') or 0,
            corral_actual = request.POST.get('corral_actual'),
            notas = request.POST.get('notas',''),
        )
        return redirect('ver_vacas')
    return render(request, 'Vacas/agregar_vaca.html', {})

def ver_vacas(request):
    vacas = Vaca.objects.all().order_by('numero_identificacion')
    return render(request, 'Vacas/ver_vacas.html', {'vacas': vacas})

def actualizar_vaca(request, pk):
    vaca = get_object_or_404(Vaca, pk=pk)
    return render(request, 'Vacas/actualizar_vaca.html', {'vaca': vaca})

def realizar_actualizacion_vaca(request, pk):
    vaca = get_object_or_404(Vaca, pk=pk)
    if request.method == 'POST':
        vaca.numero_identificacion = request.POST.get('numero_identificacion')
        vaca.nombre = request.POST.get('nombre') or None
        vaca.fecha_nacimiento = request.POST.get('fecha_nacimiento')
        vaca.raza = request.POST.get('raza')
        vaca.estado_reproductivo = request.POST.get('estado_reproductivo') or "No gestante"
        vaca.peso_kg = request.POST.get('peso_kg') or 0
        vaca.corral_actual = request.POST.get('corral_actual')
        vaca.notas = request.POST.get('notas','')
        vaca.save()
        return redirect('ver_vacas')
    return redirect('ver_vacas')

def borrar_vaca(request, pk):
    vaca = get_object_or_404(Vaca, pk=pk)
    if request.method == 'POST':
        vaca.delete()
        return redirect('ver_vacas')
    return render(request, 'Vacas/borrar_vaca.html', {'vaca': vaca})
```

# 15 — Crear carpeta `templates` dentro de `app_Granja_Vacas`

Estructura:

```
app_Granja_Vacas/
 └─ templates/
    ├─ base.html
    ├─ header.html
    ├─ navbar.html
    ├─ footer.html
    ├─ inicio.html
    └─ Vacas/
       ├─ agregar_vaca.html
       ├─ ver_vacas.html
       ├─ actualizar_vaca.html
       └─ borrar_vaca.html
```

# 16 & 17 — `base.html` (con Bootstrap) y archivos parciales

Crea `app_Granja_Vacas/templates/base.html`:

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>{% block title %}Sistema de Administración Granja_Vacas{% endblock %}</title>

  <!-- Bootstrap CSS (CDN) -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">

  <style>
    /* Colores suaves y modernos */
    body { background: #f7fbfd; color: #21303f; }
    .footer-fixed { position: fixed; left:0; bottom:0; width:100%; }
    .card-soft { border-radius: 12px; box-shadow: 0 4px 12px rgba(33,48,63,0.06); }
  </style>
  {% block extra_head %}{% endblock %}
</head>
<body>
  {% include 'navbar.html' %}
  <main class="container my-4">
    {% block content %}{% endblock %}
  </main>

  {% include 'footer.html' %}

  <!-- Bootstrap JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
  {% block extra_js %}{% endblock %}
</body>
</html>
```

`navbar.html` (coloca en `templates/navbar.html`). Incluye iconos con emojis (solución simple y compatible):

```html
<nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm">
  <div class="container">
    <a class="navbar-brand" href="{% url 'inicio' %}">🐄 Sistema de Administración Granja_Vacas</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navs">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navs">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="{% url 'inicio' %}">Inicio</a></li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">🐮 Vacas</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_vaca' %}">Agregar Vaca</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_vacas' %}">Ver Vaca</a></li>
            <li><a class="dropdown-item" href="#">Actualizar Vaca</a></li>
            <li><a class="dropdown-item" href="#">Borrar Vaca</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">🥛 Producción</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Producción</a></li>
            <li><a class="dropdown-item" href="#">Ver Producción</a></li>
            <li><a class="dropdown-item" href="#">Actualizar Producción</a></li>
            <li><a class="dropdown-item" href="#">Borrar Producción</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">💉 Evento_Sanitario</a>
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
```

`footer.html` (en `templates/footer.html`):

```html
<footer class="footer-fixed bg-white text-center py-2 border-top">
  <div class="container">
    <small>© {{ now|date:"Y" }} - Creado por Maximiliano Lira, Cbtis 128. Todos los derechos reservados.</small>
  </div>
</footer>
```

(En `base.html` usamos `{{ now }}` si agregas `django.template.context_processors.request`/`django.template.context_processors.tz` — si no, puedes usar `{{ "" }}`; alternativa simple: poner la fecha manual.)

`header.html` es opcional; puedes incluir metadatos o banner.

# 18 — `navbar.html` ya incluye las opciones que solicitaste

* Opciones principales con iconos (emoji) y submenús sin iconos (tal como pediste).
* Ajusta enlaces donde faltan (los `#`) cuando implementes Producción/Eventos.

# 19 — footer: derechos de autor, fecha del sistema y “Creado por Maximiliano Lira, Cbtis 128” y fija al final

(ya incluido en `footer.html` con clase `.footer-fixed`).

# 20 — `inicio.html` (imagen desde la red)

`app_Granja_Vacas/templates/inicio.html`:

```html
{% extends 'base.html' %}
{% block title %}Inicio - Granja Vacas{% endblock %}
{% block content %}
<div class="card card-soft p-4">
  <h1>Sistema de Administración - Granja Vacas</h1>
  <p>Bienvenido al sistema de administración. Aquí puede gestionar vacas, producción y eventos sanitarios.</p>
  <img src="https://images.unsplash.com/photo-1548199973-03cce0bbc87b" alt="granja" class="img-fluid rounded">
</div>
{% endblock %}
```

(Esa URL es de ejemplo; cámbiala si quieres otra.)

# 21 — Crear subcarpeta `categoria` dentro de `app_Granja_Vacas/templates`

Tu instrucción 21 pide crear carpeta `categoria`. Hazlo:

```
app_Granja_Vacas/templates/categoria/
```

(Si más adelante quieres archivos ahí, agrégalos.)

# 22 — Crear archivos HTML de categorías dentro de `app_Granja_Vacas/templates/Vacas`

Crea `Vacas/agregar_vaca.html`, `Vacas/ver_vacas.html`, `Vacas/actualizar_vaca.html`, `Vacas/borrar_vaca.html`.

Ejemplos básicos:

`Vacas/agregar_vaca.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Agregar Vaca</h2>
<form method="post">
  {% csrf_token %}
  <div class="mb-3">
    <label>Número de Identificación</label>
    <input name="numero_identificacion" class="form-control" required>
  </div>
  <div class="mb-3"><label>Nombre</label><input name="nombre" class="form-control"></div>
  <div class="mb-3"><label>Fecha de Nacimiento</label><input type="date" name="fecha_nacimiento" class="form-control"></div>
  <div class="mb-3"><label>Raza</label><input name="raza" class="form-control"></div>
  <div class="mb-3"><label>Estado Reproductivo</label><input name="estado_reproductivo" class="form-control"></div>
  <div class="mb-3"><label>Peso (kg)</label><input name="peso_kg" class="form-control" type="number" step="0.01"></div>
  <div class="mb-3"><label>Corral Actual</label><input name="corral_actual" class="form-control"></div>
  <div class="mb-3"><label>Notas</label><textarea name="notas" class="form-control"></textarea></div>
  <button class="btn btn-primary">Guardar</button>
</form>
{% endblock %}
```

`Vacas/ver_vacas.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Listado de Vacas</h2>
<table class="table table-striped">
  <thead><tr>
    <th>ID</th><th>Nº Identif.</th><th>Nombre</th><th>Raza</th><th>Fecha Nac.</th><th>Acciones</th>
  </tr></thead>
  <tbody>
    {% for v in vacas %}
    <tr>
      <td>{{ v.pk }}</td>
      <td>{{ v.numero_identificacion }}</td>
      <td>{{ v.nombre }}</td>
      <td>{{ v.raza }}</td>
      <td>{{ v.fecha_nacimiento }}</td>
      <td>
        <a class="btn btn-sm btn-success" href="{% url 'actualizar_vaca' v.pk %}">Editar</a>
        <a class="btn btn-sm btn-danger" href="{% url 'borrar_vaca' v.pk %}">Borrar</a>
      </td>
    </tr>
    {% empty %}
    <tr><td colspan="6">No hay vacas registradas.</td></tr>
    {% endfor %}
  </tbody>
</table>
{% endblock %}
```

`Vacas/actualizar_vaca.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Actualizar Vaca</h2>
<form method="post" action="{% url 'realizar_actualizacion_vaca' vaca.pk %}">
  {% csrf_token %}
  <div class="mb-3"><label>Número de Identificación</label><input name="numero_identificacion" value="{{ vaca.numero_identificacion }}" class="form-control"></div>
  <div class="mb-3"><label>Nombre</label><input name="nombre" value="{{ vaca.nombre }}" class="form-control"></div>
  <div class="mb-3"><label>Fecha de Nacimiento</label><input type="date" name="fecha_nacimiento" value="{{ vaca.fecha_nacimiento }}" class="form-control"></div>
  <div class="mb-3"><label>Raza</label><input name="raza" value="{{ vaca.raza }}" class="form-control"></div>
  <div class="mb-3"><label>Estado Reproductivo</label><input name="estado_reproductivo" value="{{ vaca.estado_reproductivo }}" class="form-control"></div>
  <div class="mb-3"><label>Peso (kg)</label><input name="peso_kg" value="{{ vaca.peso_kg }}" class="form-control"></div>
  <div class="mb-3"><label>Corral Actual</label><input name="corral_actual" value="{{ vaca.corral_actual }}" class="form-control"></div>
  <div class="mb-3"><label>Notas</label><textarea name="notas" class="form-control">{{ vaca.notas }}</textarea></div>
  <button class="btn btn-primary">Actualizar</button>
</form>
{% endblock %}
```

`Vacas/borrar_vaca.html`

```html
{% extends 'base.html' %}
{% block content %}
<h2>Confirmar borrado</h2>
<p>¿Desea eliminar la vaca <strong>{{ vaca.numero_identificacion }} - {{ vaca.nombre }}</strong>?</p>
<form method="post">
  {% csrf_token %}
  <button type="submit" class="btn btn-danger">Sí, eliminar</button>
  <a href="{% url 'ver_vacas' %}" class="btn btn-secondary">Cancelar</a>
</form>
{% endblock %}
```

# 23 — **No usar `forms.py`**

Todo el HTML usa formularios HTML puros y `request.POST` en las views (cumple tu instrucción).

# 24 — `urls.py` en la app (`app_Granja_Vacas/urls.py`)

Crea este archivo con las rutas:

```python
# app_Granja_Vacas/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_Granja_Vacas, name='inicio'),
    path('vacas/agregar/', views.agregar_vaca, name='agregar_vaca'),
    path('vacas/', views.ver_vacas, name='ver_vacas'),
    path('vacas/editar/<int:pk>/', views.actualizar_vaca, name='actualizar_vaca'),
    path('vacas/editar/<int:pk>/guardar/', views.realizar_actualizacion_vaca, name='realizar_actualizacion_vaca'),
    path('vacas/borrar/<int:pk>/', views.borrar_vaca, name='borrar_vaca'),
]
```

# 25 — Agregar `app_Granja_Vacas` en `settings.py` de `backend_Granja_Vacas`

Edita `backend_Granja_Vacas/settings.py` → `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    # apps por defecto...
    'app_Granja_Vacas',
]
```

# 26 — Configurar `urls.py` de `backend_Granja_Vacas` para enlazar la app

Edita `backend_Granja_Vacas/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Granja_Vacas.urls')),  # rutas de la app (inicio, vacas, etc.)
]
```

# 27 — Registrar modelos en `admin.py` y volver a realizar migraciones

`app_Granja_Vacas/admin.py`:

```python
from django.contrib import admin
from .models import Vaca, Produccion, EventoSanitario

@admin.register(Vaca)
class VacaAdmin(admin.ModelAdmin):
    list_display = ('numero_identificacion','nombre','raza','fecha_nacimiento','corral_actual')
    search_fields = ('numero_identificacion','nombre','raza')

@admin.register(Produccion)
class ProduccionAdmin(admin.ModelAdmin):
    list_display = ('vaca','fecha_registro','cantidad_litros','turno')

@admin.register(EventoSanitario)
class EventoSanitarioAdmin(admin.ModelAdmin):
    list_display = ('tipo_evento','fecha_evento','veterinario','costo')
```

Luego:

```bash
python manage.py makemigrations
python manage.py migrate
```

# 27 (adicional) — Por ahora solo trabajar con “Vacas”

Aunque registraste los modelos, en la interfaz web te concentrarás en las vistas/plantillas de `Vaca`. Producción y EventoSanitario los dejamos para después, como pediste.

# 28 — Estilo: colores suaves y diseño sencillo

En `base.html` incluí estilos suaves y `card-soft` para modernizar. Puedes ajustar variables CSS al gusto.

# 29 — Al inicio crear la estructura completa de carpetas y archivos

Resumen de estructura propuesta (crea con el explorador o `mkdir`):

```
UIII_Granja_Vacas_0627/
├─ .venv/
├─ manage.py
├─ backend_Granja_Vacas/
│  ├─ settings.py
│  └─ urls.py
├─ app_Granja_Vacas/
│  ├─ migrations/
│  ├─ admin.py
│  ├─ models.py
│  ├─ views.py
│  ├─ urls.py
│  └─ templates/
│     ├─ base.html
│     ├─ navbar.html
│     ├─ footer.html
│     ├─ inicio.html
│     └─ Vacas/
│        ├─ agregar_vaca.html
│        ├─ ver_vacas.html
│        ├─ actualizar_vaca.html
│        └─ borrar_vaca.html
```

# 30 — Proyecto totalmente funcional (mínimo requerido)

Pasos resumidos para dejar todo funcional:

1. Crear `.venv`, activarlo y seleccionar intérprete.
2. `pip install django`
3. `django-admin startproject backend_Granja_Vacas .`
4. `python manage.py startapp app_Granja_Vacas`
5. Pegar `models.py`.
6. Añadir `app_Granja_Vacas` en `INSTALLED_APPS`.
7. Crear `app_Granja_Vacas/urls.py` y `views.py` con el código dado.
8. Crear templates (base, navbar, vacas/...).
9. `python manage.py makemigrations` → `python manage.py migrate`.
10. `python manage.py createsuperuser` (si quieres acceder a admin).
11. `python manage.py runserver 8023`
12. Abrir `http://127.0.0.1:8023/`

# 31 — Finalmente ejecutar servidor en el puerto 8023

(ya descrito):

```bash
python manage.py runserver 8023
```

---

## Consejos prácticos / comprobaciones

* Si `templates` no se detecta: Django busca plantillas en `APP/templates/` automáticamente si `APP` está en INSTALLED_APPS. Si prefieres carpeta global de plantillas, añade en `settings.py` `TEMPLATES[0]['DIRS'] = [BASE_DIR / 'templates']`.
* Para el footer con fecha dinámica: puedes usar `django.template.context_processors.request` o pasar la fecha en el contexto desde tus views (por simplicidad, el footer usa `{{ now|date:"Y" }}` si agregas `django.template.context_processors.tz`).
* Si usas Windows PowerShell y falla `Activate.ps1`, puedes abrir un terminal cmd o ajustar políticas como indiqué.
* Para imágenes desde la red en `inicio.html` asegúrate de que la URL sea accesible o descarga la imagen y sirve como archivo estático.

---

Si quieres, en el siguiente paso **te creo los archivos exactos listos para pegar** (te los devuelvo aquí con el contenido ordenado por ruta) — dime si los quieres *todos* (views, urls, settings cambios, templates completos). También puedo generar un `.zip` si prefieres (te mostraré cómo guardarlo localmente). ¿Cómo quieres que siga?
