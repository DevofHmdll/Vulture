Título: Modelos de Amenazas dentro del sistema Vulture

Autor: Ernesto Miguel Alfonso Campos 
Fecha: 20/01/26
Versión: 0.5 draft(Github)
Licencia: GNU GPL v3


---
# Objetivo del Documento
El documento es redactado con el fin de proponer la proyección del sistema frente a diferentes tipos de amenazas. El documento, en su estructura presenta una vinculación con los elementos expuestos en: [[Vulture/research/limitations]]. **El documento posee un enfoque conceptual, sera más exacto cuando empiece el desarrollo formal del proyecto Vulture**


---
# Tabla de Contenidos 

**Tabla de contenidos**:
[Introducción](#Introducción)
[Activos](#Activos)







---

# Introducción 

Como el diseño de todo software, es importante analizar la seguridad del mismo frente a distintos tipos de amenazas y proponer soluciones internas o externas.

En este documento se explicarán los vectores de ataques más significativos, como el sistema Vulture responde ante los mismos y cómo se manejan las soluciones ya sean por diseño o por capas de externas.

---
# Activos 

Se considera a los activos (assets) dentro del sistema a:
- Telemetría cruda: Extraída, etiquetada y firmada.
- Artefactos de evidencia: Hashes, firmas, ficheros, snapshots.
- Alertas y Reportes: anomaly_score, posterior_probability.
- Claves de firmado: protocolo HMAC-SHA256 
- Infraestructura: graph DB, TSDB, brokers
- Modelos: Motores Bayesianos, Agentes de modelos estadísticos, Árboles de Decisiones.
- 