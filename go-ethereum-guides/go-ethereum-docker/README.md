# Docker for Developing DApps with Geth

## Problem statement

- Lots of resources being put into development of Ethereum Geth codebase changes
  rapidly. Stabilize DApp builds — you'll want to consciously select
  dependencies and build environments; upgrade selectively to keep environment
  consistent and known. Building on Windows is difficult. For example,
  installing and using the Solidity compiler requires GCC to be installed and
  configured to be called in the command line. Have to mess with [Mingw-w64](https://mingw-w64.org/doku.php)
  — cannot control build dependencies because tied to machine environment.
  Dependency upgrades are testable. Spin up a new container with the config you
  want to test, without having to trash your original config or container
- Docker allows you to decouple your build environment from your machine's
  native environment. Relatively lightweight: \<compare running VMs as opposed
  to docker containers\> Build environments are always clean and known — `docker
  run` provisions a container as per configuration. The longer you run a
  machine, the more likely the machine (virtual or otherwise) drifts from its
  original configuration.

## Requirements

- Docker 18.0.9 (Community version suffices) Geth 1.8.2x

Download the 
[example application](https://github.com/kauri-io/Content/tree/master/go-ethereum-guides/write-basic-quiz-dapp-in-go/quiz-dapp),
from the [Creating a DApp in Go with
Geth](https://kauri.io/article/60a36c1b17d645939f63415218dc24f9/v2/creating-a-dapp-in-go-with-geth)
guide.

## What does Docker do?

Docker is a container service that you can run on your local machine.


## Run an Ethereum node in a Docker container

1. Create a "data" directory. Docker containers cannot and should not be used to hold data that needs to be persistent. You can either persist data in Docker "volumes" (`docker volume create <volume_name>`) to allow Docker to create virtual volumes that it manages for you (necessary in container orchestration and deploying to a Docker-compatible cloud compute service), or tell Docker to mount a local directory (useful in development environments, where you might want easy access to the contents of mounted volumes) as a local directory.
2. Run the `ethereum/client-go` container.

```bash
if [ ! -d $(pwd)/data ]; then
  mkdir $(pwd)/data
fi

# Simplest
# docker run -it -p 30303:30303 ethereum/client-go

# With data volume
# docker run -it -p 30303:30303 -v $(pwd)/data:/root/.ethereum ethereum/client-go:alltools-v1.8.19

# With interactive JS console
# docker run -it -p 30303:30303 ethereum/client-go console

# With JSON RPC
docker run -it -v $(pwd)/data:/root/.ethereum -p 8545:8545 -p 30303:30303 ethereum/client-go --rpc --rpcaddr "0.0.0.0"
```

Notes:

- Geth exposes JSON-RPC service on port 8545
- Need to expose JSON-RPC service on `0.0.0.0`. By default, publishes to `127.0.0.1`, which only allows `localhost` access internally. Publishing to `0.0.0.0` allows **public** access your `go-client` container's RPC service. **DO NOT DO THIS IN PRODUCTION**. (1) re-check why this is necessary (2) check if this is only applicable to Docker on macOS, because Linux Docker containers all have publicly accessible IP addresses (IIRC).

## Compile a Solidity contract with Docker

- Need `solc` to compile Solidity contracts, which we then compile to an ABI (Application Binary Interface) with Go bindings with the `abigen` tool. Compiling Solidity contracts to an [Ethereum Contract ABI](https://solidity.readthedocs.io/en/v0.5.8/abi-spec.html) allows us to interact with the contract using a programming language of our choice. 
- Solidity only [publishes binaries](https://github.com/ethereum/solidity/releases) for minor versions (see semver). That means that to use a specific patch version e.g. 0.4.21, you have to either build the binaries yourself, or use a prebuilt Docker image e.g. `docker run ethereum/solidity:0.4.24` to use the version `0.4.24` of the Solidity compiler.

NOTE: `solc --optimize --bin sourceFile.sol` to optimize compiled contract for deployment

