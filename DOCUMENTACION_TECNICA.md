# Documentación Técnica — Ecosistema de Garantías Boxparts SAC

**Ecosistema digital para la trazabilidad de garantías tecnológicas**  
Boxparts SAC · SENATI 2026

---

## 1. Introducción

### 1.1 Propósito del sistema

El ecosistema web centraliza y automatiza la gestión del servicio postventa de Boxparts SAC. Reemplaza procesos manuales basados en boletas físicas y cuadernos por una plataforma integrada que vincula tickets de garantía, inventario, evidencia fotográfica y reportes gerenciales.

### 1.2 Alcance

El sistema cubre el ciclo completo desde la recepción de un equipo defectuoso hasta su resolución mediante diagnóstico técnico, aprobación o rechazo, y eventual reemplazo de hardware (swap), con trazabilidad inmutable de cada intervención.

---

## 2. Contexto empresarial

### 2.1 Datos de la empresa

| Dato | Valor |
|------|-------|
| Razón social | BOX PARTS SOCIEDAD ANÓNIMA CERRADA |
| RUC | 20605675523 |
| Dirección | Av. Inca Garcilaso de la Vega 1348 Int. 1137 |
| Teléfono | (+51) 924 448 300 |
| Representante legal | Marco Antonio Cuba Cuzquisiban |

### 2.2 Catálogo de productos

| Línea | Productos |
|-------|-----------|
| Componentes de PC | Procesadores, placas madre, RAM, tarjetas gráficas, fuentes de poder, SSD/HDD |
| Equipos | PCs armadas (diseño/gaming), laptops, monitores |
| Periféricos y Accesorios | Teclados mecánicos, mouses, audífonos, sillas gamer, refrigeración |

### 2.3 Problemática identificada

| Área afectada | Efecto |
|---------------|--------|
| **Almacén** | Descuadres de inventario en swaps sin actualización inmediata |
| **Soporte técnico** | Disputas por falta de evidencia fotográfica digitalizada |
| **Clientes** | Demoras por cuellos de botella en flujos manuales |
| **Personal** | Reprocesos, estrés y errores al transcribir números de serie |

### 2.4 Justificación

La digitalización elimina la informalidad operativa, estandariza la comunicación entre áreas, sincroniza el stock en tiempo real y habilita la toma de decisiones basada en datos históricos y métricas de SLA.

---

## 3. Objetivos

| Tipo | Descripción |
|------|-------------|
| **General** | Optimizar la gestión de garantías, asegurar integridad de datos y sincronizar inventarios en postventa |
| **OE1** | Digitalizar tickets de garantía en tiempo real |
| **OE2** | Integrar garantías con inventario para cambios automáticos por equivalencia |
| **OE3** | Auditoría y seguimiento de productividad técnica |
| **OE4** | Reportes automatizados para gerencia |
| **OE5** | Esquema de roles y permisos de usuario |

---

## 4. Arquitectura del sistema

### 4.1 Visión general

```
┌──────────────────────────────────────────────────────────────┐
│                     Cliente (Navegador)                      │
│                   React SPA — Interfaz visual                  │
└────────────────────────────┬─────────────────────────────────┘
                             │ HTTPS / REST API
                             │ Authorization: Bearer JWT
┌────────────────────────────▼─────────────────────────────────┐
│                    NestJS — Backend API                      │
│   Módulos: Auth · Tickets · Series · Inventario · Swap       │
│            Clientes · Usuarios · Dashboard · Storage           │
└────────────────────────────┬─────────────────────────────────┘
                             │ TypeORM
┌────────────────────────────▼─────────────────────────────────┐
│              Supabase (PostgreSQL + Storage)                 │
│   Datos transaccionales          Evidencias fotográficas     │
└──────────────────────────────────────────────────────────────┘
```

### 4.2 Fundamentos de diseño

| Principio | Aplicación en el ecosistema |
|-----------|------------------------|
| **Arquitectura modular** | Módulos independientes de garantías, inventario y reportes |
| **API RESTful** | Comunicación estandarizada cliente-servidor vía HTTP |
| **Inyección de dependencias** | NestJS con DI para bajo acoplamiento |
| **ORM (TypeORM)** | Mapeo objeto-relacional sobre PostgreSQL |
| **JWT** | Autenticación stateless sin sesiones en servidor |
| **Persistencia políglota** | PostgreSQL para datos; Storage para archivos binarios |
| **Principios SOLID** | Código mantenible y extensible |

### 4.3 Actores del sistema

