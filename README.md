# Despliegue y CI/CD en Railway

![CI/CD Status](https://github.com/jerc31/rozo-post2-u12/actions/workflows/ci.yml/badge.svg)

## Autor

**Nombre:** Jhoseth Esneider Rozo Carrillo  
**Código:** 02230131027  
**Programa:** Ingeniería de Sistemas  
**Unidad:** Unidad 12 – Despliegue y CI/CD
**Actividad:** Post-Contenido 2
**Fecha:** 16/05/2026

---

## Descripción del Proyecto

Este proyecto implementa un pipeline completo de Integración Continua y Entrega Continua (CI/CD) usando GitHub Actions. A partir de la contenedorización de una aplicación Spring Boot mediante Docker y su orquestación local con Docker Compose (Post 1), se ha añadido la automatización de pruebas y publicación.

Se integran mecanismos modernos como:

- **GitHub Actions** para el pipeline de CI/CD.
- **JaCoCo** para el reporte de cobertura de pruebas unitarias.
- **Docker Hub** para la publicación de la imagen multi-stage.
- **GitHub Secrets** para el manejo seguro de credenciales.

El objetivo es garantizar que cada cambio en el repositorio principal pase por pruebas automatizadas y genere una imagen lista para producción.

---

## Pipeline CI/CD

El pipeline se activa automáticamente en cada push a la rama `main` y realiza:

1. Compilación con Maven y ejecución de pruebas unitarias.
2. Generación de reporte de cobertura JaCoCo como artefacto.
3. Construcción de imagen Docker con multi-stage build.
4. Publicación de la imagen en Docker Hub con tags `latest` y `sha-<commit>`.

## Imagen Docker

Puedes descargar la imagen pública generada por el pipeline directamente desde Docker Hub (reemplaza `<usuario>` por el usuario de Docker Hub configurado en los Secrets):

```bash
docker pull <usuario>/mi-spring-app:latest
docker run -p 8080:8080 -e SPRING_PROFILES_ACTIVE=dev <usuario>/mi-spring-app:latest
```

---

## Prerrequisitos (CI/CD)

Para que el pipeline funcione en tu repositorio fork/clonado, debes configurar los siguientes GitHub Secrets en `Settings -> Secrets and variables -> Actions`:

- `DOCKERHUB_USERNAME`: Tu usuario de Docker Hub.
- `DOCKERHUB_TOKEN`: Tu Access Token de Docker Hub (NO usar la contraseña principal).

---

## Ejecución Local del Proyecto

1. Clonar repositorio:
   ```bash
   git clone https://github.com/jerc31/rozo-post2-u12.git
   cd rozo-post2-u12
   ```

2. Ejecutar pruebas locales y generar reporte JaCoCo:
   ```bash
   ./mvnw clean verify
   ```

3. Ejecutar con Docker Compose (como en Post 1):
   ```bash
   docker compose up -d --build
   ```

---

## Checkpoints y Verificaciones

✓ **Checkpoint 1: Configuración de Secrets**
- *Se han configurado los GitHub Secrets `DOCKERHUB_USERNAME` y `DOCKERHUB_TOKEN` en el repositorio.*

✓ **Checkpoint 2: Ejecución de Job de CI**
- *El job `build-and-test` compila y ejecuta la prueba unitaria sin errores. El reporte JaCoCo queda como artefacto en GitHub Actions.*

✓ **Checkpoint 3: Job de CD y Docker Hub**
- *El job `docker-publish` sube exitosamente la imagen a Docker Hub. El Badge de GitHub Actions muestra estado verde en este README.*

---

## Evidencias

Las siguientes evidencias se encuentran en la carpeta `/evidencias/`:

- Captura del historial de GitHub Actions con los jobs exitosos.
- Captura del reporte JaCoCo descargable.
- Captura de Docker Hub con los tags `latest` y `sha-XXXX` de la imagen.
