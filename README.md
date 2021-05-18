# Overview

This is a companion repository used in the Shift-Left Security Workshop operated by Google Cloud.

## Requirements

The below requirements are for running the lab. If using the "Cloud Shell", all of these tools are automatically installed and at-or-above the version required.

* Golang 1.15+

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