| Actor | Área | Responsabilidades |
|-------|------|-------------------|
| **Vendedor (Recepción)** | Primera línea | Validar vigencia del S/N, registrar datos del cliente, subir evidencia fotográfica y generar ticket de soporte |
| **Técnico (Soporte)** | Laboratorio | Recibir ticket digital, actualizar estado de revisión, registrar fallas y ejecutar Swap afectando stock |
| **Administrador (Jefatura)** | Gerencia | Monitorear flujo de garantías, auditar descuadres de inventario, dashboard SLA y gestión de accesos |

---

## 5. Stack tecnológico

### 5.1 Capa de presentación

| Tecnología | Función |
|------------|---------|
| **React** | Interfaz de usuario basada en componentes reutilizables |
| **Next.js** | Framework de aplicación web del frontend |
| **TypeScript** | Tipado estático en todo el ecosistema |
| **Tailwind CSS** | Sistema de diseño responsivo |

### 5.2 Capa de aplicación

| Tecnología | Función |
|------------|---------|
| **Node.js** | Runtime del servidor |
| **NestJS** | Framework backend modular con DI nativa |
| **Passport + JWT** | Autenticación y autorización |

### 5.3 Capa de datos

| Tecnología | Función |
|------------|---------|
| **Supabase** | Infraestructura BaaS en la nube |
| **PostgreSQL** | Base de datos relacional transaccional |
| **Supabase Storage** | Almacenamiento de evidencias fotográficas (URLs en BD) |
| **TypeORM** | ORM para consultas tipadas y prevención de inyección SQL |

### 5.4 Herramientas de desarrollo

| Herramienta | Uso |
|-------------|-----|
| **Figma** | Prototipado UI/UX de alta fidelidad |
| **VS Code** | IDE principal |
| **Git / GitHub** | Control de versiones y trabajo colaborativo |
| **Postman** | Pruebas de endpoints REST |

### 5.5 Metodología

**Scrum** — Ciclo de 18 semanas:

| Fase | Semanas | Entregables |
|------|---------|-------------|
| Inicialización y backlog | 1–3 | Requerimientos, prototipos Figma, historias de usuario |
| Sprint 1–2 | 4–7 | PostgreSQL, NestJS, JWT |
| Sprint 3–4 | 8–11 | React, API REST, tickets, validación S/N, evidencias |
| Sprint 5–6 | 12–15 | Swap + inventario, dashboard SLA |
| QA y estabilización | 16 | Pruebas funcionales, estrés y seguridad |
| Documentación y capacitación | 17–18 | Manuales, guías de usuario, talleres |

---

## 6. Requerimientos funcionales

| ID | Requerimiento | Descripción |
|----|---------------|-------------|
| **RF-01** | Autenticación y autorización | Login seguro con restricción por rol (Administrador, Técnico, Vendedor) |
| **RF-02** | Generación de tickets | Registro vinculando cliente, hardware y falla reportada |
| **RF-03** | Validación de S/N | Consulta automática de autenticidad y vigencia de garantía |
| **RF-04** | Evidencia fotográfica | Carga de imágenes del estado físico del equipo (máx. 5 MB) |
| **RF-05** | Actualización de estados | Cambio de estado con registro de fecha, hora e informe técnico |
| **RF-06** | Stock en tiempo real | Disponibilidad actualizada del almacén antes de autorizar cambios |
| **RF-07** | Swap | Reemplazo de hardware con descuento automático de inventario |
| **RF-08** | Dashboard gerencial | Panel con tickets abiertos, resueltos y alertas de stock crítico |
| **RF-09** | SLA | Cálculo del tiempo de resolución desde apertura hasta cierre |
| **RF-10** | Trazabilidad | Búsqueda de historial por DNI del cliente o número de serie |

### 6.1 Casos de uso principales

| CU | Nombre | Actor | Reglas clave |
|----|--------|-------|--------------|
| CU-01 | Autenticación y autorización | Todos | Contraseñas con hash Bcrypt; bloqueo tras 3 intentos fallidos |
| CU-02 | Validación automática de S/N | Vendedor, Técnico | Cálculo de garantía excluye domingos y feriados |
| CU-03 | Generación de ticket con evidencia | Vendedor | Evidencia obligatoria; máx. 5 MB; código ej. `TK-1045` |
| CU-04 | Actualización de estado y trazabilidad | Técnico | Informe técnico obligatorio; ticket cerrado no retrocede |
| CU-05 | Ejecución de swap en inventario | Técnico | Requiere estado aprobado; stock no puede quedar negativo |
| CU-06 | Dashboard gerencial | Administrador | Métricas SLA y KPIs operativos |

