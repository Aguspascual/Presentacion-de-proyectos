## EcoPolo
EcoPolo es una plataforma corporativa orientada a la gestión de mantenimiento de plantas industriales y a la administración de personal, diseñada para digitalizar procesos internos con foco en seguridad, trazabilidad y control operativo. Su objetivo es centralizar la ejecución y supervisión de tareas críticas del negocio, reduciendo dependencia de circuitos manuales y elevando la consistencia en la operación diaria.

Desde una perspectiva de Ingeniería de Sistemas, el proyecto prioriza una arquitectura B2B robusta, auditable y alineada con entornos donde el cumplimiento de protocolos no es opcional. La solución integra control de acceso basado en roles (RBAC), jerarquías organizacionales y módulos de auditoría que validan acciones sensibles, permitiendo que cada operación quede gobernada por reglas de negocio claras y mecanismos de trazabilidad verificables.

A nivel técnico, EcoPolo se apoya en una arquitectura web desacoplada con React.js en el frontend, Flask/Python como backend API y PostgreSQL como base de datos transaccional. Esta base tecnológica permite construir software empresarial seguro, escalable y preparado para evolucionar sobre procesos internos complejos, donde la observabilidad y la evidencia de cumplimiento son parte central del producto.

### Módulos principales
- `Dashboard operativo`: visión consolidada del estado general, auditorías, maquinaria, logística, reportes y personal activo.
- `Gestión de insumos`: seguimiento de stock actual, unidades, códigos internos y alertas de reposición.
- `Panel de mantenimiento`: calendario de tareas, estados por criticidad y monitoreo de próximos vencimientos.
- `Gestión de reportes`: registro de incidentes o novedades operativas con criticidad y validación de operabilidad.

### Arquitectura de Alto Nivel
```mermaid
flowchart LR
    U["Usuario Corporativo"] --> C

    subgraph Cliente["Cliente"]
        C["React.js Frontend"]
        UI["Módulos de UI<br/>Mantenimiento / Personal / Auditoría"]
        G["Guardas de Navegación<br/>RBAC + Jerarquías"]

        C --> UI
        C --> G
    end

    subgraph Backend["Backend"]
        R["Flask Router / API"]
        A["Auth & RBAC"]
        S["Servicios de Negocio"]
        AU["Módulo de Auditoría<br/>Validación de Protocolo"]
        V["Validador de Acciones Sensibles"]

        R --> A
        A --> S
        S --> AU
        AU --> V
    end

    subgraph Datos["Datos"]
        DB["PostgreSQL"]
        T1["Tablas Operativas"]
        T2["Usuarios, Roles y Jerarquías"]
        T3["Logs y Trazas de Auditoría"]

        DB --> T1
        DB --> T2
        DB --> T3
    end

    C -->|"HTTPS / JSON API"| R
    G -->|"Permisos de acceso"| A
    S -->|"Lectura / Escritura"| DB
    V -->|"Validación obligatoria antes de persistir"| DB
    AU -->|"Registro de evidencia y trazabilidad"| T3
```


### Diagrama de Flujo
```mermaid
flowchart TD
    A["Operario reporta falla o necesidad de mantenimiento<br/>Frontend React.js"] --> B["Backend Flask recibe la solicitud"]
    B --> C{"¿Usuario autorizado<br/>según RBAC?"}

    C -- "No" --> D["Rechazar operación<br/>Registrar intento no autorizado"]
    C -- "Sí" --> E["Crear Orden de Trabajo"]
    E --> F["Disparar alertas automáticas<br/>a supervisores"]
    F --> G["Personal de mantenimiento<br/>toma la tarea"]
    G --> H["Ejecutar mantenimiento"]
    H --> I["Registrar finalización<br/>de la tarea"]
    I --> J["Módulo de Auditoría intercepta<br/>el cierre de la tarea"]
    J --> K["Registrar trazabilidad:<br/>quién, cuándo, qué"]
    K --> L{"¿Supervisor aprueba<br/>el cierre?"}

    L -- "No" --> M["Reabrir Orden de Trabajo<br/>Solicitar correcciones o revisión"]
    M --> G
    L -- "Sí" --> N["Archivar orden y evidencia<br/>en PostgreSQL"]
    N --> O["Proceso cerrado con cumplimiento<br/>y trazabilidad completa"]

    classDef process fill:#EAF3FF,stroke:#1F4E79,stroke-width:1.5px,color:#0F172A;
    classDef decision fill:#FFF4D6,stroke:#B7791F,stroke-width:1.5px,color:#0F172A;
    classDef terminal fill:#E8F5E9,stroke:#2F855A,stroke-width:1.5px,color:#0F172A;
    classDef reject fill:#FDECEC,stroke:#C53030,stroke-width:1.5px,color:#0F172A;
    classDef audit fill:#F3E8FF,stroke:#6B46C1,stroke-width:1.5px,color:#0F172A;

    class A,O,N terminal;
    class B,E,F,G,H,I,M process;
    class C,L decision;
    class D reject;
    class J,K audit;
```


