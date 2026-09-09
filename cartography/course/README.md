# Institutive Cartography Course Schema

This directory contains the schemas for statically defined **Institutive Cartography Courses**.

Schema directory: https://github.com/amiasea/institutive-schemas/tree/main/cartography/course

## Property Reference

| Property    | Type                                                                                               | Required | Description                                  |
| ----------- | -------------------------------------------------------------------------------------------------- | -------: | -------------------------------------------- |
| `name`      | string                                                                                             |      Yes | Identity and semantic name of the Course.    |
| `positions` | array of [Position](https://github.com/amiasea/institutive-schemas/tree/main/cartography/position) |      Yes | The ordered Positions comprising the Course. |

### Constraints

* `name` uses lowercase kebab-case.
* `positions` must contain at least one Position.
* Each Position must conform to the Position schema.
* Position `order` values must be unique.
* `milestone.name` values must be unique across the Course.
* Additional properties are not permitted.

The Course schema represents a **fully composed static Course configuration**. It does not represent a dynamic Course instance or current solution maturity.

## Versioning

Schema revisions are stored in separate version directories.

```text
course/
├── README.md
└── 1.0.0/
    └── schema.json
```

The schema version identifies the **schema contract**, independent of the version of any Course configuration validated by it.

## Change Log

### 1.0.0

* Initial semantics.