### 6.2 Reglas de negocio — Ticket (CU-03)

- Es obligatorio ejecutar la validación de S/N antes de crear el ticket (`<<include>>`).
- Se requiere al menos una fotografía de evidencia.
- Archivos mayores a 5 MB son rechazados (`<<extend>>`).
- Las imágenes se almacenan en Supabase Storage; solo la URL se guarda en la base de datos.
- Se genera un código único de ticket (ej. `TK-1045`).

---

## 7. Requerimientos no funcionales

| ID | Requerimiento | Criterio |
|----|---------------|----------|
| **RNF-01** | Seguridad JWT | Toda comunicación cliente-servidor protegida con tokens |
| **RNF-02** | Tiempos de respuesta | Transacciones críticas (validación S/N, stock) ≤ 2 segundos |
| **RNF-03** | Disponibilidad | Uptime 99.9% en horario laboral (Supabase) |
| **RNF-04** | Interfaz responsiva | Adaptación fluida a monitores de la empresa y dispositivos móviles |
| **RNF-05** | Almacenamiento externo | Evidencias en Storage, no en tablas transaccionales |
| **RNF-06** | Arquitectura escalable | Backend modular para integración futura de nuevos procesos |
| **RNF-07** | Mantenibilidad | Tipado estricto TypeScript en todo el código fuente |
| **RNF-08** | Backups | Copias de seguridad automatizadas diarias en la nube |
| **RNF-09** | Compatibilidad | Chrome, Firefox y Edge en versiones recientes |

---

## 8. Modelo de datos

### 8.1 Entidades principales

```
Usuario ──────────────┐
                      ├── TicketGarantia ─── LogTrazabilidad
Cliente ── SerieVendida ─┘         │
                      │            └── EstadoTicket
ModeloHardware ───────┘
```

| Entidad | Descripción | Campos clave |
|---------|-------------|--------------|
| **Usuario** | Personal del sistema | nombre, email, rol, estado |
| **Cliente** | Comprador del hardware | docCliente (DNI/RUC), nombre, contacto |
| **ModeloHardware** | Catálogo de productos | marca, modelo, categoría, stock, precio |
| **SerieVendida** | Unidad individual vendida | numeroSerie, fechaVenta, mesesGarantia, estadoSerie |
| **TicketGarantia** | Caso de garantía | codigoTicket, fallaReportada, evidenciaUrl |
| **EstadoTicket** | Estado del flujo | nombreEstado, ordenFlujo |
| **LogTrazabilidad** | Auditoría de cambios | estadoAnterior, estadoNuevo, informeTecnico |

### 8.2 Estados de serie

| Estado | Significado |
|--------|-------------|
| `ACTIVA` | Unidad operativa en poder del cliente |
| `EN_GARANTIA` | Tiene un ticket de garantía abierto |
| `DESCARTADA` | Unidad reemplazada o dada de baja tras swap |

### 8.3 Estados de ticket

| Estado | Orden | Descripción |
|--------|-------|-------------|
| `RECIBIDO` | 1 | Ticket abierto por el vendedor |
| `EN_DIAGNOSTICO` | 2 | En revisión del laboratorio técnico |
| `APROBADO` | 3 | Garantía válida — procede swap |
| `RECHAZADO` | 4 | No cubierto por garantía |
| `CERRADO` | 5 | Caso resuelto |

---

## 9. Máquina de estados

### 9.1 Transiciones permitidas

| Estado actual | Transiciones | Responsable |
|---------------|--------------|-------------|
| RECIBIDO | → EN_DIAGNOSTICO | Técnico |
| EN_DIAGNOSTICO | → APROBADO, RECHAZADO | Técnico |
| APROBADO | → CERRADO (solo vía Swap) | Técnico |
| RECHAZADO | → CERRADO | Técnico |
| CERRADO | — (terminal) | — |

### 9.2 Reglas adicionales

- Todo cambio de estado exige **informe técnico** obligatorio.
- Tickets **APROBADO** no se cierran manualmente; requieren proceso de **Swap**.
- El swap decrementa stock, marca la serie antigua como `DESCARTADA`, crea nueva serie y cierra el ticket automáticamente.
- Si el modelo solicitado no tiene stock, el sistema sugiere **equivalentes** (misma categoría, ±10% de precio).

---

## 10. Control de acceso (RBAC)

| Ruta / Módulo | Administrador | Técnico | Vendedor |
|---------------|:---:|:---:|:---:|
| Dashboard | ✓ | — | — |
| Tickets (listado/detalle) | ✓ | ✓ | ✓ |
| Nuevo ticket | — | — | ✓ |
| Inventario | ✓ | ✓ | — |
| Clientes | ✓ | — | ✓ |
| Usuarios | ✓ | — | — |
| Guía del sistema | ✓ | ✓ | ✓ |

