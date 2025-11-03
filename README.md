# PersonAPI - .NET 8.0

![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?style=flat-square&logo=dotnet)
![SQL Server](https://img.shields.io/badge/SQL%20Server-2022-CC2927?style=flat-square&logo=microsoftsqlserver)
![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED?style=flat-square&logo=docker)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

API RESTful desarrollada con ASP.NET Core 8.0 para la gestión de personas, profesiones, estudios y teléfonos. Incluye interfaces web con vistas MVC y endpoints API con documentación Swagger.

## 👥 Integrantes

- Jonathan Jurado
- Juan David Ramirez
## 📋 Tabla de Contenidos

- [Características](#-características)
- [Tecnologías](#-tecnologías)
- [Arquitectura](#-arquitectura)
- [Requisitos Previos](#-requisitos-previos)
- [Instalación](#-instalación)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Base de Datos](#-base-de-datos)
- [API Endpoints](#-api-endpoints)
- [Configuración](#-configuración)
- [Documentación](#-documentación)
- [Contribuir](#-contribuir)
- [Licencia](#-licencia)

## Características

- **API RESTful** completa con operaciones CRUD
- **Interfaz Web MVC** con vistas Razor
- **Documentación Swagger/OpenAPI** integrada
- **Entity Framework Core** para acceso a datos
- **SQL Server** como base de datos
- **Docker & Docker Compose** para despliegue
- **Arquitectura por capas** (Controllers, Models, Views)
- **Validaciones** de datos
- **Relaciones** entre entidades

## 🚀 Tecnologías

### Backend
- **ASP.NET Core 8.0** - Framework web
- **Entity Framework Core 8.0.10** - ORM
- **Swashbuckle.AspNetCore 6.8.1** - Documentación API

### Base de Datos
- **SQL Server 2022** - Base de datos relacional

### DevOps
- **Docker** - Contenedores
- **Docker Compose** - Orquestación

## 🏗️ Arquitectura

El proyecto sigue el patrón **MVC (Model-View-Controller)** con una capa adicional de API REST:

```
personapi-dotnet/
├── Controllers/           # Controladores MVC
│   ├── PersonasController.cs
│   ├── ProfesionesController.cs
│   ├── EstudiosController.cs
│   ├── TelefonosController.cs
│   └── Api/              # Controladores API REST
│       ├── PersonasApiController.cs
│       ├── ProfesionesApiController.cs
│       ├── EstudiosApiController.cs
│       └── TelefonosApiController.cs
├── Models/               # Modelos y Entidades
│   └── Entities/
│       ├── Persona.cs
│       ├── Profesion.cs
│       ├── Estudio.cs
│       ├── Telefono.cs
│       └── PersonaDbContext.cs
├── Views/                # Vistas Razor
│   ├── Personas/
│   ├── Profesiones/
│   ├── Estudios/
│   └── Telefonos/
└── wwwroot/              # Archivos estáticos
```

## 📦 Requisitos Previos

- [.NET 8.0 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Git](https://git-scm.com/)

**O simplemente Docker** si vas a ejecutar con contenedores.

## 🔧 Instalación

### Opción 1: Con Docker (Recomendado)

1. **Clonar el repositorio:**
```bash
git clone https://github.com/Juando26030/personapi-dotnet.git
cd personapi-dotnet
```

2. **Levantar los servicios:**
```bash
docker-compose up --build
```

3. **Acceder a la aplicación:**
   - Web MVC: http://localhost:32773
   - API Swagger: http://localhost:32773/swagger

### Opción 2: Desarrollo Local

1. **Clonar el repositorio:**
```bash
git clone https://github.com/Juando26030/personapi-dotnet.git
cd personapi-dotnet
```

2. **Configurar SQL Server:**
   - Instalar SQL Server localmente o usar Docker:
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Password123!" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
```

3. **Ejecutar scripts de base de datos:**
```bash
# Ejecutar persona_ddl_ms.sql y persona_dml_ms.sql en SQL Server
```

4. **Actualizar cadena de conexión** en `appsettings.json`:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost,1433;User ID=sa;Password=Password123!;Database=persona_db;Trusted_Connection=False;TrustServerCertificate=True"
  }
}
```

5. **Restaurar dependencias y ejecutar:**
```bash
cd personapi-dotnet
dotnet restore
dotnet run
```

6. **Acceder a la aplicación:**
   - Web MVC: http://localhost:8080
   - API Swagger: http://localhost:8080/swagger

## 📁 Estructura del Proyecto

```
personapi-dotnet/
│
├── docker-compose.yml              # Orquestación de contenedores
├── MANUAL_EJECUCION.md            # Manual de ejecución detallado
├── README.md                       # Este archivo
│
├── db-config/                      # Configuración de base de datos
│   ├── Dockerfile                  # Dockerfile para SQL Server
│   ├── entrypoint.sh              # Script de inicialización
│   ├── persona_ddl_ms.sql         # Definición de esquema
│   └── persona_dml_ms.sql         # Datos iniciales
│
└── personapi-dotnet/              # Aplicación principal
    ├── Controllers/                # Controladores MVC y API
    ├── Models/                     # Modelos y entidades
    ├── Views/                      # Vistas Razor
    ├── wwwroot/                    # Archivos estáticos
    ├── Program.cs                  # Punto de entrada
    ├── appsettings.json           # Configuración
    └── Dockerfile                  # Dockerfile de la aplicación
```

## 🗄️ Base de Datos

### Modelo Entidad-Relación

```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│   PERSONA   │────────<│   ESTUDIOS   │>────────│  PROFESION  │
├─────────────┤         ├──────────────┤         ├─────────────┤
│ cc (PK)     │         │ id_prof (FK) │         │ id (PK)     │
│ nombre      │         │ cc_per (FK)  │         │ nom         │
│ apellido    │         │ fecha        │         │ des         │
│ genero      │         │ univer       │         └─────────────┘
│ edad        │         └──────────────┘
└─────────────┘
       │
       │
       │ 1:N
       ↓
┌─────────────┐
│  TELEFONO   │
├─────────────┤
│ num (PK)    │
│ oper        │
│ duenio (FK) │
└─────────────┘
```

### Entidades

#### Persona
- **cc** (BIGINT): Cédula de ciudadanía - PK
- **nombre** (VARCHAR): Nombre
- **apellido** (VARCHAR): Apellido
- **genero** (CHAR): Género (M/F)
- **edad** (INT): Edad

#### Profesion
- **id** (INT): Identificador - PK
- **nom** (VARCHAR): Nombre de la profesión
- **des** (TEXT): Descripción

#### Estudios
- **id_prof** (INT): ID Profesión - PK, FK
- **cc_per** (BIGINT): Cédula persona - PK, FK
- **fecha** (DATE): Fecha de graduación
- **univer** (VARCHAR): Universidad

#### Telefono
- **num** (VARCHAR): Número telefónico - PK
- **oper** (VARCHAR): Operador
- **duenio** (BIGINT): Cédula dueño - FK

## 🔌 API Endpoints

### Personas

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/personas` | Obtener todas las personas |
| GET | `/api/personas/{cc}` | Obtener persona por cédula |
| POST | `/api/personas` | Crear nueva persona |
| PUT | `/api/personas/{cc}` | Actualizar persona |
| DELETE | `/api/personas/{cc}` | Eliminar persona |

### Profesiones

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/profesiones` | Obtener todas las profesiones |
| GET | `/api/profesiones/{id}` | Obtener profesión por ID |
| POST | `/api/profesiones` | Crear nueva profesión |
| PUT | `/api/profesiones/{id}` | Actualizar profesión |
| DELETE | `/api/profesiones/{id}` | Eliminar profesión |

### Estudios

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/estudios` | Obtener todos los estudios |
| GET | `/api/estudios/{idProf}/{ccPer}` | Obtener estudio específico |
| POST | `/api/estudios` | Crear nuevo estudio |
| PUT | `/api/estudios/{idProf}/{ccPer}` | Actualizar estudio |
| DELETE | `/api/estudios/{idProf}/{ccPer}` | Eliminar estudio |

### Teléfonos

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/telefonos` | Obtener todos los teléfonos |
| GET | `/api/telefonos/{num}` | Obtener teléfono por número |
| POST | `/api/telefonos` | Crear nuevo teléfono |
| PUT | `/api/telefonos/{num}` | Actualizar teléfono |
| DELETE | `/api/telefonos/{num}` | Eliminar teléfono |

### Ejemplos de Uso

#### Crear una Persona
```bash
curl -X POST "http://localhost:32773/api/personas" \
  -H "Content-Type: application/json" \
  -d '{
    "cc": 1234567890,
    "nombre": "Juan",
    "apellido": "Pérez",
    "genero": "M",
    "edad": 30
  }'
```

#### Obtener todas las Personas
```bash
curl -X GET "http://localhost:32773/api/personas"
```

## ⚙️ Configuración

### Variables de Entorno (Docker)

El archivo `docker-compose.yml` define las siguientes variables:

```yaml
environment:
  - DB_SERVER=sqlserver_container
  - DB_NAME=persona_db
  - DB_PORT=1433
  - SA_PASSWORD=Password123!
```

### Cadena de Conexión

En `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=sqlserver_container,1433;User ID=sa;Password=Password123!;Database=persona_db;Trusted_Connection=False;TrustServerCertificate=True"
  }
}
```

### Puerto de Aplicación

La aplicación escucha en el puerto **8080** dentro del contenedor y se mapea al puerto **32773** en el host.

## 📚 Documentación

### Swagger UI

Accede a la documentación interactiva de la API en modo desarrollo:

**URL:** http://localhost:32773/swagger (con Docker) o http://localhost:8080/swagger (local)

Swagger proporciona:
- 📖 Documentación completa de endpoints
- 🧪 Interfaz para probar la API
- 📝 Esquemas de datos
- 🔍 Validaciones y ejemplos

**Nota:** Para habilitar Swagger en modo local, ejecuta:
```powershell
$env:ASPNETCORE_ENVIRONMENT = "Development"
dotnet run
```

### Vistas Web

La aplicación incluye una interfaz web completa con operaciones CRUD:

- **Personas:** http://localhost:32773/Personas
- **Profesiones:** http://localhost:32773/Profesiones
- **Estudios:** http://localhost:32773/Estudios
- **Teléfonos:** http://localhost:32773/Telefonos

## 🧪 Testing

Para ejecutar pruebas (si están implementadas):

```bash
dotnet test
```

## 🔐 Seguridad

- Las contraseñas de base de datos deben cambiarse en producción
- Implementar autenticación JWT para la API
- Configurar CORS según necesidades
- Usar HTTPS en producción
- Implementar rate limiting

## 🚀 Despliegue

### Docker Hub

```bash
# Build
docker build -t personapi-dotnet:latest ./personapi-dotnet

# Tag
docker tag personapi-dotnet:latest username/personapi-dotnet:latest

# Push
docker push username/personapi-dotnet:latest
```

### Azure App Service

1. Crear App Service con soporte para contenedores
2. Configurar Azure SQL Database
3. Actualizar cadena de conexión
4. Desplegar desde Docker Hub o Azure Container Registry

## 🤝 Contribuir

1. Fork del proyecto
2. Crear una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit de tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abrir un Pull Request

### Estándares de Código

- Seguir convenciones de C# (.NET)
- Comentar código complejo
- Escribir tests para nuevas funcionalidades
- Actualizar documentación

## 📝 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

## 👥 Autores

* **Camila Castro Neiza** - *Desarrollo*
* **Juan David Barajas Urrea** - *Desarrollo*
* **Juando26030** - *Repository* - [GitHub](https://github.com/Juando26030)

## 🙏 Agradecimientos

- ASP.NET Core Team
- Entity Framework Core Team
- Comunidad de .NET

## 📞 Contacto

Para preguntas o soporte:
- GitHub Issues: [Crear Issue](https://github.com/Juando26030/personapi-dotnet/issues)

---

⭐️ **Si este proyecto te fue útil, considera darle una estrella en GitHub** ⭐️

