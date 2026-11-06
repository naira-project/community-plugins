# Qdrant

<!-- Replace every section. Keep it short: this page is what users read before they install. -->

Collects Qdrant collections and links them to the models that write embeddings into them.

## What it collects

| Node kind | Path | Properties |
|---|---|---|
| `dataset` | `<PATH_PREFIX>/<collection>` | `vectors_count`, `distance` |

| Relation kind | From | To |
|---|---|---|
| `derived_from` | `dataset` | `model` |

## Configuration

| Variable | Required | Secret | Description |
|---|---|---|---|
| `QDRANT_URL` | yes | no | Base URL of the Qdrant HTTP API. |
| `QDRANT_API_KEY` | no | yes | Read-only API key. |
| `PATH_PREFIX` | no | no | Prefix for node paths. Default `qdrant`. |

## Permissions

- Kubernetes: none.
- Network: reads the Qdrant HTTP API. Never writes.

## Compatibility

Tested with Naira 0.1.2 and Qdrant 1.12.

## Known limitations

- Collections without payload metadata are not linked to a model.

## Support

Issues: https://github.com/jdoe/naira-plugin-qdrant/issues
