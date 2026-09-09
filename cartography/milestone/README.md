# Institutive Cartography Milestone Schema

This directory contains the schemas for statically defined **Institutive Cartography Milestones**.

Schema directory: https://github.com/amiasea/institutive-schemas/tree/main/cartography/milestone

## Property Reference

| Property      | Type            | Required | Description                                   |
| ------------- | --------------- | -------: | --------------------------------------------- |
| `name`        | string          |      Yes | Identity and semantic name of the Milestone.  |
| `version`     | string          |      Yes | Semantic version of the Milestone definition. |
| `description` | string          |      Yes | Semantic definition of the Milestone.         |
| `claims`      | array of string |      Yes | Claims required by the Milestone contract.    |

### Constraints

* `name` uses lowercase kebab-case.
* `version` must conform to Semantic Versioning 2.0.0.
* `claims` must contain non-empty strings.
* `claims` must contain unique values.
* Additional properties are not permitted.

## Versioning

Schema revisions are stored in separate version directories.

```text
milestone/
├── README.md
└── 1.0.0/
    └── schema.json
```

The schema version identifies the **schema contract**, not the version of an individual Milestone definition.

The `version` property belongs to the Milestone configuration itself.

## Change Log

### 1.0.0

* Initial semantics.