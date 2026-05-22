# Configuración del Proyecto

En esta sección se describe el proceso de configuración del proyecto de documentación utilizando MkDocs, el tema Material y GitHub.

---

## Configuración Inicial de MkDocs

Después de instalar MkDocs y crear el proyecto, se configuró el archivo principal:

```text
mkdocs.yml
````

Este archivo controla el comportamiento general de la documentación.

---

## Configuración Básica

Se modificó el archivo `mkdocs.yml` para utilizar el tema Material y habilitar algunas funcionalidades adicionales.

## Contenido del archivo mkdocs.yml

```yaml
site_name: Mi Documentación

theme:
  name: material

  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - content.code.copy
```

---

## Explicación de la Configuración

### site_name

```yaml
site_name: Mi Documentación
```

Define el nombre principal que aparece en la parte superior de la página web.

---

### Tema Material

```yaml
theme:
  name: material
```

Permite utilizar el tema Material para MkDocs, proporcionando:

- Diseño moderno.
- Navegación mejorada.
- Compatibilidad móvil.
- Mejor presentación visual.

---

## Funcionalidades Activadas

### Navegación por pestañas

```yaml
- navigation.tabs
```

Muestra la navegación superior mediante pestañas.

---

### Navegación por secciones

```yaml
- navigation.sections
```

Agrupa automáticamente el contenido en secciones organizadas.

---

### Expansión automática del menú

```yaml
- navigation.expand
```

Mantiene desplegado el árbol de navegación lateral.

---

### Botón de copiar código

```yaml
- content.code.copy
```

Añade un botón para copiar fácilmente bloques de código.

---

## Organización de la Documentación

Se creó una estructura organizada dentro de la carpeta `docs`.

## Estructura utilizada

```text
docs/
├── index.md
├── guia/
│   ├── instalacion.md
│   ├── configuracion.md
│   └── despliegue.md
└── ejemplos/
    └── ejemplo1.md
```

---

## Descripción de la Estructura

### index.md

Página principal de la documentación.

### guia/

Contiene las páginas principales del tutorial:

- Instalación
- Configuración
- Despliegue

### ejemplos/

Incluye ejemplos prácticos o demostraciones adicionales.

---

## Configuración de la Página Principal

Se editó el archivo:

```text
docs/index.md
```

Con contenido similar al siguiente:

```markdown
# Bienvenido a Mi Documentación

Esta es la página principal de mi documentación.

## Contenido

- [Guía de Instalación](guia/instalacion.md)
- [Configuración](guia/configuracion.md)
- [Despliegue](guia/despliegue.md)
```

---

## Configuración del Repositorio Git

Para controlar versiones del proyecto, se inicializó un repositorio Git.

### Inicialización

```bash
git init
```

---

## Archivo .gitignore

Se creó un archivo `.gitignore` para evitar subir archivos innecesarios al repositorio.

## Contenido del archivo

```text
venv/
```

Esto evita incluir el entorno virtual dentro del repositorio remoto.

---

## Primer Commit

Una vez configurado el proyecto, se realizó el primer commit.

### Añadir archivos

```bash
git add .
```

### Crear commit

```bash
git commit -m "Primer commit del proyecto MkDocs"
```

---

## Vinculación con GitHub

Después del commit inicial, el repositorio local se conectó con GitHub.

### Añadir repositorio remoto

```bash
git remote add origin https://github.com/usuario/repositorio.git
```

### Subir proyecto

```bash
git push -u origin main
```

---

## Configuración de GitHub Pages

Para publicar la documentación, se configuró GitHub Pages desde la configuración del repositorio.

### Pasos realizados

1. Acceder a **Settings**.
2. Entrar en **Pages**.
3. Seleccionar:

```text
Deploy from a branch
```

4. Elegir la rama:

```text
gh-pages
```

---

## Configuración de GitHub Actions

Se configuró GitHub Actions para automatizar el despliegue de la documentación.

El workflow utilizado se almacenó en:

```text
.github/workflows/deploy.yml
```

Este archivo automatiza:

* Instalación de dependencias.
* Generación de la documentación.
* Publicación en GitHub Pages.

---

## Personalización Visual

El tema Material permite añadir múltiples opciones de personalización.

Algunas opciones adicionales que pueden configurarse son:

```yaml
theme:
  palette:
    primary: blue
    accent: indigo
```

Esto modifica los colores principales de la documentación.

---

## Vista Previa de la Documentación

Durante el desarrollo se utilizó el servidor local de MkDocs para comprobar cambios en tiempo real.

### Ejecutar servidor

```bash
mkdocs serve
```

---

## Verificación del Funcionamiento

Para comprobar que toda la configuración funcionaba correctamente, se realizaron las siguientes verificaciones:

- El servidor local iniciaba sin errores.
- Las páginas Markdown se visualizaban correctamente.
- La navegación funcionaba.
- Git detectaba cambios correctamente.
- GitHub Actions completaba el despliegue.
- GitHub Pages mostraba la documentación publicada.

---

## Problemas Encontrados

### Error en rutas Markdown

**Problema:**

Algunos enlaces internos no funcionaban.

**Solución:**

Verificar que las rutas relativas entre archivos eran correctas.

---

### Error en GitHub Actions

**Problema:**

El workflow fallaba durante el despliegue.

**Solución:**

Comprobar permisos de escritura en:

```text
Settings > Actions > General
```

---

### Cambios no visibles en GitHub Pages

**Problema:**

La web no se actualizaba inmediatamente.

**Solución:**

Esperar unos minutos y limpiar la caché del navegador.

---

## Resultado Final

Tras completar toda la configuración:

- MkDocs quedó configurado correctamente.
- El tema Material funcionó sin errores.
- Git y GitHub permitieron controlar versiones.
- GitHub Actions automatizó el despliegue.
- GitHub Pages publicó correctamente la documentación.

Con esto quedó finalizada la configuración completa del proyecto de documentación.
