# config-server

Repositorio con la configuración centralizada de LogiTrack. Lo lee el Spring Cloud Config Server (`LogiRock-ConfigServer`).

## Variables de entorno obligatorias

Este repo es público: **nunca** escribas valores reales acá. Cada servicio toma los secretos de variables de entorno (en Docker, del `.env` de `LogiRock-ConfigServer/orquestacion`).

| Variable | La usa | Descripción |
|---|---|---|
| `DB_USERNAME` | shipment, courier, security | Usuario de MySQL |
| `DB_PASSWORD` | shipment, courier, security | Contraseña de MySQL |
| `JWT_SECRET_BASE64` | shipment, courier, security | Clave HS256 en Base64 (32 bytes o más). Generarla con `openssl rand -base64 32` |
| `JWT_ISSUER` | shipment, courier, security | Emisor del token. Tiene que ser el mismo en todos los servicios |
| `ADMIN_USERNAME` | security | Usuario del administrador inicial |
| `ADMIN_PASSWORD` | security | Contraseña del administrador inicial |

Si falta alguna, el servicio no arranca y muestra `Could not resolve placeholder '<VARIABLE>'`.

## Perfiles

| Archivo | Se aplica a |
|---|---|
| `application.yml` | Todos los servicios (issuer JWT, Actuator, Eureka, Feign, Swagger) |
| `<servicio>.yml` | Ese servicio, en cualquier perfil |
| `<servicio>-dev.yml` | Ese servicio con `SPRING_PROFILES_ACTIVE=dev` (SQL visible, logs DEBUG) |
| `<servicio>-prod.yml` | Ese servicio con `SPRING_PROFILES_ACTIVE=prod` (logs INFO) |

Swagger queda activo en los dos perfiles porque se usa en las sprint reviews.

## Variables opcionales (tienen valor por defecto)

| Variable | Default | Para qué |
|---|---|---|
| `COURIER_SERVICE_URL` | `http://courier-service:8082` | shipment → courier |
| `SECURITY_SERVICE_URL` | `http://security-service:8081` | shipment y courier → security |
| `EUREKA_URL` | `http://eureka-server:8065/eureka/` | Registro en Eureka (desde la J4) |
| `DB_HOST`, `DB_PORT`, `DB_DATABASE_NAME` | según el servicio | Conexión a MySQL fuera de Docker |

Si corrés un servicio desde IntelliJ (fuera de Docker), definí `COURIER_SERVICE_URL=http://localhost:8082` y `SECURITY_SERVICE_URL=http://localhost:8081`.