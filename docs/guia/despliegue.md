# Despliegue de Aplicaciones con Docker

En esta sección aprenderás a desplegar aplicaciones usando Docker y Docker Compose, desde un contenedor simple hasta un stack completo de servicios.

---

## Conceptos Clave

Antes de comenzar, es útil tener claros estos conceptos:

- **Imagen**: Plantilla de solo lectura que define el contenido del contenedor (sistema operativo, dependencias, código).
- **Contenedor**: Instancia en ejecución de una imagen.
- **Dockerfile**: Archivo de texto con instrucciones para construir una imagen personalizada.
- **Docker Compose**: Herramienta para definir y gestionar aplicaciones multi-contenedor mediante un archivo `docker-compose.yml`.

---

## Despliegue de un Contenedor Simple

El caso más básico es ejecutar un contenedor a partir de una imagen pública del registro de Docker Hub.

### Ejemplo: desplegar un servidor Nginx

```bash
docker run -d \
  --name servidor-web \
  -p 8080:80 \
  --restart unless-stopped \
  nginx:latest
```

| Opción                     | Descripción                                                             |
| -------------------------- | ----------------------------------------------------------------------- |
| `-d`                       | Ejecuta el contenedor en segundo plano (detached)                       |
| `--name`                   | Asigna un nombre al contenedor                                          |
| `-p 8080:80`               | Mapea el puerto 8080 del host al 80 del contenedor                      |
| `--restart unless-stopped` | Reinicia el contenedor automáticamente salvo que se detenga manualmente |

Accede a `http://localhost:8080` para comprobar que Nginx está funcionando.

---

## Construcción de una Imagen Personalizada

Si tu aplicación requiere una configuración específica, debes crear tu propia imagen usando un `Dockerfile`.

### Ejemplo de Dockerfile para una app Node.js

```dockerfile
# Imagen base
FROM node:20-alpine

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar archivos de dependencias
COPY package*.json ./

# Instalar dependencias
RUN npm install --production

# Copiar el resto del código
COPY . .

# Exponer el puerto de la aplicación
EXPOSE 3000

# Comando de inicio
CMD ["node", "server.js"]
```

### Construir la imagen

```bash
docker build -t mi-app:1.0 .
```

### Ejecutar el contenedor

```bash
docker run -d \
  --name mi-app \
  -p 3000:3000 \
  mi-app:1.0
```

---

## Despliegue con Docker Compose

Docker Compose es la opción recomendada cuando tu aplicación necesita múltiples servicios (por ejemplo, una app web + una base de datos).

### Estructura del proyecto

```text
mi-proyecto/
├── docker-compose.yml
├── .env
└── app/
    ├── Dockerfile
    └── ...
```

### Ejemplo de `docker-compose.yml` completo

```yaml
version: '3.9'

services:

  web:
    build: ./app
    container_name: mi-web
    ports:
      - "8080:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    depends_on:
      - db
    restart: unless-stopped
    networks:
      - mi-red

  db:
    image: postgres:15-alpine
    container_name: mi-db
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - datos-db:/var/lib/postgresql/data
    restart: unless-stopped
    networks:
      - mi-red

volumes:
  datos-db:

networks:
  mi-red:
    driver: bridge
```

### Comandos principales de Docker Compose

```bash
# Levantar todos los servicios en segundo plano
docker compose up -d

# Ver el estado de los servicios
docker compose ps

# Ver los logs de todos los servicios
docker compose logs -f

# Ver los logs de un servicio concreto
docker compose logs -f web

# Detener los servicios
docker compose down

# Detener y eliminar volúmenes
docker compose down -v

# Reconstruir las imágenes y levantar
docker compose up -d --build
```

---

## Actualización de un Servicio en Producción

Para actualizar una aplicación sin tiempo de inactividad:

```bash
# 1. Construir la nueva imagen
docker build -t mi-app:2.0 .

# 2. Actualizar la imagen en el compose
# (editar docker-compose.yml para cambiar el tag a 2.0)

# 3. Redeployar solo el servicio afectado
docker compose up -d --no-deps --build web
```

---

## Monitorización Básica

### Ver contenedores en ejecución

```bash
docker ps
```

### Ver uso de recursos en tiempo real

```bash
docker stats
```

### Inspeccionar un contenedor

```bash
docker inspect nombre-contenedor
```

### Acceder a la shell de un contenedor

```bash
docker exec -it nombre-contenedor sh
```

---

## Buenas Prácticas en el Despliegue

> [!TIP] Recomendaciones
>
> - Usa siempre **tags específicos** en las imágenes (evita `:latest` en producción).
> - Define **políticas de reinicio** (`restart: unless-stopped`) para mayor resiliencia.
> - Almacena los datos persistentes en **volúmenes**, nunca en el sistema de archivos del contenedor.
> - Utiliza **redes personalizadas** para aislar los servicios entre sí.
> - Mantén el `Dockerfile` limpio: usa imágenes base ligeras (Alpine) y elimina caché de paquetes.
> - No incluyas secretos directamente en el `docker-compose.yml`; usa archivos `.env` o secretos de Docker Swarm.

---

## Siguientes Pasos

Consulta el [Ejemplo práctico](../ejemplos/ejemplo1.md) para ver un despliegue real de una aplicación web con base de datos usando Docker Compose.
