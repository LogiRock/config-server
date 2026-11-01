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