# Guía de Implementación — ForgeSpec

Esta página proporciona una introducción práctica al uso de la especificación ForgeSpec mediante ejemplos concretos de modelos, validados conforme a la versión `1.0.0`. Aquí se presentan casos representativos para entidades y procesos, así como orientaciones para la generación de artefactos técnicos derivados.

---

## 1. Modelo de tipo `entity`

```yaml
forgespec: "1.0.0"
type: entity
entity: Producto
fields:
  - name: id
    type: string
    format: uuid
    required: true
    readOnly: true
  - name: nombre
    type: string
    required: true
  - name: precio
    type: number
    required: true
  - name: enStock
    type: boolean
crud:
  create: true
  read: true
  update: true
  delete: true
```

Este modelo describe una entidad `Producto` con atributos clave. El bloque `crud` permite derivar operaciones REST básicas.

---

## 2. Modelo de tipo `process`

```yaml
forgespec: "1.0.0"
type: process
process: calcularPrecioFinal
input:
  - name: precioBase
    type: number
    required: true
  - name: impuesto
    type: number
output:
  - name: precioFinal
    type: number
rest:
  method: post
  path: /precio-final
```

Este proceso realiza un cálculo basado en dos parámetros de entrada y devuelve un valor computado. Se define una interfaz REST para su invocación.

---

## 3. Validación y herramientas

Los archivos ForgeSpec pueden validarse mediante esquemas JSON compatibles con AJV u otras bibliotecas. Se recomienda integrar validación en pipelines CI/CD.

- Herramienta recomendada: [`ajv-cli`](https://github.com/ajv-validator/ajv-cli)
- Validación básica:
  ```bash
  ajv validate -s forgespec.schema.json -d modelo.yaml
  ```

---

## 4. Generación de artefactos (en desarrollo)

En etapas futuras, se dispondrá de:

- Generador de contratos OpenAPI 3.0 desde modelos `entity`
- Generador de definiciones AsyncAPI desde modelos `process`
- Exportación de esquemas JSON Schema a partir de `fields`
- Visualización en UI basada en ForgeSpec

---

Para más ejemplos, consulta el directorio `/examples/` o participa en la evolución de la especificación mediante pull requests.

