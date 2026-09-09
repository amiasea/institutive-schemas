# Institutive Cartography Schemas

This directory contains the authoritative schemas for the **static configuration layer of Institutive Cartography**.

The schemas define the structural contracts for static Cartography configuration that is established as versioned artifacts and subsequently consumed by the Cartography mechanical system.

The schemas do **not** define the Cartography data plane.

```text
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

The Cartography schema namespace currently contains three independently versioned schema families:

```text
cartography/

├── milestone/
│   └── 1.0.0/
│       └── schema.json
│
├── position/
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

```text
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

The initial Milestone definition contains:

```text
name
version
description
claims
```

The `name` establishes the semantic identity of the Milestone.

The `version` identifies the revision of the static Milestone configuration represented by the artifact. It is the version of the Milestone definition/package, not the version of this schema.

For example:

```text
declared@1.2.0
```

identifies the Milestone whose semantic identity is `declared` and whose static definition is revision `1.2.0`.

Milestones are independently defined and versioned so that a particular Milestone revision can be composed into Courses.

### Position

The Position schema defines the structure of a single static Position within a Course.

The initial schema is:

```text
cartography/position/1.0.0/schema.json
```

A Position establishes the ordered location of exactly one Milestone within a Course.

The initial Position definition contains:

```text
order
milestone
```

Conceptually:

```text
Position

├── order
└── milestone
    ├── name
    ├── version
    ├── description
    └── claims
```

The `milestone` property is a realized Milestone instance.

A Position does **not** contain:

* a Milestone revision collection;
* a registry slot;
* a list of alternative Milestones;
* multiple Milestone versions; or
* runtime maturity state.

The multiplicity is deliberately simple:

> **One Position contains exactly one realized Milestone.**

Registry mechanics may expose many revisions of a Milestone, but a realized static Course configuration contains one concrete Milestone instance at each Position.

For example:

```text
Position 3

    order = 3

    milestone
        name = verified
        version = 2.1.0
```

The Position therefore does not choose among revisions dynamically.

Course composition has already selected the particular Milestone revision that becomes the realized Milestone at that Position.

### Course

The Course schema defines the structure of the **fully composed static Course configuration** that is ready to be consumed by Course object instantiation.

The Course schema is independently versioned from the Milestone and Position schemas.

Conceptually:

```text
Milestone definitions

        ↓

Course composition

        ↓

realized Positions

        ↓

fully composed static Course configuration

        ↓

Course schema validation

        ↓

Course instantiation
```

The Course schema therefore describes the resulting static configuration rather than the runtime Course object.

The Course object model and instantiation mechanics belong to the `institutive-cartography` repository.

A Course contains:

```text
name
positions
```

Each Position contains exactly one realized Milestone.

For example:

```text
Course

    Position 1
        order = 1
        milestone = declared@1.2.0

    Position 2
        order = 2
        milestone = implemented@1.4.0

    Position 3
        order = 3
        milestone = verified@2.1.0

    Position 4
        order = 4
        milestone = released@1.1.0

    Position 5
        order = 5
        milestone = deployed@1.0.0
```

The Course is therefore a realized static configuration.

It is not a reflection of the registry's available Milestone revisions.

The registry may contain:

```text
verified@1.0.0
verified@2.0.0
verified@2.1.0
verified@3.0.0
```

while a particular Course contains exactly one realized instance:

```text
verified@2.1.0
```

The Course schema validates that realized configuration.

---

# Static Configuration

These schemas describe **defined expectations**, not the current state of engineering reality.

Static configuration includes concepts such as:

* Milestone definitions;
* Milestone revisions;
* Course compositions;
* realized Positions;
* selected Milestone revisions;
* required Claims;
* other declared Cartography configuration.

Dynamic facts such as Attestations, Receipts, Milestone Status, workflow results, deployments, and established state are not represented by these schemas.

The distinction is fundamental:

```text
static configuration

    =

what is defined


dynamic state

    =

what has actually been established
```

Dynamic state is evaluated against the static configuration.

It does not rewrite the configuration.

A change to static expectations is therefore a configuration change that must be established through source control and a new static artifact revision.

---

# Milestone Schema

The Milestone schema establishes the structural contract for an individual Milestone definition.

The initial definition contains:

```text
name
version
description
claims
```

### Name

The Milestone name is a prominent top-level identifier and intentionally uses a shallow lowercase kebab-case convention.

Examples include:

```text
declared

implemented

verified

released

