# ForgeSpec v1.0.0 — Especificación Técnica

**ForgeSpec** es una especificación formal, extensible y agnóstica a la plataforma, orientada a la definición estructurada de entidades y procesos de negocio. Su propósito central es establecer una gramática común que permita describir de forma precisa y reutilizable los elementos conceptuales del dominio, habilitando así la generación automatizada de artefactos técnicos.

ForgeSpec promueve la separación entre el modelo de información y sus mecanismos de implementación, facilitando la interoperabilidad, la consistencia semántica y la automatización en arquitecturas basadas en contratos.

---

## Tabla de Contenidos

1. [Objeto y Alcance](#objeto-y-alcance)
2. [Principios Rectores](#principios-rectores)
3. [Esquema General del Documento](#esquema-general-del-documento)
4. [Tipos de Modelo](#tipos-de-modelo)

   * [Modelo de tipo `entity`](#modelo-de-tipo-entity)
   * [Modelo de tipo `process`](#modelo-de-tipo-process)
5. [Definición del Tipo `Field`](#definición-del-tipo-field)
6. [Reglas de Conformidad](#reglas-de-conformidad)
7. [Diseño del Esquema JSON](#diseño-del-esquema-json)
8. [Representación del Metamodelo](#representación-del-metamodelo)
9. [Gestión de Versiones](#gestión-de-versiones)
10. [Implementación](#implementación)
11. [Extensiones y Evolución Esperada](#extensiones-y-evolución-esperada)
12. [Guía de Uso del Schema JSON](#guía-de-uso-del-schema-json)

---

## Objeto y Alcance

La especificación ForgeSpec define un lenguaje de modelado declarativo cuyo dominio de aplicación abarca tanto estructuras de datos persistentes (entidades) como procesos operativos (servicios, flujos, comandos). Su diseño permite representar el conocimiento de negocio de manera estandarizada, legible por humanos y procesable por máquinas.

No constituye un lenguaje de programación ni una especificación de transporte, y no define comportamientos en tiempo de ejecución. ForgeSpec es una capa conceptual sobre la cual pueden construirse generadores, validadores, orquestadores y sistemas de documentación técnica.

---

## Principios Rectores

* **Neutralidad tecnológica**
* **Claridad semántica**
* **Separación de responsabilidades**
* **Extensibilidad progresiva**
* **Consistencia y auditabilidad**

---

## Esquema General del Documento

Todo documento ForgeSpec debe incluir los siguientes elementos en su nivel raíz:

| Clave       | Tipo   | Obligatorio | Descripción                                                  |
| ----------- | ------ | ----------- | ------------------------------------------------------------ |
| `forgespec` | string | Sí          | Versión exacta de la especificación utilizada. Ej.: "1.0.0". |
| `type`      | string | Sí          | Clasificación del modelo: `entity` o `process`.              |

---

## Tipos de Modelo

### Modelo de tipo `entity`

Representa una estructura semántica de datos del dominio, la cual se compone de atributos con nombre, tipo y propiedades adicionales.

**Elementos obligatorios**:

* `entity`
* `fields`

**Elementos opcionales**:

* `description`
* `crud`

---

### Modelo de tipo `process`

Define una operación funcional que toma datos de entrada y puede producir un resultado.

**Elementos obligatorios**:

* `process`
* `input`

**Elementos opcionales**:

* `output`
* `rest`
* `description`

---

## Definición del Tipo `Field`

Un `Field` es un componente atómico de información.

| Clave         | Tipo    | Obligatorio | Descripción                                   |
| ------------- | ------- | ----------- | --------------------------------------------- |
| `name`        | string  | Sí          | Nombre técnico del campo.                     |
| `type`        | string  | Sí          | Tipo de dato base.                            |
| `format`      | string  | No          | Formato específico para validación semántica. |
| `required`    | boolean | No          | Campo obligatorio en entradas.                |
| `readOnly`    | boolean | No          | Solo aparece en respuestas.                   |
| `description` | string  | No          | Comentario opcional.                          |

---

## Reglas de Conformidad

Un documento es conforme si:

* Tiene `forgespec` y `type` en la raíz.
* Contiene los elementos obligatorios según su tipo (`entity` o `process`).
* Los objetos `Field` contienen al menos `name` y `type`.
* No hay claves no documentadas fuera de extensiones aprobadas.

---

## Diseño del Esquema JSON

El esquema `forgespec.schema.json` permite validar sintáctica y estructuralmente los documentos ForgeSpec.

Se puede integrar con herramientas como `ajv` o `jsonschema` y en pipelines CI/CD.

[Consulta aquí la guía detallada de uso del esquema JSON](./using_schema.md)

---

## Representación del Metamodelo

![ForgeSpec Metamodel](./0e3c5c8a-7b1f-48c0-acd0-5a535b9ba616.png)

* `Document` es la raíz
* `EntityModel` y `ProcessModel` derivan de `Model`
* `Field` es la unidad de estructura

[Descargar metamodelo (PNG)](https://github.com/apifactory-org/forge-spec/raw/main/0e3c5c8a-7b1f-48c0-acd0-5a535b9ba616.png)

---

## Gestión de Versiones

Versión semántica `x.y.z`:

* `x`: cambios incompatibles
* `y`: nuevas funciones compatibles
* `z`: correcciones menores

Cada documento debe declarar su versión.

---

## Implementación

La [guía de implementación](./implementation.md) incluye:

* Ejemplos validados
* Validación estructural
* Herramientas recomendadas
* Generación de contratos

---

## Extensiones y Evolución Esperada

Futuras capacidades:

* Relaciones (`hasOne`, `hasMany`)
* Herencia (`extends`, `includes`)
* Traits reutilizables
* Seguridad y visibilidad
* Compatibilidad inversa
* Exportadores OpenAPI, AsyncAPI, JSON Schema

---

## Guía de Uso del Schema JSON

Para validar archivos ForgeSpec usando `forgespec.schema.json`, consulte:
[./using\_schema.md](./using_schema.md)

---

© 2025 ForgeSpec Initiative — Esta especificación se distribuye bajo licencia MIT.
