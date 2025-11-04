# Primera parte

## Proyecto: Juguetería 0494

- **Lenguaje**: Python
- **Framework**: Django
- **Editor**: VS Code

---

### Procedimiento para crear carpeta del Proyecto

- **Nombre de la carpeta**: `UIII_Jugueteria_0494`

---

### Procedimiento para abrir VS Code sobre la carpeta

1. Abre Visual Studio Code.
2. En el menú, selecciona **Archivo > Abrir carpeta** y selecciona la carpeta `UIII_Jugueteria_0494`.

---

### Procedimiento para abrir terminal en VS Code

1. En Visual Studio Code, ve al menú **Ver > Terminal** o usa el atajo **Ctrl + `** (grave).

---

### Procedimiento para crear carpeta entorno virtual `.venv` desde terminal de VS Code

En la terminal de VS Code, escribe:

```bash
python -m venv .venv
Procedimiento para activar el entorno virtual
En Windows:

bash
Copiar código
.venv\Scripts\activate
En MacOS/Linux:

bash
Copiar código
source .venv/bin/activate
Procedimiento para activar intérprete de Python
En VS Code, presiona Ctrl + Shift + P y escribe Python: Select Interpreter.

Selecciona el intérprete correspondiente a tu entorno virtual .venv.

Procedimiento para instalar Django
En la terminal de VS Code, ejecuta el siguiente comando para instalar Django:

bash
Copiar código
pip install django
Procedimiento para crear proyecto backend_Jugueteria sin duplicar carpeta
En la terminal, ejecuta el siguiente comando para crear el proyecto Django:

bash
Copiar código
django-admin startproject backend_Jugueteria
Procedimiento para ejecutar servidor en el puerto 8012
En la terminal, ve al directorio de tu proyecto y ejecuta:

bash
Copiar código
python manage.py runserver 8012
Procedimiento para copiar y pegar el link en el navegador
En la terminal aparecerá algo como: http://127.0.0.1:8012/.

Copia ese link y pégalo en tu navegador.

Procedimiento para crear aplicación app_Jugueteria
En la terminal, dentro del proyecto backend_Jugueteria, ejecuta:

bash
Copiar código
python manage.py startapp app_Jugueteria
Aquí el modelo models.py
python
Copiar código
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
Procedimiento para realizar las migraciones (makemigrations y migrate)
Para crear las migraciones, ejecuta:

bash
Copiar código
python manage.py makemigrations
Para aplicar las migraciones, ejecuta:

bash
Copiar código
python manage.py migrate
Procedimiento para trabajar con el MODELO: SUCURSAL
En el archivo views.py de la app app_Jugueteria, crea las siguientes funciones:

inicio_jugueteria

agregar_sucursal

actualizar_sucursal

realizar_actualizacion_sucursal

borrar_sucursal

Crear la carpeta templates dentro de app_Jugueteria.

En la carpeta templates, crea los siguientes archivos HTML:

base.html

header.html

navbar.html

footer.html

inicio.html

En el archivo base.html, agrega Bootstrap para CSS y JS.

En el archivo navbar.html, incluye las opciones:

“Sistema de Administración Juguetería 0494”

“Inicio”

“Sucursales” con submenú:

Agregar Sucursal

Ver Sucursales

Actualizar Sucursal

Borrar Sucursal

“Clientes” con submenú:

Agregar Cliente

Ver Clientes

Actualizar Cliente

Borrar Cliente

“Empleados” con submenú:

Agregar Empleado

Ver Empleados

Actualizar Empleado

Borrar Empleado

Incluir íconos en las opciones principales, no en los submenús.

En el archivo footer.html, incluye:

Derechos de autor

Fecha del sistema

“Creado por Ailin Gallegos, Cbtis 128” (mantener fija al final de la página).

En el archivo inicio.html, coloca información del sistema más una imagen tomada desde la red sobre Jugueterías.

Crear la subcarpeta sucursal dentro de app_Jugueteria/templates
Dentro de app_Jugueteria/templates/sucursal, crea los archivos HTML con el siguiente código:

agregar_sucursal.html

ver_sucursales.html (mostrar en tabla con botones ver, editar y borrar)

actualizar_sucursal.html

borrar_sucursal.html

Procedimiento para crear el archivo urls.py en app_Jugueteria
Crea el archivo urls.py con el código correspondiente para acceder a las funciones de views.py para operaciones CRUD en sucursales.

Procedimiento para agregar app_Jugueteria en settings.py de backend_Jugueteria
En el archivo settings.py, agrega 'app_Jugueteria' en la lista de aplicaciones instaladas.

Realizar las configuraciones correspondientes en urls.py de backend_Jugueteria
Configura urls.py en backend_Jugueteria para enlazar con app_Jugueteria.

Procedimiento para registrar los modelos en admin.py y realizar las migraciones
En admin.py de app_Jugueteria, agrega el siguiente código:

python
Copiar código
from django.contrib import admin
from .models import Sucursal, Cliente, Empleado

admin.site.register(Sucursal)
admin.site.register(Cliente)
admin.site.register(Empleado)
Realiza nuevamente las migraciones.

Estilo y diseño
Utiliza colores suaves, atractivos y modernos.

El código de las páginas web debe ser sencillo.

No validar entrada de datos.

Estructura completa de carpetas y archivos
Crear la estructura completa de carpetas y archivos desde el inicio.

Proyecto completamente funcional.

Ejecutar servidor en el puerto 8012
Finalmente, ejecuta el servidor en el puerto 8012:

bash
Copiar código
python manage.py runserver 8012
css
Copiar código

Este texto ya está estructurado para ser colocado como archivo `leeme_primera_parte.md` en tu repositorio de GitHub. Puedes pegarlo tal cual para documentar el proceso y las configuraciones de tu proyecto. ¿Te gustaría ayuda con algún otro archivo o parte del proyecto?




