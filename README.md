# Overview

This is a companion repository used in the Shift-Left Security Workshop operated by Google Cloud.

## Requirements

The below requirements are for running the lab. If using the "Cloud Shell", all of these tools are automatically installed and at-or-above the version required.

* Golang 1.15+

## Activities

1. Linting - Linting is used to check common style-based mistakes or defects related to syntax. Linting assists security by
providing a common syntax pattern across multiple teams that leads to faster code reviews, knowlede sharing and clarity of code.
Additionally, Linting identifies common syntax mistakes that can lead to common vulnerabilities such as improper or less efficient
use of libraries or core APIs.

1. AST/Static security testing -




### AST Tool (`gosec`)

```bash
export GOSEC_VERSION="2.7.0"
curl -sfL https://raw.githubusercontent.com/securego/gosec/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v${GOSEC_VERSION}
```

## Go Lint (advanced lint)

Linting identifies common syntax mistakes and bad practices.

```bash
go get honnef.co/go/tools/cmd/staticcheck@latest
```

## GitHound

Githound looks for secrets in the source code and is configurable to use a policy file.

```bash
go get github.com/ezekg/git-hound
```

## Golicense

```bash
wget https://github.com/mitchellh/golicense/releases/download/v0.2.0/golicense_0.2.0_linux_x86_64.tar.gz
tar -xzf golicense_0.2.0_linux_x86_64.tar.gz
mv golicense /usr/local/bin/golicense
```

