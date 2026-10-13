# test-manager-library-3145556-infra

> **Prueba técnica · Ficha ADSO 3145556**
> Entrega: **martes 13 de octubre de 2026, a las 5:00 p. m.** (17:00, hora Colombia).

Este repositorio es la **infraestructura** de *Manager Library*: contenedores, red, volúmenes y el motor de base de datos vacío. **Empieza vacío a propósito**: se construye en el fork de cada aprendiz.

## Instrucciones

Cada aprendiz debe **crear el fork** de los seis repositorios del proyecto y **resolver el proyecto
con el spec planteado**.

1. Hacer fork, a su cuenta de GitHub, de cada repositorio de la tabla del final.
2. Leer el spec en [`test-manager-library-3145556-docs`](https://github.com/code-sena/test-manager-library-3145556-docs).
   Se entrega en dos versiones: `spec-python/` y `spec-.net/`.
3. Desarrollar en los forks.

## El reto se desarrolla con React y PHP (Laravel)

El spec está escrito para Python y para .NET, pero el reto **no** se hace en esos lenguajes:

| Capa | Tecnología del reto |
|---|---|
| Frontend | React |
| Backend | PHP con Laravel |

Lo que el spec define sobre el negocio —historias, criterios de aceptación, reglas, contrato de la
API, modelo de datos— se respeta. Lo que define sobre la tecnología se traduce a React y Laravel.

## La prueba no consiste en escribir el código

El propósito principal es ver la **capacidad de desempeño con SDD** (*Spec-Driven Development*,
desarrollo guiado por especificación): cómo se lee, se interpreta y se aplica una especificación
para llevarla a un stack distinto. El código es el medio, no el fin.

## Entrega

Cuenta lo que quede **subido a los forks antes de las 5:00 p. m.** (17:00). Lo que llegue después no se revisa.
No hay prórroga: suban avances parciales con commits pequeños durante la prueba.

## Los seis repositorios

| Repositorio | Qué va ahí |
|---|---|
| [`test-manager-library-3145556-docs`](https://github.com/code-sena/test-manager-library-3145556-docs) | El spec: `spec-python/` y `spec-.net/` |
| [`test-manager-library-3145556-api`](https://github.com/code-sena/test-manager-library-3145556-api) | Backend en PHP (Laravel) |
| [`test-manager-library-3145556-app`](https://github.com/code-sena/test-manager-library-3145556-app) | Frontend en React |
| [`test-manager-library-3145556-page`](https://github.com/code-sena/test-manager-library-3145556-page) | Sitio público estático de presentación |
| [`test-manager-library-3145556-infra`](https://github.com/code-sena/test-manager-library-3145556-infra) | Contenedores, red, volúmenes y motor de base de datos vacío |
| [`test-manager-library-3145556-tool`](https://github.com/code-sena/test-manager-library-3145556-tool) | Utilidades: sembrador de datos de demostración |

## Entorno integrado

Requiere Docker y Docker Compose. Los seis repositorios deben ser hermanos.
PostgreSQL 16 inicia sin tablas de negocio: **solo el backend ejecuta las
migraciones y la semilla de categorías**. Después provisiona el administrador
desde las credenciales del entorno. Nunca se montan scripts SQL de inicialización.

```powershell
Copy-Item .env.example .env
# Completar DB_PASSWORD, JWT_SECRET (mínimo 32 bytes), ADMIN_USERNAME,
# ADMIN_PASSWORD y APP_KEY (base64: seguido de una clave de 32 bytes).
docker compose config --quiet
docker compose up --build -d
docker compose ps
curl.exe --fail http://127.0.0.1:8000/api/v1/health
```

Aplicación: `http://localhost:8080`. Sitio público: `http://localhost:8081`.
La aplicación usa un proxy `/api` a Laravel; no expone credenciales de base de
datos al navegador. PostgreSQL no publica un puerto al host. Los datos quedan
en el volumen del proyecto; `docker compose stop` conserva los datos. **No usar
`down -v`, `migrate:fresh` ni rollback sobre datos compartidos.**

Este entorno local usa el servidor de desarrollo de Laravel; no es una
configuración de producción ni una promesa de la meta de rendimiento RNF-1.
El contenedor independiente `test-manager-library-pg16`, si existe, es exclusivo
de pruebas y no forma parte de este entorno ni comparte sus volúmenes.
