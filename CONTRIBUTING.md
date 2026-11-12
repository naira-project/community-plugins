# Contributing

Thanks for extending Naira. This repository is the plugin index: you add a folder that describes your plugin and points to its image. Your code, releases and support stay in your own repository.

## Code of conduct

All members of the project community must abide by the [SAP Open Source Code of Conduct](https://github.com/SAP/.github/blob/main/CODE_OF_CONDUCT.md). Report unacceptable behavior to a project maintainer.

## What belongs here

A plugin fits the index when it connects an existing tool to the Naira catalog and helps people see AI assets, their relationships and their owners. Examples: AI gateways, inference runtimes, model registries, data catalogs, vector stores, MCP servers, GitOps controllers, observability backends, cloud AI services.

Not sure? Open an issue first and describe the tool and what the plugin would show in Naira.

## Before you open a pull request

Your plugin must already exist and work:

- The source is in a public repository with an OSI-approved license.
- The image implements the `plugin.v1.CatalogPluginService` gRPC contract from [naira/plugins/pkg/pluginapi](https://github.com/naira-project/naira/tree/main/plugins/pkg/pluginapi). Read [plugin authoring](https://github.com/naira-project/naira/blob/main/docs/plugin-authoring.md) and the plugin design principles in [naira/plugins/AGENTS.md](https://github.com/naira-project/naira/blob/main/plugins/AGENTS.md).
- The image is published to a public OCI registry and pullable by digest.
- The image runs as non-root (Naira runs plugins as uid 65532 with all capabilities dropped).
- The plugin reads only. It never changes the system it connects to.
- The plugin emits stable, unique `NodeID`s and reuses existing node and relation kinds from [`pluginapi/schema.go`](https://github.com/naira-project/naira/blob/main/plugins/pkg/pluginapi/schema.go) where they fit.
- You ran it against a real Naira installation inside the range you declare in `nairaApiVersions`.

A conformance kit that checks most of this automatically is planned. Until it exists, reviewers check the points above by hand and rely on your evidence.

## Add your plugin

1. Fork this repository and create a branch.
2. Copy [`template/`](template/) to `plugins/<metadata.name>/`.
3. Choose `metadata.name`. Community plugins use `io.github.<login-or-org>.<plugin>`, for example `io.github.jdoe.qdrant`. The login or org must be yours: you are a listed maintainer or a member of that org. The folder name equals `metadata.name`.
4. Fill in `naira-plugin.yaml`. It must validate against [`schema/naira-plugin.v1alpha1.schema.json`](schema/naira-plugin.v1alpha1.schema.json). Set `tier: community`.
5. Write `README.md` from the template. Keep every section: what it collects, configuration, permissions, compatibility, known limitations, support.
6. Add an icon (`icon.svg`, or `icon.png` at least 256x256 px, at most 100 KB) and up to 6 screenshots (PNG, JPG or WebP, at most 1 MB each) that show your plugin's data in Naira.
7. Add a CODEOWNERS line for your folder: `/plugins/<metadata.name>/ @<your-login>`.
8. Add a row for your plugin to the "Verified and community" table in the root [README.md](README.md), sorted by name.
9. Open a pull request titled `feat(plugins): add <metadata.name>`.

### Manifest fields

| Field | Required | Notes |
|---|---|---|
| `metadata.name` | yes | Reverse-DNS, equals the folder name, never changes after merge. |
| `metadata.version` | yes | Semver of the release the image digest belongs to. |
| `spec.displayName` | yes | At most 40 characters. |
| `spec.description` | yes | One sentence, at most 200 characters. |
| `spec.tier` | yes | `community` for new entries. Maintainers change it. |
| `spec.categories` | yes | 1 to 3 values from the schema list. Propose a new category in an issue. |
| `spec.image` | yes | `registry/repo@sha256:<digest>`. Tags are rejected. |
| `spec.nairaApiVersions` | yes | Semver range of Naira releases, for example `">=0.1.2 <0.2.0"`. |
| `spec.source` | yes | Public source repository. |
| `spec.license` | yes | SPDX identifier. |
| `spec.maintainers` | yes | Name and GitHub login. At least one. |
| `spec.icon` | yes | File in your folder. |
| `spec.homepage` | no | Docs or product page. |
| `spec.screenshots` | no | Path under `screenshots/` and a caption. |
| `spec.collects` | no | Node and relation kinds you write. Strongly recommended. |
| `spec.configuration.env` | no | Every environment variable, marked `required` and `secret` where it applies. |
| `spec.permissions` | no | Kubernetes ClusterRole rules (read verbs only) and the external systems you call. Required when the plugin reads the cluster. |

## Pull request rules

- One pull request changes one plugin folder. The root README table and CODEOWNERS lines for that plugin are the only allowed changes outside it.
- A new image digest needs a new `metadata.version`.
- `metadata.name` never changes. To rename, add a new entry and ask for the old one to be deprecated.
- Every commit carries a DCO sign-off (`git commit -s`). You accept the [Developer Certificate of Origin](https://developercertificate.org/) on your first pull request.
- Pull request titles follow [Conventional Commits](https://www.conventionalcommits.org/): `feat(plugins): add <name>`, `fix(plugins): update <name> to <version>`, `docs: ...`, `chore: ...`.

## Review

A maintainer reviews new entries for fit, manifest validity, declared permissions and the evidence in your pull request. Updates to an existing entry need the approval of a CODEOWNER of that folder and are merged by a Naira maintainer.

Listing a community plugin is not an endorsement. The Naira project does not build, scan or support community plugin images.

## Keeping your entry alive

- Update `metadata.version`, `spec.image` and `spec.nairaApiVersions` when you release.
- Answer issues in your own repository.
- When the image cannot be pulled or the entry no longer supports current Naira releases, a time-boxed policy will marking entries unmaintained and remove them. Until that policy exists, maintainers raise such cases in an issue on the entry.

## Verified tier

The verified tier needs proof of namespace ownership (GitHub org or DNS) and is meant for vendor-maintained plugins. The process is not defined yet. If you want to be verified, open an issue so we can design it with you.

## Changes to this repository itself

Changes to the schema, template, CI or docs start with an issue. Schema changes that break existing manifests need a new `apiVersion` and an RFC or ADR in [naira-project/architecture](https://github.com/naira-project/architecture).

## Licensing

Content you add to this repository (manifest, README, icon, screenshots) is licensed under [Apache 2.0](LICENSE). Only add images and logos you have the right to publish. Your plugin source and image keep the license you declare in `spec.license`.
