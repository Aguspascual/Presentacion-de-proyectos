# Portafolio de Proyectos de Ingeniería y Desarrollo
**Agustín Pascual Marcos** | Estudiante de Ingeniería en Sistemas de Información (UTN FRLP) & Full-Stack Developer

Bienvenido a mi repositorio principal de proyectos. Aquí documento la arquitectura, el modelado de datos y el flujo lógico de las soluciones tecnológicas que he diseñado y desarrollado de extremo a extremo. Mi enfoque se centra en crear sistemas escalables, seguros y orientados a resolver problemas reales de negocio, tanto en entornos B2C como corporativos (B2B).

## 🛠️ Stack Tecnológico Principal
<p align="left">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" />
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" />
  <img src="https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi" />
  <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white" />
</p>

---

## Índice de Proyectos Destacados

Haz clic en el nombre de cada proyecto para acceder a su documentación técnica detallada (Arquitectura, Diagramas de Flujo y Modelos Entidad-Relación).

### 1. [Velo](./Velo) 
**Ecosistema de Delivery en Tiempo Real (B2C)**
Plataforma transaccional de alta concurrencia que conecta clientes, comercios y repartidores. Destaca por su motor de asignación en tiempo real, manejo de estados concurrentes y uso de WebSockets para tracking en vivo.
* **Stack:** FastAPI, React Native, PostgreSQL, WebSockets.
* **Hito:** Aplicaciones desplegadas en producción (App Store / Play Store).

### 2. [AgroManager](./AgroManager) 
**Plataforma de Gestión Agrícola con Copiloto de IA (B2B)**
Sistema avanzado para la digitalización de la operatividad en el campo. Incluye un pipeline asíncrono impulsado por IA para la extracción y validación de datos (remitos, stock, maquinaria) sin bloquear la experiencia del operario.
* **Stack:** FastAPI, React Native, PostgreSQL, Redis, Procesamiento Asíncrono (Workers).
* **Hito:** Resolución de problemas de UX en zonas de baja conectividad y modelado de datos industriales complejos.

### 3. [EcoPolo](./EcoPolo) 
**Sistema Corporativo de Mantenimiento y Auditoría**
Plataforma corporativa enfocada en la trazabilidad y el cumplimiento de protocolos de seguridad en plantas industriales. Basada fuertemente en Control de Acceso por Roles (RBAC), jerarquías de aprobación y logs inmutables de auditoría.
* **Stack:** React.js, Flask (Python), PostgreSQL.
* **Hito:** Diseño arquitectónico enfocado en la seguridad transaccional, observabilidad y cumplimiento de protocolos industriales.

### 4. [Buscapolo](./Buscapolo) 
**App de Gestión para Servicios Eléctricos**
Aplicación móvil especializada diseñada para facilitar la administración de trabajos, presupuestos y clientes para profesionales del rubro eléctrico, optimizando el seguimiento de servicios en terreno.
* **Stack:** React Native (Expo).

---

## Filosofía de Desarrollo
* **Arquitectura antes que código:** Todo proyecto comienza con un modelado de datos robusto (ERD) y flujos críticos validados.
* **Seguridad y Consistencia:** Implementación de locks transaccionales, RBAC y auditorías para garantizar la integridad de los datos.
* **Experiencia de Usuario (UX):** Uso de asincronía y arquitecturas desacopladas para mantener interfaces fluidas, incluso en procesos pesados de backend.
