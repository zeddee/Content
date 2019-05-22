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


