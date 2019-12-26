# Introduction

This repo builds `appthreat/sast-scan`, a container image with a number of bundled opensource static analysis tools. RedHat's `ubi8/ubi-minimal` is used as a base image instead of the usual alpine to help with enterprise adoption of this tool.

## Bundled tools

| Programming Language | Tools |
|----------------------|-------|
| ansible | ansible-lint |
| aws | cfn-lint, cfn_nag |
| bash | shellcheck |
| Credential scanning | gitleaks |
| golang | gosec, staticcheck |
| java | gradle, pmd, dependency-check |
| json | jq, jsondiff, jsonschema |
| kotlin | detekt |
| kubernetes | kube-score |
| node.js | cyclonedx-bom, retire, eslint, yarn |
| puppet | puppet-lint |
| python | bandit, cyclonedx-py, ossaudit, pipenv |
| ruby | brakeman, cyclonedx-ruby |
| rust | cargo-audit |
| terraform | tfsec |
| yaml | yamllint |

## Bundled languages/runtime

- jq
- Python 3.6
- OpenJDK 11 (jre)
- Ruby 2.5.5
- Rust
- Node.js 10
- Yarnpkg
- Remic

## Usage

### Invoking built-in tools

Bandit
```bash
docker run --rm -v <source path>:/app appthreat/sast-scan bandit -r /app
```

dependency-check
```bash
docker run --rm --tmpfs /tmp -v <source path>:/app appthreat/sast-scan /opt/dependency-check/bin/dependency-check.sh -s /app
```

Retire.js
```bash
docker run --rm --tmpfs /tmp -v <source path>:/app appthreat/sast-scan retire -p --path /app
```

### Using custom scan script

Scan python project
```bash
docker run --rm --tmpfs /tmp -v <source path>:/app appthreat/sast-scan python3 /usr/local/src/scan.py --src /app --type python --out_dir /app
```

Scan node.js project
```bash
docker run --rm --tmpfs /tmp -v <source path>:/app appthreat/sast-scan python3 /usr/local/src/scan.py --src /app --type nodejs --out_dir /app
```

Scan java project
```bash
docker run --rm --tmpfs /tmp -v ~/.m2:/.m2 -v <source path>:/app appthreat/sast-scan python3 /usr/local/src/scan.py --src /app --type java --out_dir /app
```

## Alternatives

GitLab [SAST](https://docs.gitlab.com/ee/user/application_security/sast/) uses numerous single purpose [analyzers](https://gitlab.com/gitlab-org/security-products/analyzers) and GoLang based convertors to produce a custom json format. This model has the downside of increasing build times since multiple container images should get downloaded and hence is not suitable for CI environments such as Azure Pipelines, CodeBuild and Google CloudBuild. Plus the license used by GitLab is not opensource even though the analyzers merely wrap existing oss tools!