deployed
```

The initial naming grammar is:

```regex
^[a-z0-9]+(?:-[a-z0-9]+)*$
```

Dot notation is deliberately not used for Milestone names.

A Milestone is a primary concept in a short sequential Course. Its name should not imply an extensible namespace hierarchy.

The `name` establishes the identity and semantic name of the Milestone.

### Version

The `version` property identifies the revision of the static Milestone configuration.

It is distinct from the schema version.

For example:

```text
milestone

    name = verified
    version = 2.1.0
```

means:

```text
Milestone identity
    = verified

Milestone static configuration revision
    = 2.1.0
```

It does **not** mean that the Milestone was validated against schema `2.1.0`.

Schema identity and Milestone artifact identity are separate concerns.

### Claims

The Claim representation is intentionally less constrained at this stage.

The initial schema represents Claims as unique strings because the Claim model has not yet demonstrated a need for additional structure.

The Claim model is expected to evolve as actual Milestone definitions demonstrate what structure is required.

---

# Position Schema

The Position schema establishes the structural contract for a single Position within a Course.

The initial definition contains:

```text
order
milestone
```

### Order

The `order` property identifies the ordinal location of the Position within the Course.

The initial schema requires:

```text
integer
minimum = 1
```

Course-level validation establishes uniqueness of Position order across the Position collection.

The Position schema itself is responsible for establishing that an individual `order` value has the correct structural form.

### Milestone

The `milestone` property is a required reference to exactly one Milestone instance.

The Position schema therefore composes the Milestone schema:

```text
Position

    ↓

milestone

    ↓

Milestone schema
```

The Position does not introduce another revision dimension for its Milestone.

The selected Milestone's `version` is already part of the realized Milestone instance.

Conceptually:

```text
Position

    order = 3

    milestone
        name = verified
        version = 2.1.0
```

The Position is therefore the boundary at which the Course's ordered location is associated with one concrete Milestone definition.

---

# Course Schema

The Course schema validates the static result of Course composition.

A Course is not assumed to be a catalog containing every possible Milestone.

Instead, a Course composition deliberately selects independently versioned Milestone artifacts and realizes them as ordered Positions.

For example:

```text
Course

    Position 1 → declared@1.2.0

    Position 2 → implemented@1.4.0

    Position 3 → verified@2.1.0

    Position 4 → released@1.1.0

    Position 5 → deployed@1.0.0
```

The resulting composed configuration is what the Course schema validates.

The Course therefore does not contain a collection of possible Milestone revisions for each Position.

It contains one realized Milestone at each Position.

Conceptually:

```text
Milestone registry

    verified
       ├── 1.0.0
       ├── 2.0.0
       └── 2.1.0

              ↓ composition

Course

    Position 3
        milestone
            name = verified
            version = 2.1.0
```

The Course schema therefore validates the realization of composition rather than the mechanics of the registry.

## Course-Level Collection Constraints

The Course schema establishes constraints over the collection of Positions.

At minimum:

1. Position `order` values must be unique.
2. `Position.milestone.name` values must be unique.

The first constraint applies directly to a Position property:

```text
positions[*].order
```

The second applies to a nested property:

```text
positions[*].milestone.name
```

This means a Course cannot contain:

```text
Position 1 → declared@1.0.0

Position 2 → declared@2.0.0
```

even though those are different Milestone revisions.

The Milestone identity is `name`.

The `version` identifies the selected revision of that identity.

The Course therefore contains at most one realized instance of a given Milestone identity.

The relevant validation intent is:

```text
uniqueItemProperties

    →
    order


uniqueItemPropertyPaths

    →
    /milestone/name
```

The nested uniqueness requirement is expressed as an Institutive Cartography validation vocabulary interpreted by the schema validation machinery.

It does not require the Course object model to duplicate this structural rule in application code.

## Course Composition vs Course Validation

Structural validity and cross-artifact resolution remain separate concerns:

```text
Course schema

    ↓

is the realized configuration structurally valid?


Composition/build machinery

    ↓

were the selected immutable Milestone artifacts resolved
and composed correctly?
```

The Course schema validates the resulting static configuration.

Composition machinery is responsible for obtaining the particular Milestone artifacts that become the values of that configuration.

---

# Independent Versioning

Milestone, Position, and Course schemas have independent lifecycles.

Their versions do not need to advance together.

For example:

```text
cartography/milestone/1.3.0/

cartography/position/1.0.0/

cartography/course/2.1.0/
```

is a valid registry state.

A schema version identifies the revision of the **schema itself**.

It does not identify the version of a Milestone, Position, or Course validated by that schema.

Therefore:

```text
cartography/milestone/1.3.0/schema.json
```

means:

> Milestone schema revision 1.3.0

not:

> Milestone revision 1.3.0

Likewise:

```text
cartography/course/2.1.0/schema.json
```

means:

> Course schema revision 2.1.0

not:

> Course revision 2.1.0

The same distinction applies to the Position schema.

---

# Versioned Schema Addresses

Schema revisions are represented as path components.

The canonical filename remains:

```text
schema.json
```

rather than embedding the version into the filename.

Conceptually:

```text
cartography/

