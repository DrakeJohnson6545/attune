# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in Attune, please report it
responsibly.

**DO NOT** create a public GitHub issue for security vulnerabilities.

Instead, please report vulnerabilities via GitHub's private security advisory
feature: [Report a vulnerability](https://github.com/attune-io/attune/security/advisories/new).

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest release | Yes |
| Previous release | Security fixes only |
| Older | No |

## Security Practices

### Supply Chain

- Container images and binaries are signed with
  [cosign](https://github.com/sigstore/cosign) using keyless signing
  (Sigstore OIDC)
- [SLSA](https://slsa.dev) build provenance is generated for binary
  artifacts and container images using GitHub's native attestations
- SBOMs are generated for every release (SPDX format) and attached to
  the GitHub release
- [FOSSA](https://fossa.com) license compliance scanning runs on pull
  requests and main pushes that change Go sources, `go.mod`, `go.sum`,
  the filter script, or the License Compliance workflow. Manual
  dispatch is also available.

### Vulnerability Scanning

- [Trivy](https://trivy.dev) filesystem and image scans run on every
  pull request, on the merge queue, on manual dispatch, and every
  Monday at 06:00 UTC. The release workflow scans the published image
  again.
- [govulncheck](https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck)
  runs on every pull request as a CI job, and again in the security
  workflow on those same security triggers.
- [Gitleaks](https://gitleaks.io) secret detection runs in the security
  workflow on those triggers. A push to main does not run it.
- [CodeQL](https://codeql.github.com) for Go and GitHub Actions runs in
  the security workflow on those triggers. Release-please pull requests
  skip the analysis and report a stand-in.
- [GitHub secret scanning](https://docs.github.com/en/code-security/secret-scanning)
  is enabled at the organization level

### CI Hardening

- [StepSecurity harden-runner](https://github.com/step-security/harden-runner)
  is enabled on all CI workflows
- [OpenSSF Scorecard](https://securityscorecards.dev) monitors the
  repository's security posture
- [Dependabot](https://docs.github.com/en/code-security/dependabot)
  monitors Go modules, GitHub Actions, and Docker base images weekly
- Static analysis via golangci-lint (50+ linters) runs in CI on pull
  requests. Product CI does not run on a push to main.

### Tenancy

What an `AttunePolicy` author can make the operator do is documented in
[Tenancy](docs/security/tenancy.md).

### Runtime

- The operator runs as non-root (`runAsUser: 65532`) with a read-only
  root filesystem
- All Linux capabilities are dropped (`drop: ALL`); privilege escalation
  is disabled
- RBAC permissions follow least-privilege principles
