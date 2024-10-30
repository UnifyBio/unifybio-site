
# Concepts

The core of UnifyBio's functionality is built around
[Unify](https://github.com/vendekagon-labs/unify/), a domain and schema agnostic tool
for data-driven ETL, focused around harmonizing scientific data into a hybrid distributed
data system, consisting of one or many
[Datomic]() databases and an object store such as
[Amazon s3](https://aws.amazon.com/s3/).

UnifyBio is a set of libraries, schemas, and various tools that make use of the Unify CLI
and its schema annotations, as well as common conventions for biological data,
in order to harmonize disparate data into common data models, store data in granular
time pronvenaced storage, and to provide data consistency and quality checks, along with
an ecosystem of downstream access and analysis tools that 


### unify CLI

The Unify CLI is an executable jar file, optionally released or deployed with
additional configuration and data, with execution wrapped via shell scripts.
Unify is open source and Apache licensed and available at
[this repository](https://github.com/vendekagon-labs/unify/).

By convention, UnifyBio projects such as the Pattern Data Commons distribute
Unify as part of a software package/distribution in a `bin/` subdirectory,
where it can be accessed via
commands such as:

```bash
bin/unify --import-config ~/import-name/config.yaml --working-directory ~/prepared-data/import-name
```

### datomic

Unify uses Datomic as its central store of record, and stores all relational
data in Datomic databases. Unify is designed to be able to work with multiple
Datomic databases, and most UnifyBio workflows expect multiple databases, possibly
containing multiple versions of datasets, with a central database which indexes
and tracks dataset states and readiness.

### unify schema

A Unify schema as stored in Datomic consists of four different components:

- Unify's own schema, such as attribute definitions used by the metamodel, or
  import metadata and attributes.
- Base Datomic schema, e.g. all the attributes used in a particular data model.
- The metamodel, a list of schema annotations about relations between entities
  and which attributes encode different kinds of identity (more in metamodel section).
- The enum set: a list of all enum values used in the schema.

For example full schemas, see:

- the reference [CANDEL schema](https://github.com/vendekagon-labs/unify/tree/main/test/resources/systems/candel/template-dataset/schema)
  in the Vendekagon Labs Unify repository.

(Links TBD)

See the schema docs for more information about schemas.

### the unify metamodel

Unify's metamodel enables the Unify CLI's declarative, data-driven ETL process. The metamodel encodes relations between entities
by annotating relevant attributes, e.g. that the `:dataset/assays` attribute points to `assay` entities, and that an `assay` kind
is a child of the `dataset`, and has children `measurement sets`, which have children `measurements`, and so on.

The metamodel also

### dataset

### reference data

### seed data

### import config files

### prepare task

### transact task

### inference tasks

### database management

### unify's compact schema DSL

## Datomic

### datalog query

## object storage and large integrated data