├── milestone/
│   ├── 1.0.0/
│   │   └── schema.json
│   └── 1.1.0/
│       └── schema.json
│
├── position/
│   └── 1.0.0/
│       └── schema.json
│
└── course/
    └── 1.0.0/
        └── schema.json
```

This makes the schema revision part of its address.

It also leaves the version directory available to contain additional files if a schema revision eventually requires supporting definitions or other resources.

Independent versioning remains the default.

---

# Schema Identity

Schema identity is distinct from:

* source location;
* configuration identity;
* Milestone revision;
* Position identity;
* Course revision; and
* artifact identity.

Conceptually:

```text
schema identity

    ≠ source location

    ≠ Milestone revision

    ≠ Position instance

    ≠ Course revision
```

The schema's registry path provides its address.

The schema revision identifies the structural contract represented at that address.

A consumer should therefore be able to identify the required schema by its identity and revision without making the schema's source-relative filesystem location part of the configuration semantics.

---

# Milestone Validation

A Milestone artifact is established through a build that validates its definition against the applicable schema.

The Milestone source does not simply select the schema revision it wishes to use as an authoritative assertion.

The build workflow determines the applicable schema.

Conceptually:

```text
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

The schema contains the structural validation intention.

The schema validation machinery interprets that intention.

---

# Course Validation

A fully composed Course artifact is established through a build that validates its static configuration against the applicable Course schema.

Conceptually:

```text
Course composition

      ↓

realized Course configuration

      ↓

Course build workflow

      ↓

resolve applicable Course schema

      ↓

schema validation

      ↓

successful validation

      ↓

Course artifact
```

The Course schema validates the realized static object:

```text
Course

    └── Positions
          └── Milestone instances
```

It does not perform Course instantiation or evaluate dynamic maturity.

Those responsibilities belong to the Cartography mechanical system.

---

# Artifact Validation and Nested Schemas

The schema hierarchy composes validation responsibilities.

Conceptually:

```text
Course artifact
    ↓
Course schema
    │
    └── positions
          ↓
        Position schema
          │
          └── milestone
                ↓
              Milestone schema
```

The nested schemas establish the structural validity of their respective objects.

The containing schema may additionally establish constraints over the collection or relationships of the contained objects.

For example:

```text
Milestone schema

    → Milestone name grammar


Position schema

    → Position order type
    → Milestone structure through $ref


Course schema

    → Position collection structure
    → Position order uniqueness
    → Milestone identity uniqueness
```

A `$ref` composes the referenced schema's validation.

It does not prevent the containing schema from establishing additional constraints over the resulting object collection.

This permits validation to remain structurally aligned with the object hierarchy.

---

# Milestone Artifact

The established Milestone artifact retains the schema against which the Milestone definition was validated.

Conceptually:

```text
Milestone package

├── Milestone definition
│
└── schema
```

The included schema is the actual schema revision used for that particular artifact.

The artifact may also contain generated metadata identifying:

* schema identity;
* schema revision;
* source identity;
* Milestone identity;
* Milestone revision; and
* other establishment facts.

That metadata describes **how the particular artifact was established**.

It is not part of the semantic Milestone definition and is not package-level distribution metadata.

The exact metadata filename and format remain an implementation question.

The important invariant is:

> **An established Milestone artifact carries the structural contract under which its definition was validated.**

---

# Course Artifact

The same distinction applies to an established Course artifact.

The Course artifact represents the fully composed static Course configuration.

Conceptually:

```text
Course package

├── Course definition
│
├── realized Milestone definitions
│
└── schema
```

The artifact should preserve the structural contract used to validate the realized Course configuration.

Its static configuration contains concrete Milestone instances rather than registry references that require a runtime registry lookup to determine what the Course means.

For example:

```text
Position 3

    milestone
        name = verified
        version = 2.1.0
```

is the realized static value.

The existence of other `verified` revisions in a registry does not alter this Course artifact.

---

# Relationship to Milestone Definitions

The actual Milestone definitions are maintained in:

```text
amiasea/institutive-cartography-milestones
```

That repository defines the consequential Milestones and their static artifact/build mechanics.

This schema repository provides the structural contract consumed by those builds.

The relationship is:

