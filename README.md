# lymon-fullstack

Monorepo de plataforma hotelera con frontend web, backend API y orquestacion con contenedores.

## Que contiene este repositorio

- Infraestructura fullstack con Docker Compose.
- Submodulo backend con API REST.
- Submodulo frontend con aplicacion web.
- Base de datos MongoDB en contenedor.

## Tecnologias y versiones

### Backend

- Node.js 20 (imagen usada en Dockerfile).
- NestJS 11.x.
- TypeScript 5.7.x.
- Mongoose 9.x.
- MongoDB Driver 7.x.
- JWT + Passport.
- Jest 30.x.

### Frontend

- Node.js 20 (imagen usada en Dockerfile).
- Angular 21.1.x.
- TypeScript 5.9.x.
- RxJS 7.8.x.
- Vitest 4.x.
- Nginx Alpine para servir build en produccion.

### Infraestructura

- Docker Compose.
- MongoDB (imagen `mongo`).

## Requisitos de instalacionón y ejecución

- Git.
- Node.js 20 LTS.
- Corepack habilitado.
- pnpm 10.x.
- Docker Desktop o Docker Engine.
- Docker Compose v2.
- Puertos libres:
  - 80 para frontend.
  - 3000 para backend.
  - 27017 para MongoDB.

## Descarga del repositorio

1. Clonar con submodulos:

	```bash
	git clone --recurse-submodules https://github.com/JulianaFranco140/lymon-fullstack.git
	```

2. Entrar al proyecto:

	```bash
	cd lymon-fullstack
	```

3. Si clonaste sin `--recurse-submodules`, inicializarlos:

	```bash
	git submodule update --init --recursive
	```

4. Actualizar submodulos al ultimo commit remoto:

	```bash
	git submodule update --remote --merge
	```

5. Ver commit actual de cada submodulo:

	```bash
	git submodule status
	```

## Variables de entorno para Docker Compose

Crea archivo `.env` en raiz con estas variables:

- `MONGO_USER`
- `MONGO_PASS`
- `MONGO_DB`
- `PORT`
- `isDevelopment`
- `JWT_SECRET`
- `APP_URL`
- `BREVO_API_KEY`
- `SUPPORT_URL`
- `SONAR_TOKEN`

## Ejecucion con Docker

1. Levantar servicios desde raiz:

	```bash
	docker compose up -d
	```

2. Ver estado:

	```bash
	docker compose ps
	```

3. Ver logs:

	```bash
	docker compose logs -f
	```

4. Detener servicios:

	```bash
	docker compose down
	```

Servicios esperados:

- Frontend: http://localhost:80
- Backend: http://localhost:3000
- MongoDB: localhost:27017

## Ejecucion local para desarrollo

### Backend

1. Entrar a carpeta backend:

	```bash
	cd lymon-backend
	```

2. Instalar dependencias:

	```bash
	pnpm install
	```

3. Iniciar en desarrollo:

	```bash
	pnpm run start:dev
	```

### Frontend

1. Entrar a carpeta frontend:

	```bash
	cd lymon-frontend
	```

2. Instalar dependencias:

	```bash
	pnpm install
	```

3. Iniciar en desarrollo:

	```bash
	pnpm start
	```

Nota: frontend en desarrollo consume API en `http://localhost:3000`.

## Build de produccion local

### Backend

```bash
pnpm run build
pnpm run start:prod
```

### Frontend

```bash
pnpm run build
```

## Publicación de imagenes en Docker Hub

### Frontend

```bash
docker build -t julianafranco140/lymon-frontend:latest lymon-frontend
docker push julianafranco140/lymon-frontend:latest
```

### Backend

```bash
docker build -t julianafranco140/lymon-backend:latest lymon-backend
docker push julianafranco140/lymon-backend:latest
```

Si aparece error `tag does not exist`, primero construye o retaggea imagen local con mismo nombre y tag que vas a pushear.

## Notas

- Recomendado fijar versión de MongoDB en `docker-compose.yaml` para evitar cambios inesperados.
- Mantener submodulos sincronizados antes de build y deploy.