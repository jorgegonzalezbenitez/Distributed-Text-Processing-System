# Sistema Distribuido de Procesamiento de Texto
### Despliegue en Clúster

---

## Resumen

Sistema de arquitectura distribuida para el procesamiento de texto a gran escala usando Java, Hazelcast y componentes multihilo. Diseñado para descargar, indexar y consultar grandes colecciones de libros almacenados en un datalake.

La arquitectura incluye un Crawler para la descarga de libros, un Indexer para construir índices invertidos y una Query API REST para búsqueda y recuperación de metadatos. El sistema fue probado con más de 2.150 libros, demostrando escalabilidad, tolerancia a fallos y rendimiento eficiente en consultas mediante almacenamiento y sincronización distribuidos.

---

## Arquitectura del sistema

### 1. Crawler
- Descargador de libros multihilo con `ExecutorService`
- Almacena los libros en un datalake estructurado
- Gestiona reintentos y descargas fallidas

### 2. Indexer
- Procesa los libros descargados y construye un índice invertido
- Extrae metadatos (título, autor, idioma) mediante expresiones regulares
- Almacenamiento dual: JSON Datamart (basado en archivos) y MongoDB Datamart (distribuido y escalable)

### 3. Query API
- API REST construida con SparkJava
- Soporta búsquedas por palabra simple y múltiples palabras
- Usa Hazelcast como caché distribuida para respuestas de baja latencia

---

## Resultados

### Crawler

| Métrica | Valor |
|---------|-------|
| Libros descargados | 61 |
| Velocidad media | 50 libros/min |
| Velocidad pico | 75 libros/min |
| Velocidad pico (burst) | 12,2 libros/seg |

### Indexer

| Ejecución | Almacenamiento | Duración | Palabras procesadas |
|-----------|---------------|----------|---------------------|
| 1ª | JSON | Instantáneo | 86.696 |
| 1ª | MongoDB | 1:12 min | 86.696 |
| 2ª | JSON | 1:05 min | 86.696 |
| 2ª | MongoDB | 1:08 min | 86.696 |

### Query API

| Almacenamiento | Tiempo medio (ms) | CPU (%) | Memoria (MB) |
|---------------|-------------------|---------|--------------|
| JSON | 24,3 | 15,2 | 45,3 |
| MongoDB | 31,7 | 17,8 | 55,7 |

---

## Conclusiones

- JSON es más rápido en indexación pero menos escalable
- MongoDB ofrece mayor escalabilidad y mejores capacidades de consulta
- Java supera significativamente a Python tanto en indexación como en consulta
- Hazelcast garantiza tolerancia a fallos y consultas distribuidas eficientes

---

## Tecnologías

Java · Hazelcast · MongoDB · SparkJava · API REST · Multithreading · Docker
