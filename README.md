# Laboratorio CI/CD DevSecOps con github-actions

**Apellidos y Nombres:** Choquechambi Quispe Germain Ronald

**Codigo:** 202312345

## Objetivo del laboratorio
Este laboratorio implementa un flujo de trabajo DevSecOps integral para un microservicio basado en Python, priorizando la ejecución "local-first" para agilizar el desarrollo. El proceso inicia con la gestión del código fuente, el cual es empaquetado en un contenedor Docker bajo estándares de seguridad no-root. A través de un pipeline de automatización en GitHub Actions, se ejecutan análisis estáticos de seguridad, pruebas unitarias y escaneos de vulnerabilidades en tiempo real. El ciclo culmina con la preparación de manifiestos para el despliegue en Kubernetes, integrando sondas de salud que garantizan la alta disponibilidad del servicio



---

# Ejercicio 3

## Análisis Estático y Dependencias

Se han integrado comprobaciones de calidad y seguridad directamente en el pipeline para analizar el código antes de su ejecución:

- **SAST (Bandit):** Analiza el código fuente en busca de vulnerabilidades comunes de Python y exporta resultados a `artifacts/bandit.json`.
- **SAST (Semgrep):** Utiliza reglas personalizadas (definidas en `.semgrep.yml`) para prohibir funciones peligrosas como `eval()` y `exec()`, exportando a `artifacts/semgrep.json`.
- **SCA (pip-audit):** Escanea el archivo de requerimientos para detectar dependencias con vulnerabilidades conocidas, guardando el reporte en `artifacts/pip-audit.json`.

---

# Ejercicio 5

## Seguridad y Revisión de Imágenes

La fase de empaquetado se ha reforzado con metadatos y escaneos de infraestructura:

- **Dockerfile Seguro:** Se utiliza una imagen base `slim`, configuración no-root y etiquetas (LABEL) que incluyen el usuario de GitHub y la descripción del servicio.
- **Generación de SBOM:** Se emplea **Syft** para crear un inventario detallado de componentes del proyecto e imagen en `artifacts/`.
- **Escaneo de Vulnerabilidades:** Se utiliza **Grype** para analizar la imagen recién construida, generando reportes en formato SARIF dentro de `artifacts/`.

---

# Ejercicio 5

## Servicio HTTP y Smoke Test

El pipeline verifica la disponibilidad del servicio en tiempo real:

- **Docker Compose:** Levanta el entorno completo en segundo plano durante el workflow.
- **Pruebas de Humo:** Se consulta el endpoint `/health` y se imprime la respuesta JSON en los logs del pipeline para verificar que el servicio esté operando.
- **Pruebas Unitarias:** Se incluye una suite de pruebas con `pytest` que valida códigos de estado y lógica interna del microservicio.

---

# Ejercicio 6

## Orquestación y Endpoint de Salud

Se han actualizado los manifiestos de Kubernetes para garantizar la alta disponibilidad:

- **Liveness Probe:** Detecta si el contenedor se ha bloqueado y lo reinicia automáticamente consultando `/health`.
- **Readiness Probe:** Asegura que el servicio no reciba tráfico hasta que el endpoint `/health` responda positivamente.

---

# Ejercicio 7

## Automatización Local-First

El archivo `Makefile` permite ejecutar el pipeline de forma independiente a GitHub:

- **Encadenamiento:** El comando `make pipeline` ejecuta construcción, pruebas, análisis y empaquetado de forma secuencial.
- **Empaquetado de Evidencias:** El comando `make evidence-pack` genera un archivo comprimido `.tar.gz` en `artifacts/` con marca de tiempo, consolidando reportes y logs.

---

# Ejercicio 8

## Control de Cambios y Gobernanza

Se implementó un archivo `.github/CODEOWNERS` para gestionar la revisión por pares:

- **Revisión Obligatoria:** Garantiza que los cambios en `src/`, `k8s/` o el pipeline sean validados por los responsables designados.
- **Dos Pares de Ojos:** Esta configuración evita que una sola persona fusione cambios críticos, asegurando que al menos dos personas participen en la validación de seguridad.

---

## Ejercicio 9

## Optimización y Rendimiento

Se mejoró la eficiencia del workflow de GitHub Actions:

- **Caché de Python:** Reutiliza las dependencias instaladas, reduciendo el tiempo de ejecución en corridas sucesivas.
- **Gestión de Concurrencia:** Cancela ejecuciones obsoletas en la misma rama para optimizar el uso de recursos.