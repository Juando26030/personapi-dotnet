#  GUÍA DE EJECUCIÓN DEL PROYECTO personapi-dotnet

## CÓMO EJECUTAR EL PROYECTO

### **Opción 1: Con Docker Compose**

```bash
cd "tu\ruta\personapi-dotnet"
docker-compose up --build
```

**Accesos:**
-  Aplicación Web: http://localhost:32773
-  SQL Server: localhost:1433
-  Usuario SQL: sa
-  Password: Password123!

**Características:**
-  Base de datos SQL Server en contenedor
-  Aplicación .NET en contenedor
-  Datos persistentes en volúmenes Docker
-  Red privada entre contenedores
-  Scripts DDL y DML ejecutados automáticamente

---

### **Opción 2: Ejecución Local**

#### **Paso 1: Verificar SQL Server Local**
Asegurarse de tener SQL Server corriendo localmente en `localhost:1433` o actualizar el connection string en `appsettings.json`.

#### **Paso 2: Ejecutar la aplicación**
```bash
cd "tu\ruta\personapi-dotnet\personapi-dotnet"
dotnet run
```

**Accesos:**
-  Aplicación Web: http://localhost:5000
-  HTTPS: https://localhost:5001

---

### **Opción 3: Modo Development con Swagger**

```bash
cd "tu\ruta\personapi-dotnet\personapi-dotnet"
set ASPNETCORE_ENVIRONMENT=Development
dotnet run
```

**Accesos:**
-  Swagger UI: http://localhost:8080/swagger
-  Aplicación Web: http://localhost:8080
-  API REST: http://localhost:8080/api/[controller]

**Endpoints disponibles:**
- `GET/POST/PUT/DELETE /api/PersonasApi`
- `GET/POST/PUT/DELETE /api/ProfesionesApi`
- `GET/POST/PUT/DELETE /api/TelefonosApi`
- `GET/POST/PUT/DELETE /api/EstudiosApi`

---

##  ESTRUCTURA DE LA BASE DE DATOS

### **Tablas Creadas**

1. **persona**
   - `cc` (BIGINT) - Cédula [PK]
   - `nombre` (NVARCHAR(255))
   - `apellido` (VARCHAR(45))
   - `genero` (NVARCHAR(1)) - M/F
   - `edad` (INT)

2. **profesion**
   - `id` (INT) [PK]
   - `nom` (VARCHAR(90))
   - `des` (TEXT)

3. **telefono**
   - `num` (VARCHAR(15)) [PK]
   - `oper` (VARCHAR(45))
   - `duenio` (BIGINT) [FK → persona.cc]

4. **estudios**
   - `id_prof` (INT) [PK, FK → profesion.id]
   - `cc_per` (BIGINT) [PK, FK → persona.cc]
   - `fecha` (DATE)
   - `univer` (VARCHAR(50))

### **Datos de Prueba**

El proyecto incluye datos de prueba que se insertan automáticamente:
-  3 Personas
-  3 Profesiones
-  3 Teléfonos
-  3 Estudios

---

##  COMANDOS ÚTILES

### **Compilación**
```bash
# Restaurar dependencias
dotnet restore

# Compilar el proyecto
dotnet build

# Compilar en modo Release
dotnet build --configuration Release

# Limpiar compilación
dotnet clean
```

### **Ejecución**
```bash
# Ejecutar en modo Development
dotnet run --environment Development

# Ejecutar en modo Production
dotnet run --environment Production

# Ejecutar con watch (recarga automática)
dotnet watch run
```

### **Docker**
```bash
# Iniciar servicios
docker-compose up

# Iniciar en background
docker-compose up -d

# Detener servicios
docker-compose down

# Ver logs
docker-compose logs -f

# Reconstruir imágenes
docker-compose up --build

# Limpiar todo (incluyendo volúmenes)
docker-compose down -v
```

---

##  FUNCIONALIDADES IMPLEMENTADAS

### **Aplicación Web MVC**
 Página de inicio (Home)
 CRUD completo de Personas
 CRUD completo de Profesiones
 CRUD completo de Teléfonos
 CRUD completo de Estudios
 Navegación entre módulos
 Validación de formularios
 Manejo de errores

### **API REST**
 20 Endpoints RESTful
 Documentación con Swagger/OpenAPI
 Serialización JSON
 Verbos HTTP correctos (GET, POST, PUT, DELETE)
 Códigos de estado HTTP apropiados
 Manejo de errores

### **Base de Datos**
 Entity Framework Core
 Code First approach
 Relaciones entre entidades (1:N, N:M)
Migraciones automáticas
 Scripts de inicialización

---

## URLS IMPORTANTES

### **Con Docker (Puerto 32773)**
| Función | URL |
|---------|-----|
| Página Principal | http://localhost:32773 |
| Personas | http://localhost:32773/Personas |
| Profesiones | http://localhost:32773/Profesiones |
| Teléfonos | http://localhost:32773/Telefonos |
| Estudios | http://localhost:32773/Estudios |

### **Local (Puerto 5000/8080)**
| Función | URL |
|---------|-----|
| Aplicación Web | http://localhost:5000 |
| Swagger UI | http://localhost:8080/swagger |
| API Personas | http://localhost:8080/api/PersonasApi |
| API Profesiones | http://localhost:8080/api/ProfesionesApi |
| API Teléfonos | http://localhost:8080/api/TelefonosApi |
| API Estudios | http://localhost:8080/api/EstudiosApi |

---

## SOLUCIÓN DE PROBLEMAS COMUNES

### **Error: "No se puede conectar a SQL Server"**
**Solución:**
1. Verificar que SQL Server esté corriendo
2. Verificar el connection string en `appsettings.json`
3. Si usas Docker, asegúrate de que el contenedor de SQL Server esté activo:
   ```bash
   docker ps
   ```

### **Error: "Puerto ya en uso"**
**Solución:**
1. Cambiar el puerto en `docker-compose.yml` o `launchSettings.json`
2. Detener el proceso que está usando el puerto:
   ```bash
   netstat -ano | findstr :8080
   taskkill /PID [número] /F
   ```

### **Error: "Entity Framework no puede crear la base de datos"**
**Solución:**
1. Verificar permisos del usuario SQL
2. Ejecutar los scripts DDL manualmente:
   ```bash
   sqlcmd -S localhost -U sa -P Password123! -i db-config/persona_ddl_ms.sql
   ```

### **Error: "Dependencias no restauradas"**
**Solución:**
```bash
cd personapi-dotnet
dotnet restore
dotnet build
```

## TECNOLOGÍAS UTILIZADAS

| Tecnología | Versión | Propósito |
|------------|---------|-----------|
| .NET | 8.0 | Framework principal |
| ASP.NET Core MVC | 8.0 | Interfaz web |
| Entity Framework Core | 8.0.10 | ORM |
| SQL Server | 2022 | Base de datos |
| Docker | Latest | Contenedorización |
| Swagger/OpenAPI | 6.8.1 | Documentación API |
| Bootstrap | 5.x | Estilos UI |
| jQuery | 3.x | JavaScript |

---

## COMANDOS RÁPIDOS

```bash
# INICIO RÁPIDO CON DOCKER
cd "tu\ruta\personapi-dotnet"
docker-compose up --build

# INICIO RÁPIDO LOCAL
cd "tu\ruta\personapi-dotnet\personapi-dotnet"
dotnet run

# INICIO RÁPIDO CON SWAGGER
cd "tu\ruta\personapi-dotnet\personapi-dotnet"
set ASPNETCORE_ENVIRONMENT=Development
dotnet run
```
