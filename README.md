## About

## Repository

### Setup

`hajlesilesia/toolbox` Docker image is a preferred way to distribute the tools used in this organization. It's designed to bring consistency for local and [remote](.github/workflows) usage by being cross-platform (macOS, Linux, WSL), multi architecture (linux/amd64, linux/arm64), version controlled and reusable.

Run once:

```shell
docker run --rm hajlesilesia/toolbox:latest init | bash
```

Run every time new version was released and updated in the workflows files (for consistency with remote workflows):

```shell
docker pull hajlesilesia/toolbox:latest
```

Run on daily basis, preferably as a main terminal:

```shell
toolbox
```

Before cloning any repository, create `.env` file with the following content in your local organization directory:

```shell
#!/usr/bin/env bash

git config --global user.email <email>
git config --global user.name <username>

# Due to volume mounts of the image, permission issues may occur. Observed examples:
# - PyCharm installed on Windows, codebase located in WSL 2 - autosave and backup denied for files created from container.
umask 0
```

Then, run:

```shell
. .env
git clone <repo-name>
cd "$(basename "$_" .git)"
touch .env
```

Copy following content into the `.env` file in your local repository directory:

```shell
#!/usr/bin/env bash

. ../.env

# Pre-commit needs to be installed to allow `git` actions (e.g. pre-commit, pre-push, etc.)
pre-commit install
```

Then, run:

```shell
. .env
```

Note: always source `.env` file (run: `. .env`) after starting container to avoid permission issues between host/container.

Example: static analysis with hooks managed by pre-commit can be executed by running:

```shell
pre-commit --all-files --hook-stage manual
```

Following CLI tools are contained within the image:

| Name                                                                                                  | Description                                       |
|-------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [atmos](https://atmos.tools/install/)                                                                 | Cloud architecture framework for native Terraform |
| [helm](https://helm.sh/docs/intro/install/)                                                           | Container orchestration package manager           |
| [k3s](https://docs.k3s.io/quick-start)                                                                | Container orchestration                           |
| [kubeconform](https://github.com/yannh/kubeconform?tab=readme-ov-file#installation)                   | Manifest validation                               |
| [mise](https://mise.jdx.dev/getting-started.html)                                                     | Tool version manager                              |
| [oci](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm#Quickstart)               | Oracle Cloud Infrastructure cloud provider        |
| [packer](https://developer.hashicorp.com/packer/tutorials/docker-get-started/get-started-install-cli) | Machine images provisioning                       |
| [pre-commit](https://pre-commit.com/)                                                                 | Managing pre-commit hooks                         |
| [terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)          | Infrastructure provisioning, static analysis      |
| [tflint](https://github.com/terraform-linters/tflint#installation)                                    | Static analysis                                   |
| [trivy](https://aquasecurity.github.io/trivy/latest/getting-started/installation/)                    | Static analysis                                   |