```text
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

```text
amiasea/institutive-cartography
```

That repository contains the runtime concepts and machinery required to consume static configuration and operate on dynamic state.

The relationship is:

```text
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

The authoritative schema language for Cartography is **JSON Schema Draft 2020-12**.

Schemas are stored as:

```text
schema.json
```

JSON Schema provides the structural validation mechanism.

This does not require the source configuration to be permanently coupled to JSON as its human-facing serialization format.

Configuration may be represented in JSON, YAML, or another suitable representation provided that the resulting data can be validated against the authoritative JSON Schema.

The initial design therefore distinguishes:

```text
schema language

    =

JSON Schema


configuration serialization

    =

an independent ergonomics decision
```

XML/XSD is not part of the initial Cartography schema design.

---

# Schema Validation Vocabulary

The Cartography schemas may use standard JSON Schema keywords together with explicitly defined validation vocabulary where the structural requirement cannot be expressed by standard JSON Schema alone.

For example, Course validation requires uniqueness of a nested property across an array of Positions:

```text
positions[*].milestone.name
```

Standard `uniqueItems` does not express uniqueness of a projected nested property.

The Cartography schema therefore uses the intended validation vocabulary:

```text
uniqueItemPropertyPaths
```

with:

```text
/milestone/name
```

The purpose is narrow:

> **Validate that the value resolved by a specified property path is unique across the items of an array.**

This is a validation concern, not a runtime Cartography concern.

The schema remains the declaration of validation intent.

The validator provides the mechanism through which that intent is evaluated.

No separate application-level Course validator should duplicate the same structural rule merely because the schema engine requires an extension to express it.

---

# Static Configuration Is the Boundary

The schema namespace deliberately establishes a boundary between declared configuration and dynamic reality.

The static layer can therefore be understood as:

```text
source-controlled configuration

        ↓

schema validation

        ↓

immutable static artifact

        ↓

object instantiation

        ↓

dynamic hydration
```

The static artifact is not mutated when the world changes.

Instead:

```text
static configuration
        +
dynamic reality
        ↓
Cartography evaluation
```

produces the runtime state.

This distinction prevents observations such as:

```text
Attestation exists
```

or:

```text
workflow succeeded
```

from becoming configuration merely because they were observed.

---

# Independent Versioning of Static Artifacts

Schema versioning and static artifact versioning are separate dimensions.

For a Milestone:

```text
schema version
    = version of the structural contract

Milestone version
    = version of the static Milestone definition
```

For a Course:

```text
schema version
    = version of the structural Course contract

Course version
    = version of the composed Course configuration
```

A Milestone may therefore change its static configuration without requiring a new schema revision.

Likewise, a Course may change its composition without requiring a new Course schema revision.

A schema revision is required only when the structural contract itself changes.

---

# Relationship Between Course and Milestone Revisions

The Course contains realized Milestone instances.

The registry may contain many revisions, but the Course contains one selected revision at each Position.

For example:

```text
Milestone registry

verified
    ├── 1.0.0
    ├── 2.0.0
    └── 2.1.0


Course composition

    ↓

Position 3

    milestone
        name = verified
        version = 2.1.0
```

The Course's static identity therefore includes the selected Milestone revision through the realized Milestone configuration.

The Course does not maintain:

```text
Position 3

    milestone revisions
        ├── 1.0.0
        ├── 2.0.0
        └── 2.1.0
```

That would represent registry mechanics rather than the realized Course.

The Course contains the result of composition.

---

# Relationship Between Position and Course

A Position is meaningful as an ordered element of a Course.

The Position establishes:

```text
order
+
one Milestone
```

The Course establishes:

```text
ordered collection of Positions
```

The Course therefore owns collection-level invariants such as:

```text
Position order uniqueness
```

and:

```text
Milestone identity uniqueness
```

The Position owns the structural validity of its own values.

This produces a clean composition:

```text
Course

    ↓

Positions

    ↓

Milestones
```

rather than making the Course schema independently reproduce the complete structure of every nested object.

---

# Relationship to the Runtime Object Model

The static schema should not be confused with the runtime Cartography object model.

The distinction is:

```text
JSON Schema

    ↓

structural contract


static artifact

    ↓

realized configuration


Cartography object model

    ↓

runtime semantics and behavior


data plane

    ↓

dynamic engineering reality
```

The schema does not define:

* object methods;
* state transitions;
* API behavior;
* persistence behavior;
* dynamic state hydration;
* Attestation processing;
* Receipt processing;
* Milestone Status evaluation;
* Cursor advancement; or
* promotion execution.

Those concerns belong to `institutive-cartography`.

---

# Design Principles

The Cartography schema namespace follows these principles:

