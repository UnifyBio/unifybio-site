
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

For one example full schema, see the reference [CANDEL schema](https://github.com/vendekagon-labs/unify/tree/main/test/resources/systems/candel/template-dataset/schema) in the Vendekagon Labs Unify repository.

See the [schema docs](schema.md) for more information about schemas and other example schemas.

### The Unify Metamodel

Unify's metamodel enables the Unify CLI's declarative, data-driven ETL process. The metamodel encodes relations between entities
by annotating relevant attributes, e.g. that the `:dataset/assays` attribute points to `assay` entities, and that an `assay` kind
is a child of the `dataset`, and has children `measurement sets`, which have children `measurements`, and so on.

See the metamodel section of the [schema docs](schema.md) for more details.

### Datasets

The Unify CLI expects that all data being batch imported is modeled within the framework of a dataset.
I.e., subjects, assays, samples, clinical observations, etc. are all part of a particular dataset.
This does not necessarily constraint downstream tooling (e.g. a query can look at all subjects, not just
all subjects for a dataset) but does structure imports and import config files. See the
[Import Config](import-config.md) docs for more information.

### Reference Data

The Unify data model specifies that some of the contents of the database are Reference Data. Reference
data consists of standard identifiers, controlled vocabularies, ontologies, and so on. It also refers
to common conventions that can be used to uniquely and globally identify things like the biological
entities that measurements are intended to target, even when not exhausitvely modeled in the data,
such as variants and genomic coordinates. Some reference data can be imported with the
Unify CLI via special forms in the import config, more information about reference data can be found
in the [schema docs](schema.md).

### Seed Data

Seed data refers to the the portion of reference data which is bulk loaded into UnifyBio databases
on creation. Due to CANDEL terminology priors, this is sometimes referred to as bootsrap data.
This is usually data from a standard source, e.g. genes and gene products from HGNC and
proteins and epitopes from Uniprot. Every UnifyBio should provide seed data (or e.g. scripts
for downloading seed data) as part of their software distribution.

### Import Config Files

An import config file specifies the mapping from however the data being is being imported (e.g.
the ad hoc schema implied by a TSV file's column names) into the particular UnifyBio system's
schema. Import config files are specified in detail in the [import config][import-config.md] docs.

### working directory

The _working directory_ is a directory that the Unify CLI writes to and stores intermediate outputs
and representations in while data is being prepared, and which contains all the contents needed
to transact data.

### prepare task

Before data can be transacted into a Unify system's datomic database and object storage, the import
config must be used to create [edn](https://github.com/edn-format/edn) data, which can be
transacted into Datomic database systems (or, with future planned extensions, into other storages).

Details on running prepare are documented in the [Unify CLI docs](unify-cli.md).

### transact task

Transaction takes data off disk and stores it in the distributed representation model specified by a UnifyBio system.
Once data has been prepared into a working directory, it is ready to be transacted.

Details on running transact are documented in the [Unify CLI docs](unify-cli.md).

### inference tasks

The Unify schema and metamodel are used not only for inference throughout the UnifyBio stack,
but also for integration with other tools and systems. This includes things like inferring a
metaschema for the Trino integration via Datomic analytics, or inferring a JSON schema
for enabling editors to provide static analysis and autocomplete functionality to users
writing import configs.

The available `infer` tasks are documented in the [Unify CLI docs](unify-cli.md).

### Database Management

UnifyBio systems typically manage multiple Datomic databases. Some of this is done through the
Unify CLI, requesting databases to be used for transacting imports and validating data.
In general, mature UnifyBio systems have to implement some degree of database management and
administration as part of dataset curation and historical data preservation for users.

## Datomic

UnifyBio uses Datomic as a central data store for all relational data and some measurement data.
Datomic is a database with a sparse core representation (datoms) that provides granular time travel
capabilities and strong transactional guarantees. See the
[official Datomic docs](https://docs.datomic.com/datomic-overview.html) for more information.

### Datalog query

The core data representation in UnifyBio can be queried directly and efficiently using Datomic's
dialect of datalog. Users should refer to Datomic's
[query reference](https://docs.datomic.com/query/query-data-reference.html) material for full
documentation on capabilities.

### SQL Integration

UnifyBio uses Datomic Analytics' Trino integration to map data from Datomic into tabular views
that can be accessed over SQL through the appropriate Trino or PrestoDB adaptors. See the
[Trino](https://trino.io/docs/current/index.html) and 
[Datomic Analytics](https://docs.datomic.com/analytics/analytics-concepts.html#sql-mapping) docs for more
information on how the integration works and how it can be used to integrate UnifyBio systems
with traditional data warehouses and tools that map analysis workflows and visualization to
generated SQL like Tableau or Apache Superset.

### Object storage and highly dimensional molecular data

UnifyBio stores highly dimensional molecular data in measurement matrices, which are then linked
via identifiers to the primary Datomic databases where relational assay and clinical data are stored.
There are future plans for storing HDF, Zarr, Parquet, and similar formats directly to provide
distributed access to external analysis ecosystems like
[Seurat](https://satijalab.org/seurat/) and
[scverse](https://scverse.org/).

