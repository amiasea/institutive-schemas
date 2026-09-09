# Institutive Cartography Schemas

This directory contains the authoritative schemas for the **static configuration layer of Institutive Cartography**.

The schemas define the structural contracts for static Cartography configuration that is established as versioned artifacts and subsequently consumed by the Cartography mechanical system.

The schemas do **not** define the Cartography data plane.

```text id="c9v5na"
static configuration
        ↓
Cartography schemas
        ↓
validated static artifacts
        ↓
Cartography object instantiation
        ↓
dynamic data plane
```

## Schema Families

The Cartography schema namespace currently contains two independently versioned schema families:

```text id="q6l1hb"
cartography/
├── milestone/
│   └── 1.0.0/
│       └── schema.json
│
└── course/
    └── 1.0.0/
        └── schema.json
```

### Milestone

The Milestone schema defines the structure of an independently defined Cartography Milestone.

A Milestone is a contract of Claims associated with a Position in a Course.

The initial schema is:

```text id="3b0z6n"
cartography/milestone/1.0.0/schema.json
```

The schema deliberately describes the static Milestone definition rather than:

* a Course;
* a Course Position;
* an Attestation;
* a Receipt;
* Milestone Status;
* runtime state; or
* the mechanics by which a Milestone is achieved.

Milestones are independently defined and versioned so that the same Milestone revision may be composed into multiple Courses.

### Course

The Course schema defines the structure of the **fully composed static Course configuration** that is ready to be consumed by Course object instantiation.

The Course schema is independently versioned from the Milestone schema.

Conceptually:

```text id="9d1f4c"
Milestone definitions
        ↓
Course composition
        ↓
fully composed static Course configuration
        ↓
Course schema validation
        ↓
Course instantiation
```

The Course schema therefore describes the resulting static configuration rather than the runtime Course object.

The Course object model and instantiation mechanics belong to the `institutive-cartography` repository.

---

# Static Configuration

These schemas describe **defined expectations**, not the current state of engineering reality.

Static configuration includes concepts such as:

* Milestone definitions;
* Course compositions;
* selected Milestone revisions;
* required Claims;
* other declared Cartography configuration.

Dynamic facts such as Attestations, Receipts, Milestone Status, workflow results, deployments, and established state are not represented by these schemas.

The distinction is fundamental:

```text id="4j7v9q"
static configuration
    =
what is defined

dynamic state
    =
what has actually been established
```

Dynamic state is evaluated against the static configuration. It does not rewrite the configuration.

---

# Milestone Schema

The Milestone schema establishes the structural contract for an individual Milestone definition.

The initial schema is intentionally small.

The initial definition contains:

```text id="1m3v8c"
name
description
claims
```

The Milestone name is a prominent top-level identifier and intentionally uses a shallow lowercase kebab-case convention.

Examples include:

```text id="w8h2az"
declared
implemented
verified
released
deployed
```

The initial naming grammar is:

```regex id="7q2x4p"
^[a-z0-9]+(?:-[a-z0-9]+)*$
```

Dot notation is deliberately not used for Milestone names.

A Milestone is a primary concept in a short sequential Course. Its name should not imply an extensible namespace hierarchy.

The Claim representation is intentionally less constrained at this stage. The Claim model is expected to evolve as actual Milestone definitions demonstrate what structure is required.

---

# Course Schema

The Course schema validates the static result of Course composition.

A Course is not assumed to be a catalog containing every possible Milestone.

Instead, a Course composition deliberately selects independently versioned Milestone artifacts and establishes their ordered progression.

For example:

```text id="2n4j8v"
Course
  Position 1 → declared@1.2.0
  Position 2 → implemented@1.4.0
  Position 3 → verified@2.1.0
  Position 4 → released@1.1.0
  Position 5 → deployed@1.0.0
```

The resulting composed configuration is what the Course schema validates.

The Course schema therefore need not enumerate a universal Milestone catalog unless a future engineering requirement establishes that such a catalog is necessary.

Structural validity and cross-artifact resolution remain separate concerns:

```text id="c7a0mb"
Course schema
    ↓
is the composed configuration structurally valid?

Composition/build machinery
    ↓
do the referenced immutable Milestone artifacts resolve correctly?
```

---

# Independent Versioning

Milestone and Course schemas have independent lifecycles.

Their versions do not need to advance together.

For example:

