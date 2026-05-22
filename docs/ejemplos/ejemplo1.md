# Ejemplo Práctico: App Web con Base de Datos

En este ejemplo desplegaremos una aplicación web completa utilizando Docker Compose. El stack estará compuesto por tres servicios:

- **Nginx** – Servidor web y proxy inverso
- **Node.js** – Backend de la aplicación
- **PostgreSQL** – Base de datos relacional

Este ejemplo representa un caso de uso real y habitual en entornos de desarrollo y producción.

---

## Estructura del Proyecto

```
mi-app-docker/
├── docker-compose.yml
├── .env
├── nginx/
│   └── default.conf
└── backend/
    ├── Dockerfile
    ├── package.json
    └── server.js
```

---

## Paso 1: Crear el archivo `.env`

Crea un archivo `.env` en la raíz del proyecto con las variables de entorno necesarias:

```env
# Base de datos
DB_NAME=appdb
DB_USER=appuser
DB_PASSWORD=S3cr3tP4ss!

# Aplicación
NODE_ENV=production
PORT=3000
```

!!! warning "Importante"
    Añade `.env` a tu `.gitignore` para no exponer credenciales en el repositorio.

---

## Paso 2: Crear el servidor Node.js

### `backend/package.json`

```json
{
  "name": "mi-backend",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.2",
    "pg": "^8.11.0"
  }
}
```

### `backend/server.js`

```javascript
const express = require('express');
const { Pool } = require('pg');

const app = express();
const port = process.env.PORT || 3000;

const pool = new Pool({
  host: 'db',
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  port: 5432,
});

app.get('/', async (req, res) => {
  try {
    const result = await pool.query('SELECT NOW() AS hora_servidor');
    res.json({
      mensaje: '¡Servidor Docker funcionando correctamente!',
      hora: result.rows[0].hora_servidor,
    });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.get('/health', (req, res) => {
  res.json({ estado: 'OK' });
});

app.listen(port, () => {
  console.log(`Servidor escuchando en el puerto ${port}`);
});
```

### `backend/Dockerfile`

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

---

## Paso 3: Configurar Nginx como Proxy Inverso

### `nginx/default.conf`

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass         http://backend:3000;
        proxy_http_version 1.1;
        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_cache_bypass $http_upgrade;
    }

    location /health {
        proxy_pass http://backend:3000/health;
    }
}
```

Con esta configuración, Nginx actúa como **proxy inverso**: recibe las peticiones en el puerto 80 y las redirige al backend Node.js internamente.

---

## Paso 4: Definir el `docker-compose.yml`

```yaml
version: '3.9'

services:

  nginx:
    image: nginx:stable-alpine
    container_name: proxy
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - backend
    restart: unless-stopped
    networks:
      - app-net

  backend:
    build: ./backend
    container_name: backend
    environment:
      - NODE_ENV=${NODE_ENV}
      - PORT=${PORT}
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - app-net

  db:
    image: postgres:15-alpine
    container_name: base-de-datos
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - datos-postgres:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped
    networks:
      - app-net

volumes:
  datos-postgres:

networks:
  app-net:
    driver: bridge
```

### Puntos destacados de esta configuración

- El servicio `db` incluye un **healthcheck** para que el backend no arranque hasta que PostgreSQL esté listo.
- Los tres servicios comparten la red `app-net`, lo que les permite comunicarse por nombre de servicio (por ejemplo, `http://backend:3000`).
- Los datos de PostgreSQL se almacenan en el volumen `datos-postgres`, garantizando su persistencia entre reinicios.

---

## Paso 5: Arrancar el Stack

```bash
# Construir imágenes y levantar todos los servicios
docker compose up -d --build

# Verificar que todos los servicios están en ejecución
docker compose ps
```

La salida esperada debería ser similar a:

```
NAME              IMAGE                    STATUS          PORTS
proxy             nginx:stable-alpine      Up              0.0.0.0:80->80/tcp
backend           mi-app-docker-backend    Up              3000/tcp
base-de-datos     postgres:15-alpine       Up (healthy)    5432/tcp
```

---

## Paso 6: Verificar el Funcionamiento

Abre un navegador o usa `curl` para comprobar que todo funciona:

```bash
# Comprobar la respuesta de la API
curl http://localhost/

# Comprobar el endpoint de salud
curl http://localhost/health
```

Respuesta esperada de `/`:

```json
{
  "mensaje": "¡Servidor Docker funcionando correctamente!",
  "hora": "2024-11-15T10:32:41.123Z"
}
```

---

## Paso 7: Detener y Limpiar el Entorno

```bash
# Detener los servicios (conservando los datos)
docker compose down

# Detener los servicios y eliminar todos los volúmenes
docker compose down -v

# Eliminar imágenes construidas localmente
docker rmi mi-app-docker-backend
```

---

## Resumen

| Servicio        | Imagen                | Puerto interno | Puerto externo |
| --------------- | --------------------- | -------------- | -------------- |
| Nginx (proxy)   | `nginx:stable-alpine` | 80             | 80             |
| Backend Node.js | Imagen personalizada  | 3000           | — (interno)    |
| PostgreSQL      | `postgres:15-alpine`  | 5432           | — (interno)    |

Este ejemplo demuestra cómo Docker Compose facilita el despliegue de un stack completo de servicios con pocas líneas de configuración, asegurando el aislamiento, la portabilidad y la persistencia de los datos.