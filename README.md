# EKS token action

> Action to get an access token from EKS

## Table of Contents <!-- omit in toc -->

- [Purpose](#purpose)
- [Requirements](#requirements)
- [Inputs](#inputs)
- [Outputs](#outputs)
- [Usage](#usage)
- [Maintainers](#maintainers)
- [Contributing](#contributing)
- [License](#license)

## Purpose

You can use this action to fetch a token from Amazon EKS to use for `kubectl` or `helm` commands.

## Requirements

The action requires that a valid AWS access key is configured in the environment. You can do this, for example, with [aws-actions/configure-aws-credentials@v1](https://github.com/aws-actions/configure-aws-credentials).

## Inputs

The action has two possible inputs:

* `cluster-name` (required): the cluster name to get a token for. This must be the local name of a cluster within your AWS account.
* `role-arn` (optional): the role to assume when logging in.

## Outputs

* `token`: the bearer token you can use to communicate with your cluster. Note that this is only valid for a few minutes.
* `raw`: the raw output of `aws eks get-token`. This is a JSON structure that includes extra metadata.

Both outputs are masked in your logging.

## Usage

In this example we log in with AWS, get a token, and then pass the token to an input in a next step:

```yaml
- name: Configure AWS Credentials
  uses: aws-actions/configure-aws-credentials@v1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: eu-west-1

- name: Get EKS token
  uses: elseu/sdu-eks-token-action@1.0.0
  id: get-token
  with:
    cluster-name: "messier-15"

- name: Deploy with Helm
  uses: "...your own step..."
  with:
    token: "${{ steps.get-token.outputs.token }}"
```

Or you can use the token as an environment variable:

```yaml
- name: Configure AWS Credentials
  uses: aws-actions/configure-aws-credentials@v1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: eu-west-1

- name: Get EKS token
  uses: elseu/sdu-eks-token-action@1.0.0
  id: get-token
  with:
    cluster-name: "messier-15"

- name: Deploy with Helm
  uses: "...your own step..."
  env:
    KUBE_TOKEN: "${{ steps.get-token.outputs.token }}"
```

## Maintainers

- [Sebastiaan Besselsen](https://github.com/sbesselsen) (Sdu)

## Contributing

Please create a branch named `feature/X` or `bugfix/X` from `master`. When you are done, send a PR to Sebastiaan Besselsen.

## License

MIT License

Copyright (c) 2021 Sdu Uitgevers BV

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

© 2021 Sdu Uitgevers BV
