# Institutive Cartography Schemas

This directory contains the **authoritative schemas for statically defined Institutive Cartography configuration**.

The schemas define the structural contracts consumed by the Cartography machinery. They do not define dynamic Cartography state such as Course instances, Positions established for a solution, Attestations, Receipts, or Milestone Status.

## Schema Families

```text
Course
  │
  └── Position
        │
        └── Milestone
```

| Schema                                                                                      | Purpose                                              |
| ------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| [Milestone](https://github.com/amiasea/institutive-schemas/tree/main/cartography/milestone) | Defines a statically declared Cartography Milestone. |
| [Position](https://github.com/amiasea/institutive-schemas/tree/main/cartography/position)   | Defines an ordered Position containing a Milestone.  |
| [Course](https://github.com/amiasea/institutive-schemas/tree/main/cartography/course)       | Defines a fully composed, ordered Course.            |

The schemas are composed through JSON Schema `$ref` relationships. Higher-level schemas may impose constraints over the collections of objects they contain.

## Schema Specification

All schemas currently contained in `cartography/` use **JSON Schema Draft 2020-12**.

This directory follows the repository rule:

> **One schema specification per top-level directory.**

The schema specification may change in a future top-level schema directory without requiring every schema in `institutive-schemas` to use the same specification.

## Versioning

Each schema revision is stored in its own version directory:

```text
cartography/
└── milestone/
    └── 1.0.0/
        └── schema.json
```

The version in the path must correspond to the version represented by the schema's canonical `$id`.

Once a schema revision is formally versioned and used, that revision is immutable. A subsequent schema revision receives a new version directory.

Schema versioning is independent of the version of the configuration or artifact validated by the schema.

## Static Configuration Boundary

These schemas describe **declared configuration**.

They must not become a representation of dynamic engineering reality.

```text
Static configuration
    │
    ├── Course
    ├── Position
    ├── Milestone
    └── declared requirements
             │
             ▼
    Cartography machinery
             │
             ▼
Dynamic engineering reality
```

The runtime Cartography object model and dynamic data plane are implemented by [`amiasea/institutive-cartography`](https://github.com/amiasea/institutive-cartography).

Solution-specific Milestone definitions are maintained by [`amiasea/institutive-cartography-milestones`](https://github.com/amiasea/institutive-cartography-milestones).

## Appendix — Schema Property References

The schema-specific READMEs provide human- and AI-oriented property references for their respective schemas:

* [Milestone Property Reference](https://github.com/amiasea/institutive-schemas/tree/main/cartography/milestone)
* [Position Property Reference](https://github.com/amiasea/institutive-schemas/tree/main/cartography/position)
* [Course Property Reference](https://github.com/amiasea/institutive-schemas/tree/main/cartography/course)

The versioned `schema.json` files remain the authoritative machine-readable contracts. The property reference tables document those contracts and are not separate schema definitions.
