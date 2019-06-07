## Compile Solidity Contract with Docker

- Need `solc` to compile Solidity contracts, which we then compile to an ABI (Application Binary Interface) with Go bindings with the `abigen` tool. Compiling Solidity contracts to an [Ethereum Contract ABI](https://solidity.readthedocs.io/en/v0.5.8/abi-spec.html) allows us to interact with the contract using a programming language of our choice. 
- Solidity only [publishes binaries](https://github.com/ethereum/solidity/releases) for minor versions (see semver). That means that to use a specific patch version e.g. 0.4.21, you have to either build the binaries yourself, or use a prebuilt Docker image e.g. `docker run ethereum/solidity:0.4.24` to use the version `0.4.24` of the Solidity compiler.

NOTE: `solc --optimize --bin sourceFile.sol` to optimize compiled contract for deployment

To compile a Solidity contract, run in the Terminal:

```bash
docker run -ti --rm -v $(pwd):/root ethereum/solc:0.5.2 --abi --bin /root/quiz/quiz.sol -o /root/build --overwrite
```

----

We'll need to compile our Solidity contract to produce two files: a `.bin` and a `.abi` file. 
