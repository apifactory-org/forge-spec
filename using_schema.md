# Validación de Documentos ForgeSpec con JSON Schema

Esta guía técnica proporciona los pasos necesarios para validar documentos ForgeSpec conforme al esquema oficial `forgespec.schema.json`. La validación asegura que cualquier definición estructural cumpla con las reglas normativas de la especificación ForgeSpec v1.0.0.

Para una explicación detallada sobre cómo instalar validadores, convertir archivos YAML y automatizar esta verificación en procesos CI/CD, consulte la [guía completa de validación](./using_schema.md).

---

## 1. Requisitos Previos

- Tener instalado `Node.js` (v14+)
- Tener acceso al archivo `forgespec.schema.json`
- Contar con uno o más archivos `.yaml` o `.json` que deseen validarse

---

## 2. Instalación de Herramienta de Validación

Se recomienda utilizar [`ajv-cli`](https://github.com/ajv-validator/ajv-cli), una herramienta basada en el validador JSON Schema más ampliamente adoptado.

### Instalación global:

```bash
npm install -g ajv-cli
```

---

## 3. Conversión opcional de YAML a JSON

Si su documento ForgeSpec está en formato YAML, conviértalo a JSON para validarlo:

```bash
yq -o=json eval input.yaml > input.json
```

*Nota: se requiere *[*`yq`*](https://github.com/mikefarah/yq)* para esta operación.*

---

## 4. Validación básica

Ejecute la validación con el siguiente comando:

```bash
ajv validate -s forgespec.schema.json -d input.json --strict=false
```

Donde:

- `-s` especifica la ubicación del esquema
- `-d` apunta al documento a validar
- `--strict=false` permite validaciones más tolerantes con campos adicionales

---

## 5. Validación en CI/CD

Para entornos de integración continua (CI/CD), incorpore el siguiente paso en su pipeline:

```yaml
- name: Validar ForgeSpec
  run: |
    npm install -g ajv-cli
    ajv validate -s ./forgespec.schema.json -d ./modelos/*.json --strict=false
```

---

## 6. Validadores Alternativos

- [JSON Schema Validator (Java)](https://github.com/everit-org/json-schema)
- [Python jsonschema](https://python-jsonschema.readthedocs.io/en/stable/)
- [Visual Studio Code](https://code.visualstudio.com/docs/languages/json#_json-schemas-and-settings) con vinculación de esquema

---

## 7. Errores comunes

- Campos faltantes obligatorios (`name`, `type`, `entity`, etc.)
- Tipos de datos incorrectos
- Esquemas mixtos (`type: entity` con campos de proceso)

---

## Recursos

- [Especificación ForgeSpec](./README.md)
- [Esquema JSON oficial](./forgespec.schema.json)
- [Guía de implementación](./implementation.md)
- [Guía completa de validación](./using_schema.md)

---

© 2025 API Factory / ForgeSpec Initiative