---

## 11. API REST — Endpoints principales

Base: servidor NestJS. Autenticación: `Authorization: Bearer <JWT>`.

| Método | Endpoint | Rol | Función |
|--------|----------|-----|---------|
| POST | `/auth/login` | Público | Inicio de sesión |
| GET | `/tickets` | Todos | Listado paginado |
| GET | `/tickets/buscar` | Todos | Búsqueda por DNI, serie o código |
| POST | `/tickets` | Vendedor | Crear ticket |
| PATCH | `/tickets/:id/estado` | Técnico | Cambiar estado |
| GET | `/series/validar/:sn` | Todos | Validar garantía |
| GET | `/inventario` | Técnico, Admin | Listar stock |
| POST | `/swap` | Técnico | Ejecutar cambio de hardware |
| GET | `/dashboard/resumen` | Admin | KPIs gerenciales |
| POST | `/storage/upload` | Todos | Subir evidencia fotográfica |

---

## 12. Módulos de la interfaz

| Pantalla | Funcionalidad |
|----------|---------------|
| **Login** | Autenticación con redirección según rol |
| **Dashboard** | KPIs, gráficos de tickets/semana, distribución por estado, SLA por técnico, stock crítico |
| **Tickets** | Listado con búsqueda, filtro por estado, alertas SLA (>5 días) |
| **Nuevo ticket** | Wizard: validar S/N → datos + evidencia → confirmación |
| **Detalle ticket** | Información del caso, gestión técnica, panel swap, timeline de trazabilidad |
| **Inventario** | Catálogo con filtros, alertas de stock crítico (< 5 uds) |
| **Clientes** | Registro y consulta |
| **Usuarios** | CRUD de personal, activar/desactivar, desbloquear |
| **Guía** | Documentación operativa integrada |

---

## 13. Diseño responsivo

La interfaz React se adapta a distintos viewports:

- **Sidebar**: panel fijo en desktop; drawer deslizable en mobile
- **Tablas**: scroll horizontal táctil; columnas secundarias ocultas en pantallas pequeñas
- **Dashboard**: grid adaptable (1 → 2 → 4 columnas según breakpoint)
- **Gráficos**: contenedores responsivos con leyendas compactas
- **Formularios**: layouts en columna única en mobile

Breakpoints: `sm` (640px), `md` (768px), `lg` (1024px), `xl` (1280px).

---

## 14. Conceptos del dominio

| Término | Definición |
|---------|------------|
| **Ticket de soporte** | Registro digital único con datos del cliente, diagnóstico y estado de garantía |
| **S/N (Serial Number)** | Código alfanumérico único para validar autenticidad y vigencia |
| **Swap** | Reemplazo oficial de hardware defectuoso con actualización instantánea de inventario |
| **SLA** | Tiempo de resolución calculado desde apertura hasta cierre del ticket |
| **Trazabilidad** | Seguimiento digital e inmutable del historial de cada equipo |
| **Stock crítico** | Alerta cuando el inventario disponible es menor a 5 unidades |
| **Logística inversa** | Proceso de retorno del equipo gestionado desde la plataforma web |

---

## 15. Resultados e indicadores

| Indicador | Antes | Después |
|-----------|-------|---------|
| Tiempo de atención por ticket | ~120 min | ~30 min (−75%) |
| Fugas de inventario | Presentes | 0% |
| Centralización de datos | No | Sí |
| Reportes gerenciales | Manuales | Automatizados |
| Evidencia fotográfica | No digitalizada | Almacenada en la nube |

---

## 16. Referencias

- Estrada Guerrero, M. E. & Manrique Mateo, R. F. (2026). *Implementación de un ecosistema digital para la trazabilidad de garantías tecnológicas que optimice el soporte postventa de Boxparts SAC*. Proyecto de innovación — SENATI.
- Repositorio del sistema: [github.com/MardenEstrada/Boxparts_SAC-Demo](https://github.com/MardenEstrada/Boxparts_SAC-Demo)

---

## 17. Equipo

| Rol | Nombre |
|-----|--------|
| Autores | Marden Erick Estrada Guerrero, Roy Fernando Manrique Mateo |
| Asesor | Joe Alexander Castillo Liñán |
| Empresa | Boxparts SAC — Marco Antonio Cuba Cuzquisiban |
| Institución | SENATI — Desarrollo de Software
