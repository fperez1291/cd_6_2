# Configuración del Servidor Docker

Una vez instalado Docker, es importante configurarlo correctamente para garantizar un entorno seguro, eficiente y bien organizado. En esta guía se cubren las configuraciones más habituales para un servidor Docker.

---

## Configuración del Daemon de Docker

El **daemon de Docker** (`dockerd`) es el proceso en segundo plano que gestiona contenedores, imágenes, redes y volúmenes. Su comportamiento se controla mediante el archivo `/etc/docker/daemon.json`.

### Crear o editar el archivo de configuración

```bash
sudo nano /etc/docker/daemon.json
```

### Ejemplo de configuración recomendada

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "storage-driver": "overlay2",
  "default-address-pools": [
    { "base": "172.20.0.0/16", "size": 24 }
  ],
  "live-restore": true
}
```

| Parámetro        | Descripción                                                |
| ---------------- | ---------------------------------------------------------- |
| `log-driver`     | Define el controlador de logs (por defecto `json-file`)    |
| `max-size`       | Tamaño máximo de cada archivo de log                       |
| `max-file`       | Número máximo de archivos de log rotados                   |
| `storage-driver` | Motor de almacenamiento (recomendado `overlay2`)           |
| `live-restore`   | Mantiene los contenedores activos si el daemon se reinicia |

### Aplicar los cambios

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

---

## Gestión de Redes en Docker

Docker crea automáticamente tres redes por defecto: `bridge`, `host` y `none`. Para proyectos más complejos, es recomendable crear redes personalizadas.

### Crear una red personalizada

```bash
docker network create \
  --driver bridge \
  --subnet 172.20.0.0/24 \
  --gateway 172.20.0.1 \
  mi-red
```

### Listar redes disponibles

```bash
docker network ls
```

### Conectar un contenedor a una red

```bash
docker network connect mi-red nombre-contenedor
```

---

## Gestión de Volúmenes

Los volúmenes permiten **persistir datos** más allá del ciclo de vida de un contenedor. Son la forma recomendada de almacenar datos en Docker.

### Crear un volumen

```bash
docker volume create mi-volumen
```

### Listar volúmenes existentes

```bash
docker volume ls
```

### Inspeccionar un volumen

```bash
docker volume inspect mi-volumen
```

### Montar un volumen en un contenedor

```bash
docker run -d \
  --name mi-app \
  -v mi-volumen:/datos \
  imagen:tag
```

---

## Variables de Entorno y Archivos `.env`

Es una buena práctica gestionar la configuración sensible mediante variables de entorno, evitando incluirla directamente en el código o en los archivos `docker-compose.yml`.

### Crear un archivo `.env`

```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=mi_base_datos
DB_USER=admin
DB_PASSWORD=contraseña_segura
```

### Referenciar variables en `docker-compose.yml`

```yaml
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
```

> [!WARNING] **Seguridad**
> Nunca subas el archivo `.env` a tu repositorio. Añádelo al `.gitignore`:
> ```text
> .env
> ```

---

## Límites de Recursos

Es recomendable establecer límites de CPU y memoria para los contenedores en entornos de producción, evitando que un contenedor consuma todos los recursos del host.

### Mediante `docker run`

```bash
docker run -d \
  --name mi-app \
  --memory="512m" \
  --cpus="1.0" \
  imagen:tag
```

### Mediante `docker-compose.yml`

```yaml
services:
  mi-app:
    image: imagen:tag
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
```

---

## Configuración del Firewall

Si estás usando Docker en un servidor, asegúrate de que el firewall permite el tráfico necesario:

```bash
# Permitir puertos HTTP y HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Permitir un puerto personalizado (ej. 8080)
sudo ufw allow 8080/tcp

# Ver el estado del firewall
sudo ufw status
```

> [!INFO] **Docker y UFW**
> Docker modifica directamente las reglas de `iptables`, lo que puede saltarse las reglas de UFW. Consulta la [documentación oficial](https://docs.docker.com/) para configurar esto correctamente en entornos de producción.

---

## Siguientes Pasos

Con el entorno configurado, ya puedes proceder al [Despliegue de tu aplicación con Docker](despliegue.md).
