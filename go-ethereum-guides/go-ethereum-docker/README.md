# Docker for Developing DApps with geth

## Problem statement

- Lots of resources being put into development of Ethereum geth codebase changes
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
- Environments are managed through configuration files,
which can be version-controlled, diffed, examined, debugged, reused, and forked.

## Why use Docker instead of <insert tool here>

- Docker allows you to get as close to a production environment as possible. I
- Docker gives you a clean and controlled environment to work with.
- 


## Requirements

- Docker 18.0.9 (Community version suffices) geth 1.8.2x

Download the 
[example application](https://github.com/kauri-io/Content/tree/master/go-ethereum-guides/write-basic-quiz-dapp-in-go/quiz-dapp),
from the [Creating a DApp in Go with
geth](https://kauri.io/article/60a36c1b17d645939f63415218dc24f9/v2/creating-a-dapp-in-go-with-geth)
guide.

## What does Docker do?

Docker is a container service that you can run on your local machine.

## Why would you want to run a private Ethereum node?

* Consistent environments for development and production.
* Own your Ethereum account and keys.

## Run a Private Ethereum node in a Docker container

WARNING: The following instructions show you how to start a local Ethereum node with Docker. You MUST publish the RPC ports at 0.0.0.0 when running a node in a docker container in order to be able to access the node through a JSON-RPC endpoint, because the geth JSON-RPC service is by default bound to `127.0.0.1` or the loopback IP address, which can only be accessed locally i.e. from within the container itself.

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

- geth exposes JSON-RPC service on port 8545
- Need to expose JSON-RPC service on `0.0.0.0`. By default, publishes to `127.0.0.1`, which only allows `localhost` access internally. Publishing to `0.0.0.0` allows **public** access your `go-client` container's RPC service. **DO NOT DO THIS IN PRODUCTION**. (1) re-check why this is necessary (2) check if this is only applicable to Docker on macOS, because Linux Docker containers all have publicly accessible IP addresses (IIRC).


