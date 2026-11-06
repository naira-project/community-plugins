# Naira Community Plugins

The plugin index for [Naira](https://github.com/naira-project/naira), the open-source AI Engineering Hub for AI and cloud-native teams.

Each plugin is a container image that serves the Naira plugin gRPC contract and writes nodes and relations into the catalog graph.

This repository does not hold plugin code, except for core maintainer developed plugins. Each plugin lives in its owner's repository and registry. This repository holds one folder per plugin with a manifest (`naira-plugin.yaml`) that points to a digest-pinned image, plus an icon, screenshots and a short description. The Naira website catalog is generated from these folders.

> **Alpha.** The manifest schema is `naira.dev/v1alpha1` and will change while the plugin contract evolves. Pin the Naira version you test against in `nairaApiVersions`.

## Plugin index

### Official

Built and maintained by the Naira maintainers in [naira-project/naira](https://github.com/naira-project/naira/tree/main/plugins/cmd). They ship with the Naira Helm chart.

| Plugin | Connects |
|---|---|
| [bedrock](https://github.com/naira-project/naira/tree/main/plugins/cmd/bedrock) | AWS Bedrock foundation models and inference endpoints |
| [depl_calls_svc](https://github.com/naira-project/naira/tree/main/plugins/cmd/depl_calls_svc) | Kubernetes Deployments to the Services they call |
| [depl_uses_litellm](https://github.com/naira-project/naira/tree/main/plugins/cmd/depl_uses_litellm) | Kubernetes Deployments to the models they reach through LiteLLM keys |
| [depl_uses_thalamus](https://github.com/naira-project/naira/tree/main/plugins/cmd/depl_uses_thalamus) | Kubernetes Deployments to the models they consume through Thalamus |
| [fluxcd](https://github.com/naira-project/naira/tree/main/plugins/cmd/fluxcd) | Flux sources, Kustomizations and Helm charts |
| [github](https://github.com/naira-project/naira/tree/main/plugins/cmd/github) | Deployments to their source repositories, with CODEOWNERS ownership |
| [litellm](https://github.com/naira-project/naira/tree/main/plugins/cmd/litellm) | LiteLLM models and inference endpoints |
| [mcp_servers](https://github.com/naira-project/naira/tree/main/plugins/cmd/mcp_servers) | MCP servers and their tools |
| [mlflow](https://github.com/naira-project/naira/tree/main/plugins/cmd/mlflow) | MLflow registered models |
| [openai_api_models](https://github.com/naira-project/naira/tree/main/plugins/cmd/openai_api_models) | Models from any OpenAI-compatible API |
| [openmetadata](https://github.com/naira-project/naira/tree/main/plugins/cmd/openmetadata) | OpenMetadata datasets |
| [tech_radar](https://github.com/naira-project/naira/tree/main/plugins/cmd/tech_radar) | An admin-managed technology radar |
| [thalamus](https://github.com/naira-project/naira/tree/main/plugins/cmd/thalamus) | Models served by Thalamus |

### Verified and community

Listed from the folders in [`plugins/`](plugins/).

| Plugin | Tier | Category | Maintainers | Description |
|---|---|---|---|---|
| _No plugins yet. [Add the first one.](CONTRIBUTING.md)_ | | | | |

Tiers:

- **Official**: built and maintained by the Naira maintainers.
- **Verified**: maintained by a vendor or partner whose namespace ownership is proven. The verification process is not defined yet.
- **Community**: maintained by the contributor, best effort, no guarantees from the Naira project. Review the declared permissions and the source before you install.

## Repository layout

```
plugins/
  io.github.<owner>.<plugin>/   one folder per plugin, named after metadata.name
    naira-plugin.yaml           manifest (required)
    README.md                   what it collects, configuration, permissions (required)
    icon.svg | icon.png         icon (required)
    screenshots/                up to 6 screenshots (recommended)
schema/
  naira-plugin.v1alpha1.schema.json   JSON Schema for the manifest
template/                       copy this folder to start a new entry
```

## Using a plugin

1. Pick a plugin from the index and read its `README.md`, especially the declared permissions and configuration.
2. Check that your Naira version is inside the plugin's `nairaApiVersions` range.
3. Add the plugin as a sidecar entry under `catalog.plugins` in the Naira Helm chart values, with its image, port, schedule and environment.

**Known gap:** the Naira Helm chart currently prefixes every plugin image with one global registry (`image.registry`) and accepts tags, not digests. Community plugin images hosted in other registries and pinned by digest cannot be configured without a chart change yet.

## Contributing a plugin

Read [CONTRIBUTING.md](CONTRIBUTING.md). In short: build and publish your plugin in your own repository, copy [`template/`](template/) to `plugins/<your-plugin-name>/`, fill in the manifest and README, and open a pull request that touches only that folder.

To propose a plugin you would like someone to build, open an issue.

## Support, feedback, contributing

Questions, ideas and bug reports for this index go to [GitHub issues](https://github.com/naira-project/community-plugins/issues). Problems with a specific community plugin go to that plugin's own repository, linked in its manifest.

## Security / disclosure

If you find a security problem in this repository or in a listed plugin, follow the [security policy](https://github.com/naira-project/.github/blob/main/SECURITY.md).

## Code of conduct

Everyone in the project community follows the [Code of Conduct](https://github.com/SAP/.github/blob/main/CODE_OF_CONDUCT.md).

## Licensing

The content of this repository is licensed under [Apache 2.0](LICENSE). Each plugin's source and image are licensed by their owners as declared in `spec.license`.

<p align="center"><img alt="Bundesministerium für Wirtschaft und Energie (BMWE)-EU funding logo" src="https://apeirora.eu/assets/img/BMWK-EU.png" width="400"/></p>
