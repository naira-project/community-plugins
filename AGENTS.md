# AGENTS.md

Instructions for AI coding agents working in `naira-project/community-plugins`.

## Project context

Naira is an open-source AI Engineering Hub for cloud-native teams on Kubernetes. A Go catalog service keeps a graph of AI assets (models, datasets, endpoints, gateways, MCP servers, deployments, owners) and their relations. Plugins collect that data from existing tools. Naira connects tools and shows relationships; it does not replace model registries, gateways, data catalogs, inference runtimes or GitOps tools.

This repository is the plugin index. It holds metadata only: one folder per plugin with a `naira-plugin.yaml` manifest pointing to a digest-pinned OCI image in the owner's registry, plus README, icon and screenshots. Plugin source code never lives here, except for some core maintained ones.

## Layout

- `plugins/<metadata.name>/`: one entry per plugin. `naira-plugin.yaml`, `README.md`, `icon.svg` or `icon.png`, optional `screenshots/`.
- `schema/naira-plugin.v1alpha1.schema.json`: JSON Schema for the manifest. Source of truth for field names and constraints.
- `template/`: starting point for a new entry.
- `README.md`: the human-readable index. Contains the table of official, verified and community plugins.
- `CONTRIBUTING.md`: submission rules. Keep it consistent with the schema.

## Rules for changes

- A pull request touches one plugin folder, plus that plugin's row in the root README table and its CODEOWNERS line. Never mix plugin entries with schema, template or doc changes.
- Folder name equals `metadata.name`. `metadata.name` is reverse-DNS and never changes after merge. Community entries use `io.github.<login-or-org>.<plugin>`.
- `spec.image` is always pinned by `@sha256:` digest. Never write a tag.
- A changed image digest needs a new `metadata.version` (semver).
- New entries use `tier: community`. Do not set `verified` or `official`; that is a maintainer decision.
- Keep `spec.description` to one sentence. It appears in the index and on the website.
- Reuse node and relation kinds from [`pluginapi/schema.go`](https://github.com/naira-project/naira/blob/main/plugins/pkg/pluginapi/schema.go) in `spec.collects`. Kinds never contain `/`.
- `spec.permissions.kubernetes` lists only read verbs (`get`, `list`, `watch`). A plugin never writes to the cluster or to the system it connects to.
- Do not invent manifest fields. If the schema lacks something, propose a schema change in a separate issue.
- Schema changes that break existing manifests need a new `apiVersion` and an RFC or ADR in `naira-project/architecture`.
- Do not add binaries, archives, generated code or plugin source. Icons at most 100 KB, screenshots at most 1 MB each, at most 6 screenshots.

## Validating a manifest

The schema is plain JSON Schema (draft 2020-12). Any validator works, for example:

```bash
# check-jsonschema (pip install check-jsonschema)
check-jsonschema --schemafile schema/naira-plugin.v1alpha1.schema.json plugins/*/naira-plugin.yaml
```

Also check by hand: the folder name equals `metadata.name`, every file referenced in `spec.icon` and `spec.screenshots` exists, and the README table row matches the manifest.

## Reviewing an entry

When asked to review a plugin pull request, check in this order:

1. Fit: the plugin connects an existing tool to the catalog and does not drift into training, notebooks, RAG building, an AI studio or writing back to the tool.
2. Scope: only the allowed files changed.
3. Manifest: validates against the schema, digest-pinned image, name matches folder and namespace matches the author.
4. Safety: declared permissions are minimal and match what the README says; secrets are marked `secret: true`.
5. Docs: README has every template section; icon and screenshots exist and are within limits.
6. Evidence: the pull request shows the plugin running against a Naira version inside `nairaApiVersions`.

Report findings with file and line. Do not approve or merge.

## Writing style

English only. Short sentences, active voice. No marketing adjectives. Use Naira's terms: catalog, node, relation, kind, path, plugin, collect, snapshot, `pluginClaims`, tier.

## Commits and pull requests

- Conventional Commits: `feat(plugins): add <name>`, `fix(plugins): update <name> to <version>`, `docs: ...`, `chore: ...`.
- Every commit is signed off (`git commit -s`, DCO).
- If AI helped with a pull request, say so in the "AI usage" section of the pull request template.
