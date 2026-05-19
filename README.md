# Storywrangler Specification

An open specification for registering, describing, and querying research datasets
across independent pipelines and institutions.

## Motivation

Computational social science produces datasets that share structural patterns —
time-indexed counts, entity-partitioned observations, ranked distributions — yet
each pipeline invents its own conventions for storage layout, entity identification,
and API response shape. This makes cross-dataset analysis brittle: joining Wikipedia
page views with U.S. baby name frequencies requires knowing that both use Wikidata
Q-codes, that one is hive-partitioned by country and the other by state, and that
both return `{types, counts}` pairs.

The Storywrangler Specification formalises these conventions into a shared contract
so that:

- **Pipelines** declare their storage format, entity identifiers, and query axes
  using a standard registration schema.
- **Platforms** validate identifiers, auto-discover partition structure, and serve
  data through uniform API endpoints.
- **Instruments** (visualisations, analyses) consume any conforming dataset without
  per-dataset adapter code.

## What the Specification Covers

### Shared identifier space (Section 3.1--3.5)

A registry of accepted entity identifier systems (Wikidata, ORCID, OpenAlex, ROR,
DOI, ISBN) and field taxonomies (OpenAlex concepts, IPEDS CIP codes), with format
requirements, validation rules, and resolution URLs. Any dataset that tags its
entities with a recognised namespace can be joined with any other dataset in the
same namespace — without ad-hoc crosswalks.

### API endpoint schemas (Section 3.6)

Standard response contracts for instruments. Currently two types:

| Type | Shape | Use case |
|---|---|---|
| `types-counts` | `{types: [...], counts: [...]}` | Rank distributions (allotaxonograph, word shift) |
| `time-series` | `[{dim: v, ..., count: n}]` | Temporal trends, panel data |

### Dataset registration schema (Section 3.7)

The full metadata payload a pipeline submits to register its data: storage format,
query slice axes, entity mapping, hive partition structure, hash bucket routing,
ownership, lineage, versioning, and manifest (coverage index). The platform
auto-derives what it can (partition levels, availability ranges, bucket counts)
and validates the rest against the specification.

## Versioning

Versions follow [Semantic Versioning](https://semver.org/). The current release is
**v0.0.3**.

| Version | Date | Summary |
|---|---|---|
| 0.0.3 | 2025-05 | Dataset registration schema, `time-series` endpoint, `hash_algorithm`/`hash_seed` |
| 0.0.1 | 2024-11 | Entity identifiers, field taxonomies, validation rules |

Specification documents live in [`versions/`](versions/).

## Implementations

| Project | Role |
|---|---|
| [storywrangler](https://pypi.org/project/storywrangler/) | Python SDK — registration client, entity validation, project scaffolding |
| [storywrangler-schemas](https://pypi.org/project/storywrangler-schemas/) | Shared Pydantic models and hash bucket assignment |
| [storywrangler (backend)](https://github.com/vermont-complex-systems/storywrangler) | FastAPI registry, query layer, and routers |
| [Complex Stories](https://complexstories.uvm.edu) | SvelteKit frontend consuming the API |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) and [GOVERNANCE.md](GOVERNANCE.md).

New identifier systems and endpoint types follow the extension process in Section 4
of the specification. Changes require a proposal, review period, and implementation
in the SDK before inclusion in the next version.

## License

This specification is licensed under the [MIT License](LICENSE).
