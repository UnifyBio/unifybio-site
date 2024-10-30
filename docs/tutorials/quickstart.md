# Quickstart

This section contains step by step instructions to import the
[template dataset]. Running these steps will help you make sure that everything is setup correctly on your system and also provide an overall view of the process.

This quickstart assumpes you have a local UnifyBio distribution downloaded (such as 
[the UnifyBio package](TODO)
provided by RCRF's Pattern Data Commons initiative),
and that you have [installed Docker](https://docs.docker.com/)
and can stand up a local system with Docker compose. You will also need a JVM
(version 17 or later recommended). One may already be on your machine, which you can check by running:

```
java -version
```

in a terminal. If you do not have a JVM installed,  [Temurin 17](https://adoptium.net/installation/)
is a good default.

Download the latest version of the template-dataset
[here](https://github.com/vendekagon-labs/unify/tree/main/test/resources/systems/candel/template-dataset) and unzip to a directory you will
remember, and with a path reachable for command line tools.

Download and unzip your UnifyBio distribution. If you have not been provided one, you can use
the [Pattern distribution](https://github.com/RCRF/data-commons-workshop-env).

- Navigate to the directory created when you unzip or clone the UnifyBio distribution.
- Start a local system with:

    ```
    bin/start-local-system
    ```

All subsequent commands assume you are in the UnifyBio distribution directory and have a local system
running with Docker compose.

## Simple Import Workflow

This shows an example of working with a minimal import, using the Unify CLI.

The first thing you will do is transform data from tables into entity map representations in _edn_ form,
by using the Unify CLI prepare task.

  ```bin/unify prepare --import-config $PATH-TO-TEMPLATE-DATASET/config.yml --working-directory PATH-TO-TEMP-DIRECTORY```

When prepare has completed successfully, you are now able to transact the data into a database.
To do so, first request a database from the local UnifyBio system:

- **Request a new database**. This will create a new Datomic database in your local system.

  ```bin/unify request-db --database YOUR-DB-NAME"```

Then transact it:

- **Transact** data in the database with this command. Note that `YOUR-DB-NAME` is the same db-name you created in the request step.

  ```bin/unify transact --database YOUR-DB-NAME --working-directory PATH-TO-TEMP-DIRECTORY```

After the data has been transcated, you can validate it, which ensures that entities conform to spec,
are structurally consistent, and referentially consistent (all targets of entity reference
attributes refer to other entities that actually exist).

- **Validate** your import with this command

  ```bin/validate --database YOUR-DB-NAME --dataset "template-dataset"```

## Query Your Data

You can now run queries against the local database! If you are using the Pattern Data Commons distribution and schema,
you can install the `patternq` [Python](https://github.com/rcrf/patternq)
or [R](https://github.com/CANDELbio/wick) libraries.

### Query TODO:

- simple Python script in Pattern ditributio
- how to configure PatternQ Python to talk to local system
- example library call that enumerates some data
