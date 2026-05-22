# Despliegue de la Documentación

En esta sección se explica el proceso seguido para desplegar la documentación generada con MkDocs utilizando GitHub Pages y GitHub Actions.

---

## Objetivo del Despliegue

El objetivo es publicar automáticamente la documentación del proyecto en una página web accesible desde internet cada vez que se realicen cambios en la rama principal del repositorio.

Para ello se utilizan las siguientes herramientas:

- **MkDocs** → Generador de documentación estática.
- **GitHub Pages** → Servicio de alojamiento web estáico de GitHub.
- **GitHub Actions** → Sistema de automatización e integración continua.

---

## Creación del Workflow de GitHub Actions

Para automatizar el despliegue, se creó el siguiente archivo:

```text
.github/workflows/deploy.yml
````

Este archivo contiene la configuración necesaria para que GitHub ejecute automáticamente el despliegue cada vez que se haga un `push` a la rama `main`.

## Contenido del archivo deploy.yml

```yaml
name: Deploy MkDocs
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.x'
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
```

---

## Explicación del Workflow

### 1. Evento de activación

```yaml
on:
  push:
    branches: [ main ]
```

El despliegue se ejecutará automáticamente cada vez que se suban cambios a la rama principal (`main`).

---

### 2. Máquina virtual

```yaml
runs-on: ubuntu-latest
```

GitHub crea una máquina virtual con Ubuntu para ejecutar las tareas del workflow.

---

### 3. Descarga del repositorio

```yaml
- uses: actions/checkout@v2
```

Este paso descarga el contenido del repositorio dentro de la máquina virtual.

---

### 4. Configuración de Python

```yaml
- uses: actions/setup-python@v2
```

Instala y configura Python para poder ejecutar MkDocs.

---

### 5. Instalación de dependencias

```yaml
- run: pip install mkdocs-material
```

Se instala MkDocs junto con el tema Material necesario para generar la documentación.

---

### 6. Despliegue automático

```yaml
- run: mkdocs gh-deploy --force
```

Este comando:

* Genera el sitio web estático.
* Crea o actualiza la rama `gh-pages`.
* Publica automáticamente la documentación en GitHub Pages.

La opción `--force` fuerza la actualización del contenido desplegado.

---

## Configuración de Permisos

Para que GitHub Actions pueda publicar contenido en la rama `gh-pages`, fue necesario modificar los permisos del repositorio.

### Pasos realizados

1. Acceder al repositorio en GitHub.
2. Abrir la pestaña **Settings**.
3. Entrar en **Actions > General**.
4. Buscar la sección **Workflow permissions**.
5. Seleccionar:

```text
Read and write permissions
```

6. Guardar los cambios.

Sin este paso, el workflow produce errores de permisos durante el despliegue.

---

## Configuración de GitHub Pages

Después de ejecutar el workflow por primera vez, se configuró GitHub Pages.

### Pasos realizados

1. Abrir el repositorio en GitHub.
2. Acceder a:

```text
Settings > Pages
```

3. En la sección **Source** seleccionar:

```text
Deploy from a branch
```

4. Elegir:

* Rama: `gh-pages`
* Carpeta: `/ (root)`

5. Guardar los cambios.

---

## Proceso de Publicación

Una vez configurado todo el entorno, el despliegue se realiza automáticamente siguiendo este flujo:

1. Se modifica la documentación localmente.
2. Se realiza un commit.
3. Se hace push a GitHub.
4. GitHub Actions detecta el cambio.
5. Se ejecuta el workflow.
6. MkDocs genera la web.
7. GitHub Pages publica automáticamente la documentación.

---

## Comandos Utilizados

### Añadir archivos al repositorio

```bash
git add .
```

### Crear commit

```bash
git commit -m "Configuración de GitHub Pages"
```

### Subir cambios

```bash
git push origin main
```

---

## Verificación del Despliegue

Para comprobar que el despliegue se realizó correctamente:

1. Acceder a la pestaña **Actions** del repositorio.
2. Verificar que el workflow aparece con estado correcto.
3. Esperar unos segundos tras finalizar el proceso.
4. Abrir la URL de GitHub Pages.

La documentación queda accesible en una dirección similar a:

```text
https://usuario.github.io/repositorio/
```

---

## Problemas Encontrados

Durante el proceso pueden aparecer algunos errores comunes:

### Error de permisos

**Problema:**

GitHub Actions no puede hacer push a `gh-pages`.

**Solución:**

Activar permisos de lectura y escritura en:

```text
Settings > Actions > General
```

---

### Página no encontrada (404)

**Problema:**

GitHub Pages no muestra la documentación.

**Solución:**

* Verificar que la rama `gh-pages` existe.
* Confirmar que GitHub Pages utiliza dicha rama.
* Esperar unos minutos tras el despliegue.

---

### MkDocs no instalado

**Problema:**

El workflow falla porque MkDocs no está instalado.

**Solución:**

Añadir la instalación dentro del workflow:

```yaml
- run: pip install mkdocs-material
```

---

## Resultado Final

Gracias a GitHub Actions y GitHub Pages, la documentación queda publicada automáticamente cada vez que se actualiza el repositorio, permitiendo mantener una documentación profesional, accesible y siempre actualizada.
