## 🚜 AgroManager

AgroManager es un sistema avanzado de gestión para maquinaria agrícola y operaciones de campo, diseñado para digitalizar la operatividad del agro con una visión integral sobre los procesos críticos del negocio. Su propósito es centralizar el control de stock, la planificación y trazabilidad de labores, el mantenimiento de maquinaria y el seguimiento operativo en tiempo real, transformando flujos manuales y dispersos en una plataforma robusta, auditable y escalable.

Desde una perspectiva de Ingeniería de Sistemas, el principal desafío técnico de AgroManager no es solo modelar la complejidad operativa del entorno agrícola, sino también traducirla en una arquitectura confiable y usable para usuarios de campo. En ese marco, el sistema incorpora un **Copiloto de IA** orientado a la carga asíncrona de datos, la asistencia contextual en procesos operativos y la interpretación de información proveniente de telemetría y sensores, elevando la capacidad de automatización y soporte a la toma de decisiones.

La solución está construida sobre un stack moderno y orientado a rendimiento: **FastAPI** para la capa de servicios, **React Native** para la experiencia móvil multiplataforma y **PostgreSQL** como base de datos transaccional. Este desarrollo colaborativo refleja capacidad para resolver problemas complejos de arquitectura, integración y experiencia de usuario en entornos industriales y agrícolas, donde la confiabilidad operativa, la simplicidad de uso y la escalabilidad técnica son requisitos centrales.

---

#### 🏗️ Arquitectura General del Sistema

```mermaid
flowchart LR
    subgraph CLIENTE["Capa de Cliente"]
        MOBILE["App móvil<br/>React Native<br/>Operarios"]
        WEB["Panel web<br/>Administración"]
    end

    subgraph BACKEND["Capa de Backend"]
        GATEWAY["FastAPI API<br/>Router / Gateway"]
    end

    subgraph PROC["Capa de Procesamiento"]
        WORKER["Worker Asíncrono<br/>Copiloto de IA<br/>NLP / Imágenes"]
    end

    subgraph DATA["Capa de Datos"]
        PG[("PostgreSQL<br/>Persistencia principal")]
        REDIS[("Redis<br/>Colas / Caché")]
    end

    MOBILE -->|"HTTPS/JSON"| GATEWAY
    WEB -->|"HTTPS/JSON"| GATEWAY
    MOBILE -.->|"WSS (Telemetría)"| GATEWAY

    GATEWAY -->|"Tareas async"| WORKER
    WORKER -->|"Resultados"| GATEWAY

    GATEWAY -->|"CRUD"| PG
    GATEWAY -->|"Queue"| REDIS

    WORKER -->|"Read/Write"| PG
    WORKER -->|"Broker"| REDIS

    classDef client fill:#eef6ff,stroke:#5b8def,stroke-width:1.5px,color:#1f2937;
    classDef backend fill:#ecfdf5,stroke:#2f855a,stroke-width:1.5px,color:#1f2937;
    classDef worker fill:#fff7ed,stroke:#dd6b20,stroke-width:1.5px,color:#1f2937;
    classDef data fill:#f5f3ff,stroke:#7c3aed,stroke-width:1.5px,color:#1f2937;

    class MOBILE,WEB client;
    class GATEWAY backend;
    class WORKER worker;
    class PG,REDIS data;

    style CLIENTE fill:#f8fbff,stroke:#c7d7f7,stroke-width:1px
    style BACKEND fill:#f5fff8,stroke:#b7e4c7,stroke-width:1px
    style PROC fill:#fffaf3,stroke:#f3d19c,stroke-width:1px
    style DATA fill:#faf7ff,stroke:#d6ccfa,stroke-width:1px
```

#### ⚙️ Flujo de Procesamiento Asíncrono

```mermaid
flowchart LR
    subgraph MAIN["Hilo principal (Frontend/API)"]
        direction TB
        U["Usuario<br/>envía voz, texto o foto"]
        API["Backend FastAPI<br/>recibe request"]
        QUEUE["Task Queue<br/>encola trabajo"]
        ACK["Respuesta inmediata<br/>ACK"]
        NOTIFY["Notificación<br/>WebSocket / Push"]
        CONFIRM["Confirmación de carga"]
    end

    subgraph BG["Background processing (Worker)"]
        direction TB
        WORKER["Worker de IA<br/>procesa input"]
        OCR["Extracción de entidades"]
        D1{"¿Maquinaria?"}
        D2{"¿Stock OK?"}
        D3{"¿Datos OK?"}
        READY["Payload listo"]
        REVIEW["Revisión manual"]
    end

    U --> API --> QUEUE --> ACK
    QUEUE --> WORKER --> OCR --> D1
    D1 -- Sí --> D2
    D1 -- No --> REVIEW
    D2 -- Sí --> D3
    D2 -- No --> REVIEW
    D3 -- Sí --> READY
    D3 -- No --> REVIEW

    READY --> NOTIFY
    REVIEW --> NOTIFY
    NOTIFY --> CONFIRM
```

#### 📊 Modelo de Datos Relacional

```mermaid
erDiagram
    USUARIOS ||--o{ OPERACIONES : realiza
    LOTES_CAMPOS ||--o{ OPERACIONES : contiene
    MAQUINARIA ||--o{ OPERACIONES : ejecuta
    INSUMOS_STOCK ||--o{ OPERACIONES : consume
    MAQUINARIA ||--o{ REGISTROS_SENSORES : monitorea

    USUARIOS {
        UUID id PK
        VARCHAR email UK
        VARCHAR nombre_completo
        VARCHAR rol_rbac
        BOOLEAN activo
        JSONB permisos_override
        TIMESTAMPTZ created_at
    }

    MAQUINARIA {
        UUID id PK
        VARCHAR codigo_interno UK
        VARCHAR tipo
        VARCHAR marca
        VARCHAR modelo
        DECIMAL horometro_actual
        JSONB configuracion_tecnica
        TIMESTAMPTZ updated_at
    }

    LOTES_CAMPOS {
        UUID id PK
        VARCHAR nombre
        DECIMAL superficie_ha
        JSONB poligono_geojson
        TIMESTAMPTZ created_at
    }

    OPERACIONES {
        UUID id PK
        UUID usuario_id FK
        UUID maquinaria_id FK
        UUID lote_campo_id FK
        UUID insumo_id FK
        VARCHAR tipo_operacion
        DECIMAL area_trabajada_ha
        VARCHAR estado
        TIMESTAMPTZ fecha_inicio
    }

    INSUMOS_STOCK {
        UUID id PK
        VARCHAR sku UK
        VARCHAR nombre
        VARCHAR unidad_medida
        DECIMAL stock_actual
        TIMESTAMPTZ created_at
    }

    REGISTROS_SENSORES {
        UUID id PK
        UUID maquinaria_id FK
        VARCHAR tipo_sensor
        DECIMAL valor
        JSONB ubicacion_gps
        TIMESTAMPTZ capturado_en
    }
```

#### 📸 Capturas del sistema

##### Dashboard

![Dashboard](./images/Dashboard.png)

##### Gestión de clientes

![Clientes](./images/Clientes.png)

##### Flota y logística

![Flota](./images/Flota.png)

##### Mantenimiento de flota

![Mantenimiento de flota](./images/MantenimientoFlota.png)