```text id="g2s8vk"
cartography/milestone/1.3.0/
cartography/course/2.1.0/
```

is a valid registry state.

A schema version identifies the revision of the **schema itself**.

It does not identify the version of a Milestone or Course validated by that schema.

Therefore:

```text id="j1w6cr"
cartography/milestone/1.3.0/schema.json
```

means:

> Milestone schema revision 1.3.0

not:

> Milestone revision 1.3.0

The same distinction applies to Course schemas.

---

# Versioned Schema Addresses

Schema revisions are represented as path components.

The canonical filename remains:

```text id="p9y4tw"
schema.json
```

rather than embedding the version into the filename.

Conceptually:

```text id="5v8n1k"
cartography/
└── milestone/
    ├── 1.0.0/
    │   └── schema.json
    └── 1.1.0/
        └── schema.json
```

This makes the schema revision part of its address.

It also leaves the version directory available to contain additional files if a schema revision eventually requires supporting definitions or other resources.

The registry structure also does not prevent multiple schemas from later being grouped under a coordinated version if an actual requirement for coordinated evolution emerges.

Independent versioning remains the default.

---

# Schema Identity

Schema identity is distinct from:

* source location;
* configuration identity;
* Milestone revision;
* Course revision; and
* artifact identity.

Conceptually:

```text id="e6b4yn"
schema identity
    ≠ source location
    ≠ Milestone revision
    ≠ Course revision
```

The schema's registry path provides its address. The schema revision identifies the structural contract represented at that address.

A consumer should therefore be able to identify the required schema by its identity and revision without making the schema's source-relative filesystem location part of the configuration semantics.

---

# Milestone Validation

A Milestone artifact is established through a build that validates its definition against the applicable schema.

The Milestone source does not simply select the schema revision it wishes to use as an authoritative assertion.

The build workflow determines the applicable schema.

Conceptually:

```text id="w1e9ks"
Milestone source
      ↓
Milestone build workflow
      ↓
resolve applicable schema
      ↓
schema validation
      ↓
successful validation
      ↓
Milestone artifact
```

A failed schema validation prevents successful establishment of the Milestone artifact.

Schema validation is deterministic build validation.

AI review may provide additional semantic or logical assessment, but it does not replace schema validation.

---

# Milestone Artifact

The established Milestone artifact retains the schema against which the Milestone definition was validated.

Conceptually:

```text id="n3j5qb"
Milestone package
├── Milestone definition
└── schema
```

The included schema is the actual schema revision used for that particular artifact.

The artifact may also contain generated metadata identifying:

* schema identity;
* schema revision;
* source identity;
* other establishment facts.

That metadata describes **how the particular artifact was established**.

It is not part of the semantic Milestone definition and is not package-level distribution metadata.

The exact metadata filename and format remain an implementation question.

The important invariant is:

> **An established Milestone artifact carries the structural contract under which its definition was validated.**

---

# Relationship to Milestone Definitions

The actual Milestone definitions are maintained in:

```text id="8k3r1x"
amiasea/institutive-cartography-milestones
```

That repository defines the consequential Milestones and their static artifact/build mechanics.

This schema repository provides the structural contract consumed by those builds.

The relationship is:

```text id="9c2v7m"
institutive-schemas/cartography/milestone
        │
        │ schema
        ▼
institutive-cartography-milestones
        │
        │ Milestone definition
        ▼
Milestone build
        │
        │ validation
        ▼
established Milestone artifact
```

The Milestone repository therefore does not redefine the schema semantics inside every Milestone definition.

The schema establishes the common structural boundary.

Individual Milestone definitions establish their own semantic contracts.

---

# Relationship to Cartography

The complete Cartography object model and mechanical system are maintained in:

```text id="d5j7sp"
amiasea/institutive-cartography
```

That repository contains the runtime concepts and machinery required to consume static configuration and operate on dynamic state.

The relationship is:

```text id="q0m5yc"
static schemas
    ↓
static configuration artifacts
    ↓
Cartography configuration restoration
    ↓
Course instantiation
    ↓
dynamic data-plane state
```

The Cartography runtime should not need to treat the schema registry as its dynamic data store.

The schema establishes the shape of static configuration.

The object model establishes the runtime semantics.

The data plane establishes facts about actual engineering reality.

---

# JSON Schema

The authoritative schema language for Cartography is **JSON Schema**.

Schemas are stored as:

```text id="e3p8nk"
schema.json
```

