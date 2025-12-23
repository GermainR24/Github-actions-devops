### Laboratorio CI/CD DevSecOps con github-actions

**Apellidos y Nombres:** Choquechambi Quispe Germain Ronald

**Codigo:** 202312345

## Objetivo del laboratorio
Este laboratorio implementa un flujo de trabajo DevSecOps integral para un microservicio basado en Python, priorizando la ejecución "local-first" para agilizar el desarrollo. El proceso inicia con la gestión del código fuente, el cual es empaquetado en un contenedor Docker bajo estándares de seguridad no-root. A través de un pipeline de automatización en GitHub Actions, se ejecutan análisis estáticos de seguridad, pruebas unitarias y escaneos de vulnerabilidades en tiempo real. El ciclo culmina con la preparación de manifiestos para el despliegue en Kubernetes, integrando sondas de salud que garantizan la alta disponibilidad del servicio