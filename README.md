# Pipeline CI/CD con Express + Vite + React + GitHub Actions

Este proyecto implementa un **pipeline completo de CI/CD** con:

- **Backend**: API REST con Express.js
- **Frontend**: Interfaz con Vite + React
- **CI/CD**: GitHub Actions para:
  - Instalación de dependencias
  - Ejecución de tests
  - Linting
  - Build del frontend
  - Deploy automático a GitHub Pages
  - Construcción y push de imagen Docker del backend a Docker Hub

## Estructura del Proyecto

```
mi-proyecto-ci-cd/
├── backend/                 # API Express
│   ├── src/
│   │   └── index.js
│   ├── Dockerfile
│   ├── .dockerignore
│   └── package.json
├── frontend/                # Aplicación Vite + React
│   ├── src/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
├── .github/
│   └── workflows/
│       └── ci-cd.yml       # Workflow de GitHub Actions
├── .gitignore
└── README.md
```

## Requisitos Previos

- Node.js 18+
- Git
- Cuenta en GitHub
- Cuenta en Docker Hub (opcional, para publicar imagen)

## Setup Local

### 1. Clonar el repositorio

```bash
cd c:\Users\Carol\Downloads\Pipeline
```

### 2. Instalar dependencias del backend

```bash
cd backend
npm install
```

### 3. Instalar dependencias del frontend

```bash
cd ../frontend
npm install
```

## Ejecutar Localmente

### Backend

```bash
cd backend
npm run dev
```

El servidor estará disponible en `http://localhost:4000`

### Frontend (otra terminal)

```bash
cd frontend
npm run dev
```

La aplicación estará disponible en `http://localhost:5173`

## Build del Proyecto

### Backend (Docker)

```bash
cd backend
docker build -t tu-usuario/backend-express-ci-cd:latest .
docker run -p 4000:4000 tu-usuario/backend-express-ci-cd:latest
```

### Frontend

```bash
cd frontend
npm run build
```

Los archivos compilados estarán en `frontend/dist/`

## Configurar GitHub Actions

### 1. Crear Secretos en GitHub

Ve a **Settings → Secrets and variables → Actions** y añade:

- `DOCKERHUB_USERNAME`: Tu usuario de Docker Hub
- `DOCKERHUB_TOKEN`: Token de acceso de Docker Hub (crea uno en https://hub.docker.com/settings/security)

### 2. Configurar GitHub Pages

Ve a **Settings → Pages** y selecciona:
- **Branch**: `gh-pages`
- **Folder**: `/ (root)`

El frontend se desplegará en: `https://tu-usuario.github.io/mi-proyecto-ci-cd-express-vite`

## Pipeline CI/CD

El archivo `.github/workflows/ci-cd.yml` contiene 3 jobs:

### Job 1: CI (Continuous Integration)
- Instala dependencias de backend y frontend
- Ejecuta tests
- Ejecuta linting
- Build del frontend
- Sube artefacto del build

### Job 2: Deploy Frontend (CD)
- Descarga el artefacto del build
- Publica en GitHub Pages

### Job 3: Build & Push Backend
- Construye imagen Docker
- Pushea a Docker Hub

## Próximos Pasos

1. Inicializa Git y sube a GitHub
2. Configura los secretos de Docker Hub
3. Habilita GitHub Pages
4. Realiza un push a `main` para activar el pipeline
5. Monitorea las acciones en la pestaña **Actions** de GitHub

## API Endpoints

### GET `/api/saludo`

```bash
curl http://localhost:4000/api/saludo
```

**Response:**
```json
{
  "mensaje": "Hola desde el backend con Express + CI/CD 🚀"
}
```

## Problemas Comunes

### El frontend no conecta con el backend

En producción (GitHub Pages), ajusta la URL del backend en `frontend/src/App.jsx` a la URL real de tu backend desplegado.

### Docker push falla

Verifica que:
1. Los secretos `DOCKERHUB_USERNAME` y `DOCKERHUB_TOKEN` estén configurados
2. El token tenga permisos de lectura y escritura en Docker Hub

### GitHub Pages no actualiza

- Verifica que la rama `gh-pages` esté configurada en Settings
- Espera 1-2 minutos después del push

## Licencia

MIT