1. **Schemas describe static configuration.**

2. **Dynamic Cartography state is outside this schema namespace.**

3. **Milestone, Position, and Course schemas are independently versioned.**

4. **Schema versions are path components rather than filename suffixes.**

5. **Schema versions identify schemas, not the static artifacts validated by those schemas.**

6. **Milestones are independently defined from Courses.**

7. **A Milestone has a semantic identity in `name` and a static configuration revision in `version`.**

8. **A Position contains exactly one realized Milestone.**

9. **A Position does not contain a collection of Milestone revisions.**

10. **A Course contains an ordered collection of realized Positions.**

11. **A Course schema validates the fully composed static Course configuration.**

12. **A Course schema does not currently assume a universal Milestone catalog.**

13. **Milestone names remain deliberately shallow top-level identifiers.**

14. **Milestone names use lowercase kebab-case.**

15. **Course-level uniqueness applies to Position order.**

16. **Course-level uniqueness applies to the semantic identity of the realized Milestone at each Position.**

17. **Milestone revision is not a Position-level multiplicity.**

18. **A realized Course configuration is not a reflection of registry mechanics.**

19. **Milestone builds determine the applicable schema revision.**

20. **A successful Milestone artifact retains the schema used to validate it.**

21. **Static artifact metadata describes establishment rather than semantic definition.**

22. **JSON Schema Draft 2020-12 is the authoritative schema language.**

23. **Schema validation is deterministic.**

24. **Schema validation intention belongs in the schema; the validator interprets that intention.**

25. **Nested schemas establish the structural validity of their respective objects.**

26. **Containing schemas may establish collection-level constraints over nested objects.**

27. **The Cartography object model belongs to `institutive-cartography`.**

28. **Consequential Milestone definitions belong to `institutive-cartography-milestones`.**

29. **Static configuration is not rewritten by dynamic state.**

30. **Schema structure should remain as small as actual engineering requirements permit.**

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

A Course does, however, currently require unique realized Milestone identities across its Positions.

## Position Ordering

What exact additional Course-level rules should govern Position ordering?

The current model establishes:

* Position order is an integer;
* Position order begins at `1`; and
* Position order is unique within a Course.

Whether the Course schema should additionally require strict contiguity such as:

```text
1, 2, 3, 4, 5
```

rather than merely unique positive integers remains an engineering question.

## Course Milestone Composition

What exact build-time mechanism should resolve independently published Milestone artifacts into the realized Milestone instances of a Course?

The static Course configuration should contain the resulting concrete Milestone definitions.

The mechanics by which those definitions are obtained from independently published artifacts remain a composition/build concern.

## Course Composition Validation

Which Course composition rules belong in JSON Schema and which belong in Course composition/build validation?

Potential rules include:

* ordinal uniqueness;
* ordinal contiguity;
* required ordering;
* Milestone identity uniqueness;
* referenced revision existence;
* immutable artifact resolution;
* compatibility between selected artifacts.

The Course schema should express structural constraints where practical.

Composition/build machinery should establish external artifact resolution and composition correctness.

## Schema Selection

How exactly does a Milestone build workflow resolve the applicable schema revision?

The architectural rule is established:

> **The workflow determines the schema.**

The concrete resolution mechanism remains open.

## Artifact Metadata

What exact generated metadata file should accompany an established Milestone or Course artifact?

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

JSON Schema is established as the schema language.

The human-facing serialization format remains open.

## Additional Cartography Schemas

Which additional static Cartography concepts warrant schemas?

The namespace should grow from demonstrated requirements rather than attempting to schema the entire Cartography object model in advance.

---

# Current State

The Cartography schema namespace currently contains:

```text
cartography/

├── course/
│   └── 1.0.0/
│       └── schema.json
│
├── milestone/
│   └── 1.0.0/
│       └── schema.json
│
└── position/
    └── 1.0.0/
        └── schema.json
```

The three initial concrete schemas are:

```text
cartography/milestone/1.0.0/schema.json

cartography/position/1.0.0/schema.json

cartography/course/1.0.0/schema.json
```

The initial structural model is:

```text
Course

    └── Positions

          └── Milestone
                ├── name
                ├── version
                ├── description
                └── claims
```

The resulting Course is a **realized static configuration**.

It contains concrete Milestone instances selected by composition.

The registry's ability to contain multiple Milestone revisions is not represented as multiplicity within the Course.

The immediate objective is to establish the smallest useful structural contracts for Milestones, Positions, and fully composed Courses and then test those contracts against the actual Cartography artifacts.

The schemas should evolve from concrete engineering requirements rather than attempting to anticipate the entire Institutive Cartography model.
