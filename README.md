## Laboratorio #4 – REST API Blueprints (Java 21 / Spring Boot 3.3.x)
# Escuela Colombiana de Ingeniería – Arquitecturas de Software  

---

## 📋 Requisitos
- Java 21
- Maven 3.9+
- PostgreSQL (Instalado o vía Docker)

## ▶️ Ejecución del proyecto
Para compilar y ejecutar la aplicación:
```bash
mvn clean install
mvn spring-boot:run
```

### Probar con `curl`:
*(Nota: Hemos actualizado la API a la versión `/api/v1/`)*

**Obtener todos los planos:**
```bash
curl -s http://localhost:8080/api/v1/blueprints
```

**Obtener planos de un autor:**
```bash
curl -s http://localhost:8080/api/v1/blueprints/john
```

**Crear un nuevo plano:**
```bash
curl -i -X POST http://localhost:8080/api/v1/blueprints \
     -H 'Content-Type: application/json' \
     -d '{"author":"john","name":"kitchen","points":[{"x":1,"y":1}]}'
```

**Agregar un punto a un plano:**
```bash
curl -i -X PUT http://localhost:8080/api/v1/blueprints/john/kitchen/points \
     -H 'Content-Type: application/json' \
     -d '{"x":3,"y":3}'
```

---

## 🛠️ Documentación y Monitoreo (Swagger & Actuator)
- **Swagger UI (Interactivo):** [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)
- **OpenAPI JSON:** [http://localhost:8080/v3/api-docs](http://localhost:8080/v3/api-docs)
- **Actuator Health:** [http://localhost:8080/actuator/health](http://localhost:8080/actuator/health)

---

## 🗂️ Estructura de carpetas
```
src/main/java/edu/eci/arsw/blueprints
  ├── model/         # Entidades: Blueprint, Point
  ├── persistence/   # Repositorios (InMemory, Postgres)
  ├── services/      # Lógica de negocio
  ├── filters/       # Filtros (Identity, Redundancy, Undersampling)
  ├── controllers/   # REST Controllers
  └── config/        # Configuración (OpenApiConfig)
```

---

## 📊 Pruebas y Cobertura
Para ejecutar las pruebas unitarias y ver el reporte de JaCoCo (>90%):
```bash
mvn clean test
```
El reporte detallado se genera en: `target/site/jacoco/index.html`

---

## 📖 Resumen de Implementación
- **Persistencia Dual**: Soporte para memoria y PostgreSQL (activado por defecto).
- **ApiResponse**: Todas las respuestas siguen un formato JSON estandarizado.
- **Filtros por Perfiles**: 
  - Activar redundancia: `-Dspring-boot.run.profiles=redundancy`
  - Activar submuestreo: `-Dspring-boot.run.profiles=undersampling`


## SOLUCION ✅💻

### *1. Introducción*

En este laboratorio se implementó una API REST para la gestión de Blueprints utilizando
Java 21 y Spring Boot 3.3.x. La aplicación permite crear, consultar y actualizar blueprints,
almacenando la información en una base de datos PostgreSQL y documentando los endpoints
mediante Swagger/OpenAPI.

### *2. Instrucciones para ejecutar el proyecto*

 #### *2.1. 1. Levantar la base de datos PostgreSQL*
 
Se utilizó PostgreSQL en Docker con el siguiente comando:

```bash
docker run -d -- name postgres - blueprints \
-e POSTGRES_DB = blueprints \
-e POSTGRES_USER = postgres \
-e POSTGRES_PASSWORD = postgres \
-p 5432:5432 postgres

```

#### *2.2. 2. Compilar y ejecutar la aplicación*

Desde el directorio del proyecto:

```bash
mvn clean install
mvn spring-boot:run
```

#### *2.3. 3. Acceder a Swagger UI*

Abrir en el navegador:

```bash
http :// localhost :8080/ swagger - ui / index . html
```

Desde allí se pueden probar todos los endpoints de la API

### 3. Evidencia de consultas en Swagger UI

#### *3.1. Creación de Blueprint (POST)*

Se realizó una petición POST al endpoint:

```bash
/ api / v1 / blueprints

```

Obteniendo como respuesta el código HTTP 201 Created.

<img width="921" height="486" alt="image" src="https://github.com/user-attachments/assets/b9b69b11-269e-4bd9-8413-4231d4c799bc" />

<img width="921" height="307" alt="image" src="https://github.com/user-attachments/assets/68e28a96-bd5f-41d5-b5f0-fbf5ddc7ee5a" />

#### *3.2. Consulta de Blueprints por autor (GET)*

Se realizó una petición GET al endpoint:

```bash
/ api / v1 / blueprints /{ author }

```

Obteniendo como respuesta el código HTTP 200 OK

<img width="909" height="503" alt="image" src="https://github.com/user-attachments/assets/b3509efc-b7c5-4ce5-bdec-90265626aa98" />

<img width="912" height="307" alt="image" src="https://github.com/user-attachments/assets/bb8c2df2-ab35-4f47-a337-779d60448ef4" />

### 4. Evidencia de persistencia en base de datos

Luego de realizar las peticiones desde Swagger, se verificó directamente en PostgreSQL
que la información fue almacenada correctamente.

#### *4.1. Tabla blueprint*

<img width="941" height="361" alt="image" src="https://github.com/user-attachments/assets/7339a400-6ddf-4f00-9753-bb7e0b0961b8" />

### 5. Verificación y Calidad del Proyecto

#### *5.1. Análisis de calidad con SonarQube*

Se ejecutó análisis estático de código mediante SonarQube utilizando el siguiente comando:


```bash
mvn clean verify sonar : sonar \
- Dsonar . projectKey = arsw -2024 -1 - g6 -2 _to -do - code - REST - API - Blueprints -
lab \
- Dsonar . login = tokent

```
El análisis permitió evaluar métricas de calidad y cobertura de pruebas del proyecto

<img width="931" height="383" alt="image" src="https://github.com/user-attachments/assets/dea2f164-0227-473e-b75e-093ccddfe7c1" />

#### *5.2. Cobertura de pruebas con JaCoCo*

Se utilizó JaCoCo para generar el reporte de cobertura de pruebas del proyecto mediante
el siguiente comando:

```bash
mvn clean test jacoco : report

```

El reporte generado permite visualizar el porcentaje de cobertura por clases y métodos.

<img width="781" height="440" alt="image" src="https://github.com/user-attachments/assets/e34a8263-0f3c-4945-8a69-03adf14bda85" />

### 6. Diagramas

#### *6.1. Diagrama de Componentes*

El siguiente diagrama muestra la arquitectura por capas implementada en la API, evidenciando la separación entre controlador, servicio, persistencia y base de datos.

<img width="508" height="496" alt="image" src="https://github.com/user-attachments/assets/5f4f57c0-d8d6-49df-94cf-d091873f8598" />

#### *6.2. Diagrama de Flujo*

El siguiente diagrama representa el flujo general de una petición HTTP desde el cliente
hasta la persistencia en base de datos y la generación de la respuesta.

<img width="527" height="422" alt="image" src="https://github.com/user-attachments/assets/7cc3126a-60ad-4f7c-86e4-f644e37a43d2" />



### 7. Buenas prácticas aplicadas

Durante el desarrollo del laboratorio se aplicaron las siguientes buenas prácticas:

- Versionamiento de API mediante el prefijo /api/v1.
- Uso correcto de códigos HTTP (200, 201, 400, 404).
- Implementación de una clase ApiResponse para estandarizar respuestas.
- Manejo global de excepciones con @RestControllerAdvice.
- Separación por capas (controlador, servicio, persistencia).
