# Guía de Instalación de Docker

Esta guía te llevará paso a paso por el proceso de instalación de Docker en los principales sistemas operativos.

## ¿Qué es Docker?

Docker es una plataforma de contenedores que permite empaquetar aplicaciones y sus dependencias en unidades aisladas llamadas **contenedores**. Esto garantiza que la aplicación funcione de forma idéntica en cualquier entorno, independientemente del sistema operativo o la configuración del host.

## Requisitos Previos

Antes de instalar Docker, asegúrate de cumplir con los siguientes requisitos:

- **Sistema operativo de 64 bits**
- **Windows 10/11** (Pro, Enterprise o Education) / **Ubuntu 20.04+** / **macOS 11+**
- Al menos **4 GB de RAM**
- Virtualización habilitada en la BIOS (para Windows)

---

## Instalación en Windows

### 1. Descargar Docker Desktop

Accede a la [página oficial de Docker](https://www.docker.com/products/docker-desktop/) y descarga el instalador para Windows.

### 2. Ejecutar el instalador

Haz doble clic en el archivo descargado (`Docker Desktop Installer.exe`) y sigue el asistente de instalación. Asegúrate de marcar la opción **"Use WSL 2 instead of Hyper-V"** si tu sistema lo permite.

### 3. Reiniciar el sistema

Una vez finalizada la instalación, reinicia el equipo cuando se te solicite.

### 4. Verificar la instalación

Abre una terminal (PowerShell o CMD) y ejecuta:

```bash
docker --version
docker run hello-world
```

Si ves el mensaje de bienvenida de Docker, la instalación se ha realizado correctamente.

---

## Instalación en Ubuntu / Debian

### 1. Actualizar los paquetes del sistema

```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Instalar dependencias necesarias

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

### 3. Añadir la clave GPG oficial de Docker

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

### 4. Añadir el repositorio de Docker

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. Instalar Docker Engine

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### 6. Añadir tu usuario al grupo docker (opcional pero recomendado)

```bash
sudo usermod -aG docker $USER
newgrp docker
```

### 7. Verificar la instalación

```bash
docker --version
docker run hello-world
```

---

## Instalación en macOS

### 1. Descargar Docker Desktop para Mac

Descarga el instalador desde la [web oficial de Docker](https://www.docker.com/products/docker-desktop/), eligiendo la versión correcta según tu procesador (**Intel** o **Apple Silicon**).

### 2. Instalar la aplicación

Arrastra el icono de Docker a la carpeta **Aplicaciones** y ábrelo. Docker solicitará permisos de administrador durante el primer arranque.

### 3. Verificar la instalación

```bash
docker --version
docker run hello-world
```

---

## Instalación de Docker Compose

Docker Compose permite definir y gestionar aplicaciones multi-contenedor. En versiones modernas de Docker Desktop ya viene incluido. Para verificarlo:

```bash
docker compose version
```

Si necesitas instalarlo manualmente en Linux:

```bash
sudo apt install -y docker-compose-plugin
```

---

## Solución de Problemas Comunes

| Problema                              | Causa probable             | Solución                                                                             |
| ------------------------------------- | -------------------------- | ------------------------------------------------------------------------------------ |
| `Cannot connect to the Docker daemon` | El servicio no está activo | `sudo systemctl start docker`                                                        |
| `Permission denied`                   | Usuario sin permisos       | Añadir al grupo `docker`                                                             |
| `WSL 2 installation is incomplete`    | WSL 2 no configurado       | Seguir la [guía de Microsoft](https://learn.microsoft.com/es-es/windows/wsl/install) |
| Puerto ya en uso                      | Conflicto de puertos       | Cambiar el puerto en la configuración                                                |

---

## Siguientes Pasos

Una vez completada la instalación, continúa con la [Configuración del servidor Docker](configuracion.md) para preparar tu entorno.