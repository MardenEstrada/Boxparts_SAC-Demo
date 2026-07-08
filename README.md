# Ecosistema Digital de Trazabilidad de Garantías — Boxparts SAC

Sistema web para la gestión integral del servicio postventa y trazabilidad de garantías tecnológicas.

> Proyecto de innovación desarrollado en el marco del programa de **Desarrollo de Software — SENATI** (2026).  
> *Implementación de un ecosistema digital para la trazabilidad de garantías tecnológicas que optimice el soporte postventa de Boxparts SAC.*

---

## Sobre Boxparts SAC

| Dato | Detalle |
|------|---------|
| **Razón social** | BOX PARTS SOCIEDAD ANÓNIMA CERRADA |
| **Nombre comercial** | BOXPARTS |
| **RUC** | 20605675523 |
| **Dirección** | Av. Inca Garcilaso de la Vega 1348 Int. 1137 |
| **Teléfono** | (+51) 924 448 300 |
| **Representante legal** | Marco Antonio Cuba Cuzquisiban |
| **Rubro** | Comercialización de equipos tecnológicos de alta gama y soporte postventa |
| **Área de implementación** | Departamento de Garantías (Servicio Postventa) |

Boxparts SAC es una empresa peruana dedicada a la **comercialización de equipos de tecnología de vanguardia**, ofreciendo productos de alta gama en el rubro de hardware tecnológico.

### Misión

Liderar el mercado de artículos tecnológicos de cómputo en el Perú, otorgando el mayor grado de satisfacción a sus clientes a través de una asesoría especializada y cercana, ofreciendo la más amplia variedad de productos y los precios más competitivos del mercado.

### Visión

Consolidarse como la empresa líder y referente en el mercado tecnológico peruano, reconocida por ofrecer las últimas novedades globales en hardware, precios competitivos y una experiencia de compra segura y confiable.

### Valores

- **Confianza** — Relación a largo plazo con el cliente
- **Seguridad** — Compra protegida y transacciones confiables
- **Calidad y Respaldo** — Hardware de alta gama con garantías sólidas
- **Innovación y Vanguardia** — Adopción de adelantos tecnológicos globales
- **Accesibilidad** — Precios competitivos para diversos perfiles

---

## Productos, mercado y clientes

### Productos (§1.3.1)

| Línea | Ejemplos |
|-------|----------|
| **Componentes de PC** | Procesadores, placas madre, memorias RAM, tarjetas gráficas, fuentes de poder, almacenamiento SSD/HDD |
| **Equipos** | Computadoras armadas para diseño y gaming, laptops, monitores |
| **Periféricos y Accesorios** | Teclados mecánicos, mouses, audífonos, sillas gamer, sistemas de refrigeración |

### Mercado (§1.3.2)

Sector de tecnología de la información (TI) y retail especializado. Ámbito principal: mercado nacional peruano mediante canal digital (e-commerce) y atención personalizada.

### Clientes (§1.3.3)

- **Entusiastas del Gaming** — Componentes de alto rendimiento para videojuegos
- **Profesionales Creativos** — Diseñadores, arquitectos y editores de video (workstations)
- **Sector Corporativo y estudiantil** — Renovación de equipos de oficina o estudio con garantía y soporte técnico

---

## Problema que resuelve

Antes de la digitalización, el área de garantías operaba de forma **manual y analógica**: boletas físicas, cuadernos de control y comunicación informal entre sucursales y el laboratorio técnico. Esto generaba:

- Cuellos de botella en la atención al cliente
- Pérdida de información y falta de auditoría
- Descuadres de inventario en cambios de producto (swap)
- Imposibilidad de verificar garantías y series de forma ágil
- Ausencia de reportes gerenciales y métricas de productividad (SLA)

---

## Objetivos del proyecto

### Objetivo general

Optimizar la gestión de garantías, asegurar la integridad de los datos y sincronizar el control de inventarios en el servicio postventa.

### Objetivos específicos

| ID | Objetivo |
|----|----------|
| **OE1** | Digitalizar el registro de garantías mediante tickets que centralicen la información en tiempo real |
| **OE2** | Integrar garantías con el control de inventario para gestionar automáticamente los cambios por equivalencia |
| **OE3** | Establecer auditoría y seguimiento de la productividad técnica y el historial de cada equipo |
| **OE4** | Optimizar la toma de decisiones gerenciales con reportes automatizados |
| **OE5** | Configurar roles y permisos para proteger la información del cliente y restringir modificaciones de inventario |

---

## Resultados obtenidos