### ERD Simplificado (Seguridad y Trazabilidad)
```mermaid
erDiagram
    USUARIOS {
        UUID id_usuario "PK"
        VARCHAR legajo "UK"
        VARCHAR nombre_completo
        VARCHAR email "UK"
        VARCHAR area
        VARCHAR cargo
        UUID id_rol_permiso "FK"
        BOOLEAN activo
        TIMESTAMPTZ creado_en
        TIMESTAMPTZ actualizado_en
    }

    ROLES_PERMISOS {
        UUID id_rol_permiso "PK"
        VARCHAR nombre_rol "UK"
        VARCHAR nivel_jerarquico
        JSONB permisos
        BOOLEAN requiere_aprobacion_supervisor
        BOOLEAN activo
        TIMESTAMPTZ creado_en
        TIMESTAMPTZ actualizado_en
    }

    EQUIPOS_ACTIVOS {
        UUID id_equipo "PK"
        VARCHAR codigo_interno "UK"
        VARCHAR nombre_equipo
        VARCHAR tipo_equipo
        VARCHAR ubicacion_planta
        VARCHAR estado_operativo
        TIMESTAMPTZ ultima_inspeccion_en
        BOOLEAN activo
        TIMESTAMPTZ creado_en
        TIMESTAMPTZ actualizado_en
    }

    ORDENES_MANTENIMIENTO {
        UUID id_orden "PK"
        VARCHAR nro_orden "UK"
        UUID id_equipo "FK"
        UUID id_usuario_reportante "FK"
        VARCHAR tipo_mantenimiento
        VARCHAR criticidad
        VARCHAR estado_orden
        TEXT descripcion_falla
        TIMESTAMPTZ fecha_reporte
        TIMESTAMPTZ fecha_cierre
        TIMESTAMPTZ creado_en
        TIMESTAMPTZ actualizado_en
    }

    TAREAS_ASIGNADAS {
        UUID id_tarea "PK"
        UUID id_orden "FK"
        UUID id_usuario_asignado "FK"
        UUID id_usuario_supervisor "FK"
        VARCHAR nombre_tarea
        TEXT procedimiento
        VARCHAR estado_tarea
        TIMESTAMPTZ fecha_asignacion
        TIMESTAMPTZ fecha_inicio
        TIMESTAMPTZ fecha_finalizacion
        BOOLEAN requiere_aprobacion_cierre
        TIMESTAMPTZ creado_en
        TIMESTAMPTZ actualizado_en
    }

    LOGS_AUDITORIA {
        UUID id_log "PK"
        UUID id_usuario_actor "FK"
        UUID id_orden "FK"
        UUID id_tarea "FK"
        VARCHAR modulo
        VARCHAR accion
        VARCHAR resultado
        JSONB detalle_evento
        JSONB contexto_seguridad
        TIMESTAMPTZ fecha_evento
        VARCHAR hash_integridad "UK"
    }

    ROLES_PERMISOS ||--o{ USUARIOS : "define acceso RBAC"
    USUARIOS ||--o{ ORDENES_MANTENIMIENTO : "reporta"
    EQUIPOS_ACTIVOS ||--o{ ORDENES_MANTENIMIENTO : "origina"
    ORDENES_MANTENIMIENTO ||--o{ TAREAS_ASIGNADAS : "descompone"
    USUARIOS ||--o{ TAREAS_ASIGNADAS : "ejecuta"
    USUARIOS ||--o{ TAREAS_ASIGNADAS : "supervisa"
    USUARIOS ||--o{ LOGS_AUDITORIA : "genera acciones"
    ORDENES_MANTENIMIENTO ||--o{ LOGS_AUDITORIA : "traza operacion"
    TAREAS_ASIGNADAS ||--o{ LOGS_AUDITORIA : "traza ejecucion"
    ROLES_PERMISOS ||--o{ LOGS_AUDITORIA : "contextualiza control"
```

### Capturas de la interfaz

<table>
  <tr>
    <td align="center" width="50%">
      <img src="./images/dashboard-general.png" alt="Dashboard general de EcoPolo" />
      <br />
      <strong>Dashboard general</strong>
    </td>
    <td align="center" width="50%">
      <img src="./images/gestion-insumos.png" alt="Pantalla de gestión de insumos" />
      <br />
      <strong>Gestión de insumos</strong>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="./images/panel-mantenimiento.png" alt="Panel de mantenimiento con calendario" />
      <br />
      <strong>Panel de mantenimiento</strong>
    </td>
    <td align="center" width="50%">
      <img src="./images/gestion-reportes.png" alt="Pantalla de gestión de reportes" />
      <br />
      <strong>Gestión de reportes</strong>
    </td>
  </tr>
</table>
