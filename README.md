# plenoai/aqua-registry

[aqua](https://aquaproj.github.io/) registry for plenoai tools. Packages verify
`checksums.txt` with the release's cosign bundle (keyless, GitHub OIDC) and the
archive's GitHub artifact attestation before installing.

## Usage

```yaml
# aqua.yaml
checksum:
  enabled: true
  require_checksum: true
registries:
  - name: plenoai
    type: github_content
    repo_owner: plenoai
    repo_name: aqua-registry
    ref: main # pin a commit SHA for reproducibility
    path: registry.yaml
packages:
  - name: plenoai/pleno-dlp@v0.70.2
    registry: plenoai
```

aqua requires non-standard registries to be allowed by a policy file:

```yaml
# aqua-policy.yaml
registries:
  - name: plenoai
    type: github_content
    repo_owner: plenoai
    repo_name: aqua-registry
    ref: semver(">= 0.0.0") # or a specific ref
    path: registry.yaml
packages:
  - registry: plenoai
```

Then `aqua policy allow aqua-policy.yaml` and `aqua i`.

## Layout

- `pkgs/<owner>/<repo>/registry.yaml`: per-package definition (source of truth)
- `registry.yaml`: merged registry consumed by aqua

Packages: `plenoai/pleno-dlp`.
