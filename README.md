# Overview

This is a companion repository used in the Shift-Left Security Workshop operated by Google Cloud.

In this lab, you will examine source code that has some basic vulnerabilities that exercise the tools that demonstrate better source-code security proceesures, techniques and processes.

> NOTE: There are several tools that accomplish all of these tasks associated with source code security. These tools were chosen to emphasize the intended toolset area, in real-world, choose the toolset that is most comprehensive and solves as many vulnerablity areas as possible.

## Requirements

The below requirements are for running the lab. If using the "Cloud Shell", all of these tools are automatically installed and at-or-above the version required.
* Golang 1.15+

## Activities

### Step 1

1. Linting - Linting is used to check common style-based mistakes or defects related to syntax. Linting assists security by
providing a common syntax pattern across multiple teams that leads to faster code reviews, knowlede sharing and clarity of code.
Additionally, Linting identifies common syntax mistakes that can lead to common vulnerabilities such as improper or less efficient
use of libraries or core APIs.

1. Install the `staticcheck` linking tool

    ```bash
    go get honnef.co/go/tools/cmd/staticcheck@latest
    ```

1. Run `staticcheck` in the project root directory

    > NOTE: You will get an error `unnecessary use of fmt.Sprintf (S1039)` because `http.ListenAndServe()` accepts a String, and the current code uses `Sprintf` without passing variables to the string

    1. Print out the results of the command.  This is one method that can be used in a CI/CD pipeline to determine success/failure of the tool

    ```bash
    echo $?
    # Should see some number greater than 1
    ```

1. Fix the code by commenting out the line below `LINTING - Step 1` inside the `main()` method

1. Uncomment the two lines directly below the `LINTING-step 2` inside the `main()` method

1. Re-run `staticcheck` in the project root directory

    > NOTE: No error should exist (empty line)

    1. Print out the results of the command. This time, the result should be `0`

    ```bash
    echo $?
    # Should be zero
    ```

### Step 2

1. AST/Static security testing - Provides static code analysis looking for common weaknesses and exposures (CWEs)

1. Install the AST tool (`gosec`)

    ```bash
    export GOSEC_VERSION="2.7.0"
    curl -sfL https://raw.githubusercontent.com/securego/gosec/master/install.sh | sh -s -- -b $HOME/gopath/bin v${GOSEC_VERSION}
    ```

1. Run `gosec` with policy file against the source code

    ```bash
    gosec -conf policies/gosec-policy.json -fmt=json ./...

    ```

    > Output should be similar to thisf
    ```json
    {
        "Golang errors": {},
        "Issues": [
                {
                        "severity": "HIGH",
                        "confidence": "LOW",
                        "cwe": {
                                "ID": "798",
                                "URL": "https://cwe.mitre.org/data/definitions/798.html"
                        },
                        "rule_id": "G101",
                        "details": "Potential hardcoded credentials",
                        "file": "/home/random-user-here/shift-left-security-workshop/labs/source-code-lab/main.go",
                        "code": "31: \t// STEP 2: Change this and the reference below to something different (ie, not \"pawsword\" or \"password\")\n32: \tvar pawsword = \"im-a-cute-puppy\"\n33: \tfmt.Println(\"Something a puppy would use: \", username, pawsword)\n",
                        "line": "32",
                        "column": "6"
                }
        ],
        "Stats": {
                "files": 1,
                "lines": 89,
                "nosec": 0,
                "found": 1
        }
    }
    ```

## Step 3

1. Licenses are important to security becasue they can legally require you to expose source code that you may not want to expose.  The concept is called "copyleft" licenses that require you to expose source code if you use dependencies with those licenses.

1. Install `golicense`

```bash
mkdir -p /tmp/golicense
wget -O /tmp/golicense/golicense.tar.gz https://github.com/mitchellh/golicense/releases/download/v0.2.0/golicense_0.2.0_linux_x86_64.tar.gz
pushd /tmp/golicense
tar -xzf golicense.tar.gz
chmod +x golicense
mv golicense $HOME/gopath/bin/golicense
popd
```

1. Build the binary file

    ```bash
    go build
    ```

1. Run the license check with the current policy file that does not allow "BSD-3-Clause" licenses

    ```bash
    golicense policies/license-policy.hcl hello-world
    ```

    > NOTE: This should fail with similar output:

    ```bash
    🚫 rsc.io/sampler    BSD 3-Clause "New" or "Revised" License
    🚫 rsc.io/quote      BSD 3-Clause "New" or "Revised" License
    🚫 golang.org/x/text BSD 3-Clause "New" or "Revised" License
    ```

1. Modify the policy file (policies/license-policy.hcl) to move the "BSD-3-Clause" to the allowed list

1. Re-run the license check

    ```bash
    golicense policies/license-policy.hcl hello-world
    ```

    > NOTE: This should succeed with similar output:

    ```bash
    ✅ rsc.io/quote      BSD 3-Clause "New" or "Revised" License
    ✅ rsc.io/sampler    BSD 3-Clause "New" or "Revised" License
    ✅ golang.org/x/text BSD 3-Clause "New" or "Revised" License
    ```

## Completed

For production use, these commands (and other tools targeting source code) should be incorporated into a CI/CD pipeline.
