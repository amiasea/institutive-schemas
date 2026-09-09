# Institutive Cartography Position Schema

This directory contains the schemas for statically defined **Institutive Cartography Positions**.

Schema directory: https://github.com/amiasea/institutive-schemas/tree/main/cartography/position

## Property Reference

| Property    | Type                                                                                        | Required | Description                                          |
| ----------- | ------------------------------------------------------------------------------------------- | -------: | ---------------------------------------------------- |
| `order`     | integer                                                                                     |      Yes | Ordinal position of the Milestone within the Course. |
| `milestone` | [Milestone](https://github.com/amiasea/institutive-schemas/tree/main/cartography/milestone) |      Yes | The Milestone realized at this Position.             |

### Constraints

* `order` must be an integer greater than or equal to `1`.
* `milestone` must conform to the applicable Milestone schema.
* The Position schema does not determine whether a Position is currently established for a solution.
* Position collection constraints, such as unique ordering, are imposed by the Course schema.
* Additional properties are not permitted.

## Versioning

Schema revisions are stored in separate version directories.

```text
position/
├── README.md
└── 1.0.0/
    └── schema.json
```

The schema version identifies the **schema contract**. It is independent of the version of the Milestone referenced by a Position.

## Change Log

### 1.0.0

* Initial semantics.