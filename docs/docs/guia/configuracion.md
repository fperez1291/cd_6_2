# Configuración

> [!NOTE]
> En algunas imágenes se ha censurado la ruta de directorios y archivos de forma parcial por cuestiones de seguridad.

## Instalación de Python

Para instalar Python debe seguir los pasos siguientes:

1. Descargar el `Python installer manager` de la [página web oficial](https://www.python.org/downloads/).

2. Ejecutar el archivo de instalación `python-manager-26.2.msix`. Le aparecerá la siguiente ventana.

<div align="center">
 <img src="../../../img/config_python/python-installer-1.png" alt="python-installer-1"/>
</div>

3. Pulse el botón `Instalar Python` para continuar. Cuando el instalador termine, se abrirá una terminal como la que se muestra a continuación. 

<div align="center">
 <img src="../../../img/config_python/python-manager-terminal.png" alt="python-manager-terminal"/>
</div>

4. A continuación, pulse `y` a las dos preguntas que le hará el manager de configurción para instalar Python y configurar automáticamente la variable PATH.

<div align="center">
 <img src="../../../img/config_python/python-manager-installation.png" alt="python-manager-installation"/>
</div>

5. Para finalizar, debe pulsar `Enter` o `n` para rechazar la visualización de la ayuda online.

<div align="center">
 <img src="../../../img/config_python/python-rechazar-ayuda-online.png" alt="python-rechazar-ayuda-online"/>
</div>

Para comprobar que la instalación se ha completado correctaemnte, puede ejecutar en la consola el siguiente comando:

```bash
python --version
```

Si todo está en orden, se mostrará la sigueinte salida: 

```console
> python --version
Python 3.14.5
```

## Configuración del Entorno

1. Creamos el directorio `docs\` y nos posicionamos en él.

<div align="center">
 <img src="../../../img/config_entorno/crear_dir_docs.png" alt="Creación del directorio docs/ por consola"/>
</div>

2. Creamos el entorno virtual de Python con el comando `python -m venv venv`.

<div align="center">
 <img src="../../../img/config_entorno/crear_entorno_virtual_python.png" alt="Creación del entorno virtual de Python"/>
</div>

3. Activamos el entorno virtual mediante el comando `venv\Scripts\activate`. Al ejecutar el comando, se obtiene la siguiente respuesta:

<div align="center">
 <img src="../../../img/config_entorno/activar_entorno_error_1.png" alt="Error al ejecutar venv\Scripts\activate"/>
</div>

Para solventarlo, se tiene en cuenta la siguiente información obtenida en una búsqueda de Brave (respuesta proporcionada por Leo, el agente de IA de Brave): 

> [!NOTE]
> `venv\Scripts\activate` : la ejecución de scripts está deshabilitada en este sistema
> 
> Este error ocurre porque **PowerShell** tiene por defecto una directiva de ejecución restrictiva que impide cargar scripts no firmados, como `Activate.ps1`. La solución más segura y recomendada es cambiar la política de ejecución para el usuario actual permitiendo scripts locales no firmados, pero exigiendo firma para los descargados de internet.
> 
> Abre PowerShell (preferiblemente como administrador si el cambio no surte efecto en la sesión actual) y ejecuta el siguiente comando:
> 
> ```powershell
> Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> 
> Al ejecutarlo, confirma el cambio escribiendo **S** o **A**. Una vez aplicado, puedes activar tu entorno virtual normalmente con `.\venv\Scripts\Activate.ps1`.
> 
> Si no deseas modificar la política global de PowerShell, una alternativa es cambiar el terminal predeterminado en tu editor (como VS Code) al **Símbolo del sistema (CMD)**, ya que utiliza el script `activate.bat`, el cual no está sujeto a estas restricciones de PowerShell.

Procedemos a seguir los pasos dados por Leo AI y obtenemos el siguiente problema: 

<div align="center">
 <img src="../../../img/config_entorno/activar_entorno_error_2.png" alt=""/>
</div>

Para solucionarlo, simplemente editamos la carpeta que contiene los corchetes y ejecutamos de nuevo el comando `venv\Scripts\activate`:

<div align="center">
 <img src="../../../img/config_entorno/entorno_activado.png" alt="Entorno activado"/>
</div>

4. Instalamos MkDocs y el tema Material.

<div align="center">
 <img src="../../../img/config_entorno/instalacion_mkdocs_material.png" alt="Instalación de mkdocs y mkdocs-material"/>
</div>

Con esto ya tenemos el entorno creado, activo y preparado para crear el proyecto MkDocs. 

## Creación del Proyecto MkDocs

Para inicializar el proyecto MkDocs ejecutamos el siguiente comando:

```console
mkdocs new .
```

Tras su ejecución, la estructura del proyecto debería tener el siguiente aspecto:

<div align="center">
 <img src="../../../img/config_mkdocs/incializacion_mkdocs.png" alt="Inicialización del proyecto MkDocs"/>
</div>

## Configuración de MkDocs

Para configurar el MkDocs, insertamos el fragmento siguiente en el archivo `mkdocs.yml`, configurando así el tema Material. 

```yml
site_name: Mi Documentación
theme:
  name: material
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - content.code.copy
```

<div align="center">
 <img src="../../../img/config_mkdocs/config_mkdocs_yml.png" alt="Configuración del archivo mkdocs.yml"/>
</div>

## Creación de la Estructura de Documentación

Pasamos a crear la estructura de directorios y archivos mostrada en el enunciado.

<div align="center">
 <img src="../../../img/estructura_doc/estructura.png" alt="Estructura de directorios y archivos resultante"/>
</div>

A continuación se edita el archivo `docs\index.md`: 

<div align="center">
 <img src="../../../img/estructura_doc/archivo_index.png" alt="Contenido del archivo index.md"/>
</div>

En este punto también se pide 
