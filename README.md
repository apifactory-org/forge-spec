# ForgeSpec v1.0 — Especificación Técnica

**ForgeSpec** es una especificación formal para el modelado estructurado de entidades y procesos de negocio. Su objetivo principal es proporcionar un lenguaje de definición neutral, claro y reusable para representar conceptos del dominio en arquitecturas orientadas a servicios o centradas en contratos.

La especificación se concibe como una base normativa para herramientas que generen artefactos derivados, sin prescribir mecanismos de implementación específicos.

---

## Tabla de Contenidos

1. [Objeto y Alcance](#objeto-y-alcance)
2. [Principios Rectores](#principios-rectores)
3. [Estructura del Documento](#estructura-del-documento)
4. [Tipos de Modelo](#tipos-de-modelo)

   * [`entity`](#modelo-de-tipo-entity)
   * [`process`](#modelo-de-tipo-process)
5. [Definición de Campo (`Field`)](#definición-de-campo-field)
6. [Reglas de Conformidad](#reglas-de-conformidad)
7. [Proyecciones de Evolución](#proyecciones-de-evolución)

---

## Objeto y Alcance

ForgeSpec establece una gramática semántica para describir modelos de datos y operaciones de negocio en forma estructurada. Está orientado a organizaciones que necesitan estandarizar la representación de conceptos técnicos y de negocio en múltiples dominios y capas arquitectónicas.

ForgeSpec no define comportamientos en tiempo de ejecución ni prescribe herramientas o librerías específicas. Tampoco impone estructuras de transporte o almacenamiento.

---

## Principios Rectores

* **Neutralidad tecnológica**: Independencia respecto de herramientas, lenguajes y plataformas.
* **Claridad semántica**: Terminología controlada y estructuración explícita.
* **Separación de definiciones**: La definición del modelo es independiente de su implementación o derivación.
* **Extensibilidad**: El diseño admite evolución sin rupturas.

---

## Estructura del Documento

Todo documento conforme a ForgeSpec debe incluir los siguientes elementos a nivel raíz:

| Clave       | Tipo   | Obligatorio | Descripción                                        |
| ----------- | ------ | ----------- | -------------------------------------------------- |
| `forgespec` | string | Sí          | Versión de la especificación en uso. Ej.: `"1.0"`. |
| `type`      | string | Sí          | Naturaleza del modelo: `entity` o `process`.       |

---

## Tipos de Modelo

### Modelo de tipo `entity`

Define una entidad del dominio, entendida como un conjunto de campos identificables, típicamente persistente y susceptible de operaciones de consulta y modificación.

**Elementos requeridos**:

* `entity`: Identificador técnico del modelo.
* `fields`: Conjunto ordenado de descriptores de datos (`Field`).

**Elementos opcionales**:

* `description`: Texto libre explicativo.
* `crud`: Objeto que define operaciones permitidas sobre la entidad (`create`, `read`, `update`, `delete`).

---

### Modelo de tipo `process`

Describe una operación lógica del sistema que puede ser invocada mediante datos de entrada definidos. Su propósito es representar flujos de ejecución que no dependen directamente de una entidad persistente.

**Elementos requeridos**:

* `process`: Nombre del proceso.
* `input`: Lista estructurada de parámetros de entrada (`Field`).

**Elementos opcionales**:

* `output`: Lista estructurada de datos de salida (`Field`).
* `rest`: Objeto que especifica un punto de acceso HTTP (`method`, `path`).
* `description`: Descripción general del proceso.

---

## Definición de Campo (`Field`)

Los objetos `Field` describen las unidades semánticas mínimas que componen un modelo, ya sea en el contexto de entidades o procesos.

| Clave         | Tipo    | Obligatorio | Descripción                                               |
| ------------- | ------- | ----------- | --------------------------------------------------------- |
| `name`        | string  | Sí          | Nombre técnico del campo.                                 |
| `type`        | string  | Sí          | Tipo base: `string`, `integer`, `boolean`, `number`, etc. |
| `format`      | string  | No          | Formato específico: `uuid`, `email`, `date`, etc.         |
| `required`    | boolean | No          | Indica si el campo es obligatorio en entrada.             |
| `readOnly`    | boolean | No          | Define si el campo es de solo lectura.                    |
| `description` | string  | No          | Comentario textual libre.                                 |

---

## Reglas de Conformidad

* Los documentos deben declarar explícitamente `forgespec` y `type`.
* En modelos de tipo `entity`, deben especificarse `entity` y `fields`.
* En modelos de tipo `process`, deben especificarse `process` e `input`.
* Todos los objetos de tipo `Field` deben incluir, como mínimo, `name` y `type`.
* No se admiten claves adicionales no especificadas, salvo futuras extensiones.

---

## Proyecciones de Evolución

Las siguientes funcionalidades se consideran para versiones futuras:

* Definición de relaciones entre entidades (`hasOne`, `hasMany`, `belongsTo`).
* Composición de modelos (`extends`, `includes`).
* Reutilización de estructuras mediante `traits`.
* Anotaciones de control de acceso, visibilidad y políticas.
* Generación declarativa de artefactos complementarios (OpenAPI, AsyncAPI, JSON Schema).

---

© 2025 ForgeSpec Initiative — Licencia MIT.
