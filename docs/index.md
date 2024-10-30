# About UnifyBio

UnifyBio is a set of power tools for data-driven data harmoniozation, ETL, analysis,
and visualization, designed for use by informaticians in the translational sciences.
UnifyBio is aimed at building and co-locating high quality clinical and molecular datasets
by enabling even small teams to extract data out of ad hoc tables, schemas, and siloes, and
into unified representations with a simple, declarative workflow.
UnifyBio's distributed data formats store highly dimensional and relationally intertwined datasets efficiently
in distributed systems, making them available for full query, semantically rich exploration,
and efficient processing, whether by humans or automated algorithms.
UnifyBio tools enable all of this without sacrificing either flexibility or reproducibility,
using schema inference rather than hard-coded logic whenever feasible, and
preserving granular and simple to inspect time provenance for all datasets and system interactions.

## Getting Started with Unify

The steps of the most straightforward UnifyBio workflow are:

- preprocess a dataset for import
- write an import config for your dataset
- use the unify cli to import the dataset into a UnifyBio local system
- query the dataset via Python or R libraries.

The easiest way to get started using UnifyBio with this workflow is to stand up a
[local dev system](local-system.md), and work through the [quickstart](tutorials/quickstart.md).
Depending on your learning preferences, you may want to either read the [Concepts](concepts.md) documentation before starting and/or
refer to it as you go.

## Support

UnifyBio builds off of the core tooling of the CANDEL platform, generously open sourced by the
[Parker Institute for Cancer Immunotherapy](https://www.parkerici.org/).
As of Fall 2024, development has been supported by the
[Rare Cancer Research Foundation](https://rarecancer.org/) and
[Clojurists Together](https://www.clojuriststogether.org/projects/).