JSON Schema provides the structural validation mechanism.

This does not require the source configuration to be permanently coupled to JSON as its human-facing serialization format. Configuration may be represented in JSON, YAML, or another suitable representation provided that the resulting data can be validated against the authoritative JSON Schema.

The initial design therefore distinguishes:

```text id="w5g2zr"
schema language
    =
JSON Schema

configuration serialization
    =
an independent ergonomics decision
```

XML/XSD is not part of the initial Cartography schema design.

---

# Design Principles

The Cartography schema namespace follows these principles:

1. **Schemas describe static configuration.**
2. **Dynamic Cartography state is outside this schema namespace.**
3. **Milestone and Course schemas are independently versioned.**
4. **Schema versions are path components rather than filename suffixes.**
5. **Milestones are independently defined from Courses.**
6. **A Course schema validates the fully composed static Course configuration.**
7. **A Course schema does not currently assume a universal Milestone catalog.**
8. **Milestone names remain deliberately shallow top-level identifiers.**
9. **Milestone names use lowercase kebab-case.**
10. **Milestone builds determine the applicable schema revision.**
11. **A successful Milestone artifact retains the schema used to validate it.**
12. **Schema/build metadata describes artifact establishment rather than semantic definition.**
13. **JSON Schema is the initial authoritative schema language.**
14. **Schema validation is deterministic.**
15. **The Cartography object model belongs to `institutive-cartography`.**
16. **Consequential Milestone definitions belong to `institutive-cartography-milestones`.**
17. **Static configuration is not rewritten by dynamic state.**
18. **Schema structure should remain as small as actual engineering requirements permit.**

---

# Open Questions

The following questions remain deliberately open.

## Claim Structure

What is the eventual structural representation and naming discipline for Claims?

The initial Milestone schema treats Claims as unique strings because the Claim model has not yet demonstrated a need for additional structure.

Actual Milestone definitions should drive this evolution.

## Milestone Name Uniqueness

Should a Milestone name be globally unique within Cartography, unique within another scope, or simply identify the semantic definition?

The current schema establishes the naming grammar but does not establish a global namespace.

## Course Milestone References

What exact reference form should a composed Course use to identify an immutable Milestone artifact?

Possible approaches include an identity plus revision, an immutable artifact reference, or another content-addressed reference.

The Course schema should not impose more semantics than the actual composition model requires.

## Course Composition Validation

Which Course rules belong in JSON Schema and which belong in Course composition/build validation?

Potential cross-reference rules include:

* ordinal uniqueness;
* ordinal contiguity;
* required ordering;
* Milestone uniqueness;
* referenced revision existence;
* immutable artifact resolution;
* compatibility between selected artifacts.

These may require validation beyond the expressive scope of the structural schema itself.

## Schema Selection

How exactly does a Milestone build workflow resolve the applicable schema revision?

The architectural rule is established:

> **The workflow determines the schema.**

The concrete resolution mechanism remains open.

## Artifact Metadata

What exact generated metadata file should accompany an established Milestone artifact?

The artifact should preserve the schema identity and revision used during validation, but the exact filename and format remain open.

## Schema Packaging

Does a schema revision require only `schema.json`, or will some future revision require supporting schema resources?

The version-directory structure intentionally leaves this open.

## Coordinated Schema Versions

Will Institutive ever require multiple schema families to evolve under a coordinated version?

The registry structure permits this without requiring it.

No coordinated versioning policy should be introduced until an actual dependency requires one.

## Configuration Serialization

Should Cartography static configuration use JSON, YAML, or support multiple serialization formats?

JSON Schema is established as the schema language. The human-facing serialization format remains open.

## Additional Cartography Schemas

Which additional static Cartography concepts warrant schemas?

The namespace should grow from demonstrated requirements rather than attempting to schema the entire Cartography object model in advance.

---

# Current State

The Cartography schema namespace currently contains:

```text id="r4q6yd"
cartography/
├── course/
│   └── 1.0.0/
│       └── schema.json
│
└── milestone/
    └── 1.0.0/
        └── schema.json
```

The first concrete schema being established is:

```text id="a8m2wf"
cartography/milestone/1.0.0/schema.json
```

The immediate objective is to establish the smallest useful structural contract for Milestone definitions and then test that contract against the actual Milestone artifacts.

The schema should evolve from concrete engineering requirements rather than attempting to anticipate the entire Institutive Cartography model.
