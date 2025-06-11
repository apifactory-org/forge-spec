# ForgeSpec v1.0 — Especificación Técnica

**ForgeSpec** es una especificación independiente y formal para el modelado estructurado de entidades y procesos de negocio. Su objetivo principal es servir como una fuente única de verdad, a partir de la cual se pueden generar artefactos técnicos como contratos OpenAPI (OAS3), definiciones AsyncAPI, esquemas de validación, documentación técnica, SDKs, y más.

Esta especificación se orienta a organizaciones que requieran coherencia semántica, automatización de procesos técnicos y alineación entre capas de arquitectura mediante una definición de modelo unificada y extensible.

---

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Principios de Diseño](#principios-de-diseño)
3. [Estructura del Documento](#estructura-del-documento)
4. [Tipos de Modelo](#tipos-de-modelo)

   * [`entity`](#modelo-de-tipo-entity)
   * [`process`](#modelo-de-tipo-process)
5. [Definición de Campo (`Field`)](#definición-de-campo-field)
6. [Reglas de Validación](#reglas-de-validación)
7. [Ejemplos de Implementación](#ejemplos-de-implementación)
8. [Proyecciones de Evolución](#proyecciones-de-evolución)

---

## Introducción

ForgeSpec introduce una aproximación sistemática para la descripción de modelos de dominio y operaciones funcionales. Está diseñado para ser independiente de la tecnología subyacente, permitiendo su adopción en ecosistemas heterogéneos donde es deseable alinear definiciones de negocio, interfaces técnicas y estructuras de datos en un marco común y versionado.

---

## Principios de Diseño

* **Neutralidad tecnológica**: ForgeSpec no impone un stack técnico específico.
* **Consistencia semántica**: Terminología clara, precisa y estandarizada en inglés técnico.
* **Multilingüismo documental**: Las descripciones pueden escribirse en cualquier idioma natural.
* **Extensibilidad progresiva**: Diseñado para ampliarse con módulos opcionales y futuras versiones.
* **Automatización orientada a contratos**: Facilita la generación de especificaciones OpenAPI, AsyncAPI y otros artefactos relacionados.

---

## Estructura del Documento

Todo archivo válido basado en ForgeSpec debe incluir los siguientes elementos a nivel raíz:

| Clave       | Tipo   | Obligatorio | Descripción                                           |
| ----------- | ------ | ----------- | ----------------------------------------------------- |
| `forgespec` | string | Sí          | Indica la versión de la especificación utilizada.     |
| `type`      | string | Sí          | Define la naturaleza del modelo: `entity` o `process` |

---

## Tipos de Modelo

### Modelo de tipo `entity`

Este tipo representa una entidad del dominio de negocio, concebida como una estructura persistente susceptible de ser accedida mediante operaciones CRUD.

**Campos obligatorios**:

* `entity` (`string`)
* `fields` (`array` de objetos tipo `Field`)

**Campos opcionales**:

* `description` (`string`)
* `crud` (`object` con claves `create`, `read`, `update`, `delete`)

#### Ejemplo

```yaml
forgespec: "1.0"
type: entity
entity: Empleado
description: Gestión de empleados de la organización
fields:
  - name: id
    type: string
    format: uuid
    required: true
    readOnly: true
    description: Identificador único del empleado
  - name: nombre
    type: string
    required: true
    description: Nombre completo
crud:
  create: true
  read: true
  update: true
  delete: true
```

---

### Modelo de tipo `process`

Este tipo describe operaciones, flujos o comandos de negocio que implican una entrada de datos y una salida esperada. No se orienta a la persistencia sino a la ejecución de lógica aplicada.

**Campos obligatorios**:

* `process` (`string`)
* `input` (`array` de objetos tipo `Field`)

**Campos opcionales**:

* `output` (`array` de objetos tipo `Field`)
* `rest` (`object` con `method` y `path`)
* `description` (`string`)

#### Ejemplo

```yaml
forgespec: "1.0"
type: process
process: calcularTasa
input:
  - name: monto
    type: number
    required: true
  - name: plazo
    type: integer
    required: true
output:
  - name: tasa
    type: number
  - name: mensaje
    type: string
rest:
  method: post
  path: /calcular-tasa
```

---

## Definición de Campo (`Field`)

Los objetos de tipo `Field` representan las unidades de información básicas, tanto en estructuras de entidades como en entradas y salidas de procesos.

| Clave         | Tipo    | Obligatorio | Descripción                                                       |
| ------------- | ------- | ----------- | ----------------------------------------------------------------- |
| `name`        | string  | Sí          | Nombre técnico del campo                                          |
| `type`        | string  | Sí          | Tipo de dato (`string`, `integer`, `boolean`, `number`, etc.)     |
| `format`      | string  | No          | Formato específico (`uuid`, `email`, `date`, etc.)                |
| `required`    | boolean | No          | Indica si el campo es obligatorio en entradas                     |
| `readOnly`    | boolean | No          | Indica si el campo solo debe figurar en respuestas (solo lectura) |
| `description` | string  | No          | Descripción en lenguaje natural del propósito del campo           |

---

## Reglas de Validación

* Todo documento debe declarar explícitamente `forgespec` y `type`.
* Si `type` es `entity`, deben estar presentes `entity` y `fields`.
* Si `type` es `process`, deben estar presentes `process` e `input`.
* Cada objeto `Field` debe contener al menos `name` y `type`.
* Claves adicionales no documentadas no están permitidas en la versión actual.

---

## Ejemplos de Implementación

El repositorio incluye un directorio `/examples/` con múltiples documentos ForgeSpec de referencia para diferentes casos de uso. Se incluyen variantes válidas e inválidas para facilitar el desarrollo de validadores.

---

## Proyecciones de Evolución

Entre las futuras extensiones previstas para ForgeSpec se incluyen:

* Relaciones entre entidades (`hasOne`, `hasMany`, `belongsTo`)
* Inclusión y composición de modelos (`extends`, `includes`)
* Traits reutilizables y modularización
* Anotaciones de seguridad, visibilidad y control de acceso
* Generadores CLI para OpenAPI, AsyncAPI y JSON Schema

---

## Participación

Esta especificación se encuentra en fase de adopción temprana. Las propuestas de mejora, contribuciones técnicas y discusiones arquitectónicas son bienvenidas a través de los canales establecidos en este repositorio.

---

© 2025 ForgeSpec Initiative — Distribuido bajo licencia MIT.
