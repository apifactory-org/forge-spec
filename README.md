# ForgeSpec v1.0 (Especificación Técnica)

**ForgeSpec** es una especificación independiente para el modelado de entidades y procesos de negocio en un formato estructurado, legible por humanos y herramientas. Su propósito principal es fungir como una **fuente de verdad unificada** desde la cual se puedan generar artefactos derivados como contratos OpenAPI, definiciones AsyncAPI, esquemas de validación, documentación técnica, SDKs, y más.

> La especificación define la estructura semántica del modelo; no prescribe comportamientos de implementación ni impone restricciones de plataforma.

---

## 📘 Tabla de Contenidos

1. [Introducción](#introducción)
2. [Filosofía](#filosofía)
3. [Estructura General del Documento](#estructura-general-del-documento)
4. [Tipos de Modelo](#tipos-de-modelo)

   * [`entity`](#tipo-entity)
   * [`process`](#tipo-process)
5. [Especificación de Campos (`Field`)](#especificación-de-campos-field)
6. [Reglas de Validación](#reglas-de-validación)
7. [Ejemplos](#ejemplos)
8. [Hoja de Ruta](#hoja-de-ruta)

---

## 📖 Introducción

ForgeSpec propone un enfoque sistemático para describir modelos de datos y flujos de operaciones. Está diseñado para ser agnóstico a tecnologías de transporte y persistencia, y es particularmente últil en entornos donde se desea alinear múltiples capas de una arquitectura (API, base de datos, lógica de negocio) a partir de una descripción coherente y centralizada.

---

## 🧭 Filosofía

* **Neutralidad estructural**: ForgeSpec no impone ni asume un stack tecnológico.
* **Claridad semántica**: Sus términos son explícitos, consistentes y en inglés técnico.
* **Internacionalización**: Los elementos descriptivos pueden escribirse en cualquier idioma natural.
* **Extensibilidad progresiva**: El modelo está diseñado para ser extendido en futuras versiones (por ejemplo, con relaciones, herencia o seguridad).
* **Generatividad**: Su estructura permite la producción automatizada de artefactos como OpenAPI y AsyncAPI.

---

## 🧱 Estructura General del Documento

Un archivo válido ForgeSpec debe contener los siguientes elementos en su nivel raíz:

| Clave       | Tipo   | Obligatorio | Descripción                          |
| ----------- | ------ | ----------- | ------------------------------------ |
| `forgespec` | string | ✔           | Versión del estándar (ej. `"1.0"`)   |
| `type`      | string | ✔           | Tipo de modelo: `entity` o `process` |

---

## 🤩 Tipos de Modelo

### Tipo `entity`

Representa una entidad del dominio, entendida como una unidad estructural que usualmente es persistente y accedida mediante operaciones CRUD.

**Campos adicionales requeridos**:

* `entity` (`string`)
* `fields` (`array` de objetos tipo `Field`)

**Opcionales**:

* `description` (`string`)
* `crud` (`object` con claves `create`, `read`, `update`, `delete`)

#### Ejemplo

```yaml
forgespec: "1.0"
type: entity
entity: Empleado
description: Gestión de empleados
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

### Tipo `process`

Representa una operación, flujo o comando de negocio. No se enfoca en persistencia, sino en ejecución lógica basada en entrada/salida.

**Campos adicionales requeridos**:

* `process` (`string`)
* `input` (`array` de objetos tipo `Field`)

**Opcionales**:

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

## 🧬 Especificación de Campos (`Field`)

Los objetos `Field` son el componente estructural de los modelos. Se utilizan tanto en entidades (`fields`) como en procesos (`input`, `output`).

| Clave         | Tipo    | Obligatorio | Descripción                                      |
| ------------- | ------- | ----------- | ------------------------------------------------ |
| `name`        | string  | ✔           | Nombre del atributo                              |
| `type`        | string  | ✔           | Tipo base (`string`, `integer`, `boolean`, etc.) |
| `format`      | string  | ✖           | Formato específico (`uuid`, `email`, etc.)       |
| `required`    | boolean | ✖           | Si es obligatorio en el contexto de entrada      |
| `readOnly`    | boolean | ✖           | Solo lectura (presente solo en respuestas)       |
| `description` | string  | ✖           | Descripción textual libre                        |

---

## 📊 Reglas de Validación

* Todo documento debe contener `forgespec` y `type`.
* Si `type: entity`, deben existir `entity` y `fields`.
* Si `type: process`, deben existir `process` e `input`.
* Todos los objetos `Field` deben tener `name` y `type`.
* No se permiten claves adicionales fuera del esquema previsto.

---

## 🧪 Ejemplos

Se incluyen ejemplos en el directorio `/examples/`, tanto válidos como inválidos, para facilitar la validación y comprensión.

---

## 🚧 Hoja de Ruta

ForgeSpec será extendido en versiones futuras con:

* Soporte para relaciones (`hasOne`, `hasMany`, etc.)
* Composición e inclusión de modelos (`extends`, `includes`)
* Traits reutilizables
* Anotaciones de seguridad, autorización y visibilidad
* Generadores CLI para OAS3, AsyncAPI y schemas JSON

---

## 📩 Participación

Este proyecto está en etapa de diseño. Se agradecen sugerencias, críticas constructivas y pruebas de adopción. Puedes abrir un issue o forkear este repositorio para proponer mejoras.

---

© 2025 — ForgeSpec Initiative. Libre para uso académico, comercial y comunitario bajo licencia MIT.
