## Description

<!-- What does this PR add or change? Link the related issue, if any. -->

## Type of change

- [ ] 🧩 New plugin entry
- [ ] ⬆️ Update of an existing plugin entry (new version, digest or metadata)
- [ ] 📝 Documentation
- [ ] 📐 Schema or template
- [ ] 🏗️ CI / repository setup

## Plugin entry checklist

<!-- Only for new or updated plugin entries. See CONTRIBUTING.md. -->

- [ ] This PR changes one plugin folder, plus its README table row and CODEOWNERS line
- [ ] The folder name equals `metadata.name`, and the namespace is mine (`io.github.<my-login-or-org>`)
- [ ] `naira-plugin.yaml` validates against `schema/naira-plugin.v1alpha1.schema.json`
- [ ] `spec.image` is pinned by digest and publicly pullable
- [ ] `metadata.version` is bumped if the digest changed
- [ ] The image runs as non-root and only reads from the systems it connects to
- [ ] Every environment variable and permission is declared in the manifest and README
- [ ] Icon and screenshots are mine to publish and within the size limits
- [ ] All commits are signed off (DCO)

## Evidence

<!-- Show the plugin running: Naira version, plugin version, and a screenshot or
     API output (for example GET /v1/nodes) with the nodes and relations it wrote. -->

<!-- Uncomment when AI was used for this PR
## AI usage

Describe how and for what AI was used in this PR.
-->
