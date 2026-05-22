# Instalación del Entorno

En esta sección se describe el proceso seguido para preparar el entorno de desarrollo necesario para crear la documentación con MkDocs.

---

## Requisitos Previos

Antes de comenzar fue necesario disponer de las siguientes herramientas instaladas en el sistema:

- Python 3.7 o superior
- Git
- Visual Studio Code
- Cuenta en GitHub

Estas herramientas permiten crear la documentación, gestionar versiones del proyecto y publicar el resultado final en GitHub Pages.

---

## Instalación de Python

MkDocs está desarrollado en Python, por lo que el primer paso consistió en instalar Python en el sistema.

### Comprobación de instalación

Para verificar que Python estaba instalado correctamente, se ejecutó el siguiente comando en la terminal:

```bash id="3px91m"
python --version
````

Si Python está instalado correctamente, aparece un resultado similar a:

```text id="m7lm8s"
Python 3.11.0
```

---

## Instalación de Git

Git es necesario para gestionar versiones y sincronizar el proyecto con GitHub.

### Verificación de instalación

```bash id="uwv2z5"
git --version
```

Resultado esperado:

```text id="i2p95h"
git version 2.x.x
```

---

## Creación del Proyecto

Se creó una carpeta para almacenar todo el proyecto de documentación.

```bash id="n4r5w0"
mkdir mi-documentacion
cd mi-documentacion
```

---

## Creación del Entorno Virtual

Para evitar conflictos entre dependencias de diferentes proyectos, se creó un entorno virtual de Python.

### Crear entorno virtual

```bash id="5fgm5x"
python -m venv venv
```

Este comando genera una carpeta llamada `venv` que contiene una instalación aislada de Python.

---

## Activación del Entorno Virtual

Una vez creado el entorno virtual, fue necesario activarlo.

### En Windows

```bash id="9vrmnl"
venv\Scripts\activate
```

### En Linux o macOS

```bash id="f9k5p0"
source venv/bin/activate
```

Cuando el entorno virtual está activo, la terminal suele mostrar el nombre `(venv)` al inicio de la línea de comandos.

---

## Instalación de MkDocs

Con el entorno virtual activado, se instalaron MkDocs y el tema Material mediante `pip`.

```bash id="6cw42q"
pip install mkdocs mkdocs-material
```

---

## Verificación de la Instalación

Para comprobar que MkDocs se instaló correctamente, se ejecutó:

```bash id="p8c3ew"
mkdocs --version
```

Resultado esperado:

```text id="j1b77j"
mkdocs, version X.X.X
```

---

## Inicialización del Proyecto MkDocs

Una vez instaladas las dependencias, se creó la estructura inicial del proyecto con el siguiente comando:

```bash id="r5y6gx"
mkdocs new .
```

Este comando genera automáticamente:

```text id="g34q6m"
mkdocs.yml
docs/
```

---

## Estructura Inicial Generada

La estructura básica creada por MkDocs fue:

```text id="6cm9kh"
mi-documentacion/
├── docs/
│   └── index.md
└── mkdocs.yml
```

### Descripción de los archivos

- `mkdocs.yml` → Archivo principal de configuración.
- `docs/index.md` → Página principal de la documentación.

---

## Instalación de Visual Studio Code

Para facilitar la edición de archivos Markdown y la gestión del proyecto, se utilizó Visual Studio Code.

### Ventajas de Visual Studio Code

- Integración con Git y GitHub.
- Terminal integrada.
- Extensiones para Markdown.
- Resaltado de sintaxis.
- Vista previa de documentación.

---

## Vista Previa Local

MkDocs permite visualizar la documentación localmente antes de publicarla.

### Ejecutar servidor local

```bash id="ax8g40"
mkdocs serve
```

Después de ejecutar el comando, la documentación queda disponible normalmente en:

```text id="1ep7q7"
http://127.0.0.1:8000
```

Esto permite comprobar cambios en tiempo real mientras se editan los archivos Markdown.

---

## Problemas Encontrados

Durante la instalación pueden aparecer algunos problemas comunes.

### Python no reconocido

**Problema:**

El sistema no reconoce el comando `python`.

**Solución:**

Añadir Python al `PATH` durante la instalación o reinstalar Python marcando la opción:

```text id="yvc5au"
Add Python to PATH
```

---

### Error al activar el entorno virtual en Windows

**Problema:**

Windows bloquea la ejecución de scripts.

**Solución:**

Ejecutar PowerShell como administrador y usar:

```powershell id="r6m6c9"
Set-ExecutionPolicy RemoteSigned
```

---

### MkDocs no encontrado

**Problema:**

El comando `mkdocs` no funciona.

**Solución:**

Verificar que el entorno virtual está activado antes de instalar MkDocs.

---

## Resultado Final

Tras completar todos los pasos:

- Python quedó instalado correctamente.
- El entorno virtual fue configurado.
- MkDocs y Material quedaron instalados.
- La estructura inicial de documentación fue creada.
- La documentación pudo visualizarse localmente sin errores.

Con esto quedó preparado el entorno necesario para continuar con la creación y despliegue de la documentación.
