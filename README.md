<div align="center">
  <h1>Papyrus</h1>
  <img src="./resources/img/papyrus-logo-square.png" height="200" width="200">
  <br />
  <a href="https://github.com/starkware-libs/papyrus/issues/new?assignees=&labels=bug&template=01_BUG_REPORT.md&title=bug%3A+">Report a Bug</a>
  ·
  <a href="https://github.com/starkware-libs/papyrus/issues/new?assignees=&labels=enhancement&template=02_FEATURE_REQUEST.md&title=feat%3A+">Request a Feature</a>
  ·
  <a href="https://github.com/starkware-libs/papyrus/discussions">Ask a Question</a>
</div>

<div align="center">
<br />

![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/starkware-libs/papyrus/ci.yml?branch=main)
[![Project license](https://img.shields.io/github/license/starkware-libs/papyrus.svg?style=flat-square)](LICENSE)
[![Pull Requests welcome](https://img.shields.io/badge/PRs-welcome-ff69b4.svg?style=flat-square)](https://github.com/starkware-libs/papyrus/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)
[![codecov](https://codecov.io/gh/starkware-libs/papyrus/branch/main/graph/badge.svg?token=YZA9IPDHRM)](https://codecov.io/gh/starkware-libs/papyrus)

</div>

<details open="open">
<summary>Table of Contents</summary>

- [:warning: Disclaimer](#warning-disclaimer)
- [About](#about)
- [Getting Started](#getting-started)
  - [Compiling and running `papyrus`](#compiling-and-running-papyrus)
  - [Configuration](#configuration)
- [Running `papyrus` with Docker](#running-papyrus-with-docker)
- [Endpoints](#endpoints)
- [Roadmap](#roadmap)
- [Support](#support)
- [Project assistance](#project-assistance)
- [Contributing](#contributing)
- [Authors \& contributors](#authors--contributors)
- [Security](#security)
- [License](#license)

</details>

---

## :warning: Disclaimer

:warning: :construction: `Papyrus` is still being built therefore breaking changes might happen often so use it at your own risks.:construction: :warning:

## About

`Papyrus` is a StarkNet full node written in Rust.

## Getting Started

### Compiling and running `papyrus`

Prerequisites

- [Rust](https://www.rust-lang.org/tools/install)

You can build and run a `papyrus` node with the default configuration by running:

```bash
mkdir data
cargo run --release --package papyrus_node --bin papyrus_node
```

### Configuration

`Papyrus` default [configuration file](config/default_config.json) documents all the available parameters,
including pointers to common values and `#is_none` flags for optional parameters.

You can override the default with your own key-value configuration json file, by providing a path to the
`--config_file` command-line argument. Note that the configuration file can be partial or even empty.
Alternatively, You can specify environment variables and command-line arguments.
See the available command-line arguments by running:

```bash
cargo run --release --package papyrus_node --bin papyrus_node -- --help
```

See [here](crates/papyrus_config/README.md) for more information.

## Running `papyrus` with Docker

#### Prerequisites

- [Docker](https://docs.docker.com/get-docker/)

#### Command line
You can run a `papyrus` node with the default configuration by running:

```bash
docker run --rm --name papyrus\
  -p 8080-8081:8080-8081 \
  -v <local-host-data-path>:/app/data \
  ghcr.io/starkware-libs/papyrus:dev
```

> **NOTE**  
> - The container must have write access to `<local-host-data-path>`.
> A possible way to assure this is to create the `<local-host-data-path>` directory (only the first
> time you run `papyrus`) and add `--user "$(id -u):$(id -g)"` to the docker run command.
> - You must include the `dev` tag which keeps track of our development branch and contains the most
> up-to-date code. Once we have official releases we will add a `latest` tag for the latest release.
> - Currently, there is no automatic upgrade mechanism.
> Make sure to periodically pull the latest image and re-run the node.

## Memory usage
The Papyrus node will use all the RAM it can in order to cache the storage.

If you're not running any other applications on your machine, this is the recommended behavior.

Otherwise, you can limit the node's memory usage by running it in a container with a limited memory.
Note that it might make the node less efficient as it will decrease the caching of the storage.

This can be done by adding the flag `--memory 1g` (For a 1GB limitation) to the command in the [Docker](#command-line) section.
The full command should be

```bash
docker run --rm --name papyrus\
  -p 8080-8081:8080-8081 \
  -v <local-host-data-path>:/app/data \
  --memory <memory-limit>
  ghcr.io/starkware-libs/papyrus:dev
```

For more information, see [Docker's documentation](https://docs.docker.com/config/containers/resource_constraints/#limit-a-containers-access-to-memory).


## sending API requests to the node
API requests are sent to the path `/rpc/<starknet-rpc-version-id>`.
Current supported versions are:
* V0_3_0
* V0_4_0  


See specification at: [starknet-specs repo](https://github.com/starkware-libs/starknet-specs/)  
Assuming the node is exposed at `localhost:8080` one might send requests via curl with:
```bash
curl --location 'localhost:8080/rpc/v0_3' --header 'Content-Type: application/json'\  
 --data '{"jsonrpc":"2.0","id":0,"method":"starknet_blockHashAndNumber"}'
```

> **NOTE**  
> The default expected version id is in the format: `v<Major>_<Minor>`.  
> Also supported: `V<Major>_<Minor>` and `v<Major>_<Minor>_<Patch>` (or any combination of the above).  
> The Node always supports one patched version per minor version (the latest for that minor version).  
> When specifying a patch the semantic is grater or equal to the specified patch.  

## Endpoints

| Endpoint                                   | V0.3                         | V0.4                         |
| :----------------------------------------- | :--------------------------- | :--------------------------- |
| `starknet_addDeclareTransaction`           | ![](resources/img/cross.png) | ![](resources/img/check.png) |
| `starknet_addDeployAccountTransaction`     | ![](resources/img/cross.png) | ![](resources/img/check.png) |
| `starknet_addInvokeTransaction`            | ![](resources/img/cross.png) | ![](resources/img/check.png) |
| `starknet_blockHashAndNumber`              | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_blockNumber`                     | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_call`                            | ![](resources/img/cross.png) | ![](resources/img/check.png) |
| `starknet_chainId`                         | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_estimateFee`                     | ![](resources/img/cross.png) | ![](resources/img/check.png) |
| `starknet_getBlockTransactionCount`        | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getBlockWithTxHashes`            | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getBlockWithTxs`                 | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getClass`                        | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getClassAt`                      | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getClassHashAt`                  | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getEvents`                       | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getNonce`                        | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getStateUpdate`                  | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getStorageAt`                    | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getTransactionByBlockIdAndIndex` | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getTransactionByHash`            | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_getTransactionReceipt`           | ![](resources/img/check.png) | ![](resources/img/check.png) |
| `starknet_pendingTransactions`             | ![](resources/img/cross.png) | ![](resources/img/cross.png) |
| `starknet_syncing`                         | ![](resources/img/check.png) | ![](resources/img/check.png) |

## Deployment
We provide a helm chart for deploying the node to a kubernetes cluster.
It is located under the [deployments folder](deployments/helm/README.md).

## Roadmap

See the [open issues](https://github.com/starkware-libs/papyrus/issues) for a list of proposed features (and known issues).

- [Top Feature Requests](https://github.com/starkware-libs/papyrus/issues?q=label%3Aenhancement+is%3Aopen+sort%3Areactions-%2B1-desc) (Add your votes using the 👍 reaction)
- [Top Bugs](https://github.com/starkware-libs/papyrus/issues?q=is%3Aissue+is%3Aopen+label%3Abug+sort%3Areactions-%2B1-desc) (Add your votes using the 👍 reaction)
- [Newest Bugs](https://github.com/starkware-libs/papyrus/issues?q=is%3Aopen+is%3Aissue+label%3Abug)

## Support

Reach out to the maintainer at one of the following places:

- [GitHub Discussions](https://github.com/starkware-libs/papyrus/discussions)
- Contact options listed on [this GitHub profile](https://github.com/starkware-libs)

## Project assistance

If you want to say **thank you** or/and support active development of Papyrus:

- Add a [GitHub Star](https://github.com/starkware-libs/papyrus) to the project.
- Tweet about the Papyrus.
- Write interesting articles about the project on [Dev.to](https://dev.to/), [Medium](https://medium.com/) or your personal blog.

Together, we can make Papyrus **better**!

## Contributing

First off, thanks for taking the time to contribute! Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make will benefit everybody else and are **greatly appreciated**.

Please read [our contribution guidelines](docs/CONTRIBUTING.md), and thank you for being involved!

## Authors & contributors

For a full list of all authors and contributors, see [the contributors page](https://github.com/starkware-libs/papyrus/contributors).

## Security

Papyrus follows good practices of security, but 100% security cannot be assured.
Papyrus is provided **"as is"** without any **warranty**. Use at your own risk.

_For more information and to report security issues, please refer to our [security documentation](docs/SECURITY.md)._

## License

This project is licensed under the **Apache 2.0 license**.

See [LICENSE](LICENSE) for more information.
