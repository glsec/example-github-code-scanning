# glsec example: GitHub code scanning

[![CI/CD security: glsec](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/glsec/glsec/main/assets/badge/v1.json)](https://github.com/glsec/glsec)

[glsec](https://github.com/glsec/glsec) scans GitLab CI/CD pipelines. Its SARIF output (`--format sarif`) works with GitHub code scanning, for a GitLab pipeline file that lives in a GitHub repository, or a mirror of a GitLab project. Each finding becomes a code scanning alert, and on a pull request it shows up as an annotation on the changed line. Each rule links to its page on [glsec.github.io](https://glsec.github.io/rules/).

The demo pull request [#1](../../pull/1) changes `bad-pipeline.yml` from a clean pipeline into one with the kind of mistakes glsec reports. It stays open on purpose.

## The workflow

[`.github/workflows/glsec.yml`](.github/workflows/glsec.yml):

1. downloads the glsec release and checks it against `checksums.txt` and against the provenance of glsec's release workflow (`gh attestation verify`)
2. runs `glsec --format sarif bad-pipeline.yml > glsec.sarif`. glsec exits 1 when it finds something, so only exit code 2 (a file it can't parse) fails the job
3. uploads `glsec.sarif` with `github/codeql-action/upload-sarif`, with `security-events: write` and nothing else

For your own repository, change the file name in the scan step, or pass several files or a glob.

## See also

- [Example reports](https://glsec.github.io/reports/): what every output format looks like for the same file
- GitLab examples: [Catalog component](https://gitlab.com/glsec-io/examples/component), [Code Quality](https://gitlab.com/glsec-io/examples/component-code-quality), [Docker image](https://gitlab.com/glsec-io/examples/docker), [binary](https://gitlab.com/glsec-io/examples/binary)
