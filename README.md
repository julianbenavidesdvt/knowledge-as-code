# Knowledge as Code

Este proyecto es una aplicación web moderna construida siguiendo una arquitectura robusta y escalable. El objetivo es gestionar "conocimiento como código", utilizando un stack tecnológico de vanguardia.

## Tecnologías

El proyecto utiliza las siguientes tecnologías principales:

### Backend
- **Framework**: [NestJS](https://nestjs.com/) (TypeScript)
- **Base de Datos**: PostgreSQL (Cloud SQL)
- **ORM**: Sequelize
- **Autenticación**: JWT / Estrategias de Passport

### Frontend
- **Framework**: [Angular](https://angular.io/) (Versión más reciente)
- **Estilos**: [Tailwind CSS](https://tailwindcss.com/)
- **Gestión de Estado**: Signals / RxJS

### Infraestructura y DevOps
- **Nube**: Google Cloud Platform (GCP)
- **Documentación**: Context7

## Estructura del Proyecto

El repositorio está organizado en dos directorios principales:

```bash
knowledge-as-code/
├── backend/            # Aplicación API NestJS
└── frontend/           # Aplicación Cliente Angular
```

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- [Node.js](https://nodejs.org/) (Versión LTS recomendada)
- [npm](https://www.npmjs.com/)
- [PostgreSQL](https://www.postgresql.org/) (para entorno local)
- [Google Cloud SDK](https://cloud.google.com/sdk) (opcional, para despliegue)

## Primeros Pasos

### 1. Clonar el repositorio

```bash
git clone <url-del-repositorio>
cd knowledge-as-code
```

### 2. Configuración del Backend

Navega al directorio del backend e instala las dependencias:

```bash
cd backend
npm install
```

Crea un archivo `.env` en la raíz del directorio `backend` con las variables de entorno necesarias (base de datos, claves secretas, etc.).

Para ejecutar el servidor en modo desarrollo:

```bash
npm run start:dev
```

El servidor backend estará corriendo generalmente en `http://localhost:3000`.

### 3. Configuración del Frontend

En una nueva terminal, navega al directorio del frontend e instala las dependencias:

```bash
cd frontend
npm install
```

Para servir la aplicación en modo desarrollo:

```bash
npm run start
```

La aplicación frontend estará disponible en `http://localhost:4200`.

## Scripts Comunes

- `npm run start:dev` (Backend): Inicia el servidor de desarrollo NestJS.
- `npm start` (Frontend): Inicia el servidor de desarrollo Angular.
- `npm run build`: Construye la aplicación para producción.
- `npm run test`: Ejecuta las pruebas unitarias.

## Contribución

1. Haz un Fork del proyecto.
2. Crea tu rama de funcionalidad (`git checkout -b feature/AmazingFeature`).
3. Haz Commit de tus cambios (`git commit -m 'Add some AmazingFeature'`).
4. Haz Push a la rama (`git push origin feature/AmazingFeature`).
5. Abre un Pull Request.

## Licencia

Distribuido bajo la licencia MIT. Ver `LICENSE` para más información.
