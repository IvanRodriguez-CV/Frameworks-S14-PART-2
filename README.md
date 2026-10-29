# Semana 14 - Autenticación, roles y reportes

Proyecto Django que sigue las tres guías de Semana 14: inicio de sesión y
registro de usuarios, control de acceso por rol y generación de reportes PDF
de productos. Conserva el catálogo de categorías y productos requerido por
las guías.

## Preparar el entorno

En PowerShell, desde esta carpeta:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Configura MySQL o MariaDB y crea una base de datos llamada `comercial`. La
conexión usa estas variables de entorno; si no las defines, usa `root` sin
contraseña en `127.0.0.1:3306`:

```powershell
$env:DB_NAME = "comercial"
$env:DB_USER = "root"
$env:DB_PASSWORD = "tu contraseña"
$env:DB_HOST = "127.0.0.1"
$env:DB_PORT = "3306"
```

Django 4.2.30 es compatible con Python 3.12 y MariaDB 10.4. Esta versión de
Django ya no recibe actualizaciones de seguridad. Para un entorno de
producción, actualiza MariaDB a 10.11 o posterior y usa una versión de Django
con soporte vigente.

## Reportes PDF en Windows

WeasyPrint necesita Pango en Windows. Abre la terminal UCRT64 de MSYS2 y
ejecuta:

```bash
pacman -S mingw-w64-ucrt-x86_64-pango
```

Si instalaste MSYS2 en `C:\msys64`, agrega sus bibliotecas al `PATH` de
PowerShell antes de iniciar Django:

```powershell
$env:PATH = "C:\msys64\ucrt64\bin;$env:PATH"
```

## Migraciones y servidor

```powershell
python .\manage.py migrate
python .\manage.py runserver
```

Las migraciones dejan intactas las tablas antiguas de `Cliente` y `TipoPago`
que ya existan en una base de datos, pero esos modelos y funciones ya no
forman parte del proyecto.

La aplicación inicia en <http://127.0.0.1:8000/>. Las categorías están en
<http://127.0.0.1:8000/categoria/> y los productos en
<http://127.0.0.1:8000/producto/>. El listado de productos requiere una sesión
iniciada con el rol `admin`; el registro asigna el rol `vendedor`. El reporte
se genera desde el botón de la lista de productos.

Las imágenes JPG y PNG de productos se guardan en `media/productos/`. El
listado pagina un producto por página, como muestra la guía de seguridad.

Para ejecutar las pruebas locales sin MySQL o MariaDB:

```powershell
$env:DB_ENGINE = "sqlite"
python .\manage.py test catalogo
```
