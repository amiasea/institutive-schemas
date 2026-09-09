# institutive-schemas

Authoritative schema registry for Institutive static configuration.

## Schema Organization

Each top-level directory represents a single **schema type**.

A top-level directory must contain schemas expressed using **one schema language or schema specification only**.

Different schema types may use different schema languages where appropriate, but schema languages must not be mixed within the same top-level directory.

For example:

```text
institutive-schemas/
├── cartography/
│   ├── milestone/
│   ├── position/
│   └── course/
│
├── another-schema-type/
│   └── ...
│
└── ...
```

The `cartography/` directory must therefore use one schema specification consistently. In the current Cartography implementation, that specification is **JSON Schema Draft 2020-12**.

A different top-level directory may use a different schema specification if the corresponding Institutive domain requires one.

The rule is:

> **One schema specification per top-level directory.**

This prevents a single schema namespace from becoming a mixture of incompatible schema languages, validation semantics, and tooling requirements.

## Versioned Schemas

Within a schema type, each schema version must exist in its own version directory.

For example:

```text
cartography/
└── milestone/
    ├── 1.0.0/
    │   └── schema.json
    └── 2.0.0/
        └── schema.json
```

The schema's canonical identifier must correspond to the version represented by its directory.

For example:

```text
cartography/milestone/1.0.0/schema.json
```

must contain:

```json
"$id": "https://raw.githubusercontent.com/amiasea/institutive-schemas/main/cartography/milestone/1.0.0/schema.json"
```

and:

```text
cartography/milestone/2.0.0/schema.json
```

must contain:

```json
"$id": "https://raw.githubusercontent.com/amiasea/institutive-schemas/main/cartography/milestone/2.0.0/schema.json"
```

The required invariant is:

```text
schema path version
        =
schema $id version
```

When a schema is formally versioned, the existing version remains immutable and the new version is introduced in a separate version directory.

Repository validation should enforce these invariants.