- **Reducción del 75%** en el tiempo de atención por ticket (de 120 a 30 minutos)
- **Eliminación del 100%** de pérdidas por fugas de inventario
- Centralización de la información en una única plataforma web
- Control de swaps (cambios de hardware) en tiempo real
- Fortalecimiento de la coordinación del personal y la toma de decisiones gerenciales

---

## Actores del sistema

| Actor | Área | Responsabilidades |
|-------|------|-------------------|
| **Vendedor (Recepción)** | Primera línea | Validar S/N, registrar cliente, subir evidencia fotográfica y generar ticket de soporte |
| **Técnico (Soporte)** | Laboratorio | Actualizar estados, registrar fallas, ejecutar swap afectando stock de almacén |
| **Administrador (Jefatura)** | Gerencia | Monitorear flujo de garantías, auditar inventarios, dashboard SLA y gestión de accesos |

---

## Módulos del ecosistema

| Módulo | Descripción |
|--------|-------------|
| **Autenticación y seguridad** | Inicio de sesión con JWT y acceso restringido por rol |
| **Tickets de garantía** | Registro digital del ingreso de hardware defectuoso |
| **Validación de S/N** | Verificación automática de autenticidad y vigencia de garantía |
| **Evidencia fotográfica** | Carga de imágenes del estado físico del equipo (máx. 5 MB) |
| **Gestión técnica** | Actualización de estados con informe y trazabilidad |
| **Inventario** | Consulta de stock en tiempo real y alertas críticas (< 5 unidades) |
| **Swap** | Reemplazo de hardware con descuento automático de inventario |
| **Dashboard gerencial** | KPIs, distribución por estado, SLA y stock crítico |
| **Clientes y usuarios** | Registro de clientes y administración de personal |

---

## Ciclo de vida del ticket

```
RECIBIDO → EN_DIAGNOSTICO → APROBADO → CERRADO (mediante Swap)
                         ↘ RECHAZADO → CERRADO
```

- Cada transición requiere **informe técnico** registrado en el log de trazabilidad.
- Los tickets **aprobados** se cierran exclusivamente mediante el proceso de **Swap**.
- La evidencia fotográfica es obligatoria al abrir un ticket.
- El cálculo de días de garantía excluye domingos y feriados según política de Boxparts.

---

## Stack tecnológico

| Capa | Tecnología |
|------|------------|
| **Lenguaje** | TypeScript |
| **Frontend** | React, Next.js |
| **Estilos** | Tailwind CSS |
| **Backend** | NestJS (Node.js) |
| **Base de datos** | PostgreSQL (Supabase) |
| **Almacenamiento** | Supabase Storage (evidencias fotográficas) |
| **ORM** | TypeORM |
| **Autenticación** | JWT (JSON Web Tokens) |
| **API** | RESTful |
| **Diseño UI/UX** | Figma |
| **Control de versiones** | Git / GitHub |
| **Pruebas de API** | Postman |
| **IDE** | Visual Studio Code |

### Metodología de desarrollo

Desarrollo bajo metodología ágil **Scrum**, organizado en **6 Sprints** de 2 semanas dentro de un ciclo de 18 semanas, con entregas incrementales de módulos críticos: base de datos y seguridad, interfaz de tickets, swap con inventario y dashboard gerencial.

---

## Requerimientos funcionales

| ID | Requerimiento |
|----|---------------|
| RF-01 | Autenticación y autorización por roles |
| RF-02 | Generación de tickets de garantía |
| RF-03 | Validación automática de números de serie (S/N) |
| RF-04 | Carga de evidencia fotográfica |
| RF-05 | Actualización de estados del ticket |
| RF-06 | Consulta de stock en tiempo real |
| RF-07 | Ejecución de cambio de producto (Swap) |
| RF-08 | Dashboard gerencial con indicadores estadísticos |
| RF-09 | Cálculo de tiempos de resolución (SLA) |
| RF-10 | Trazabilidad e historial por DNI o S/N |

---

## Documentación

- **[Documentación técnica](./DOCUMENTACION_TECNICA.md)** — Arquitectura, requerimientos no funcionales, casos de uso CU-01 a CU-06, modelamiento UML y reglas de negocio.

Dentro de la aplicación, el módulo **Guía del sistema** describe los flujos operativos por rol.

---

## Equipo de desarrollo

| Integrante | Código |
|------------|--------|
| Marden Erick Estrada Guerrero | 001256058 |
| Roy Fernando Manrique Mateo | 001516430 |

**Asesor:** Joe Alexander Castillo Liñán  
**Empresa:** Boxparts SAC — Representante legal: Marco Antonio Cuba Cuzquisiban  
**Institución:** SENATI — Escuela de Tecnología de la Información

---

## Licencia

Proyecto académico e institucional desarrollado para Boxparts SAC. Todos los derechos reservados.
