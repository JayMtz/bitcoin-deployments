This repository is version control for the runtime configuration of bitcoin node deployments.

## Context

Software is typically deployed, or ran, in different environments with different configurations for each environment. In the case of this repository, the software being deployed, or ran, is the bitcoin node software. The software has the ability to connect to other peers, fetch the entire blockchain from these peers, receive new blocks, and submit blocks amongst other things like hosting the RPC, remote procedure call, API. For example, in development, we don't want to use a bitcoin node that is connected to the main blockchain as submitting transactions costs real money and there is no way to wipe the data. For development, it would be better to have a sandbox like environment that is free to use and has no consequences if we mess up the state of the blockchain, as we could just wipe it and start over.

Common environment types:
- **Development**: Sandbox like environment. Typically very fast with little data. Something to code against with a very fast feedback loop and no consequence for getting things wrong.
- **Test**: After you write and merge some code to the codebase, this code can be deployed to a test environment that other team members can interact with. After code looks good here it gets promoted to staging.
- **Staging**: This environment should as closely mirror production environment as possible. The idea being that if it works in this staging environment we could be very confident that it will work properly in production. Often, production data will be copied over to the staging environment on a regular cadence to give the environment a very production like experience. In this sense, staging is the same as production minus a lag, depending upon how often the production data is brought down to staging.
- **Production**: The environment where the code is live. Real users. Real data. Very hard to go back. In the case of the blockchain, you can't revert.


## Functionality

In the case of this repository, the different environments represent different bitcoin blockchain networks we are connecting to and different configurations for those connections. In `/dev`, we connect to a locally running "regtest" blockchain. In `/test`, we connect to "testnet", a blockchain used by other people but the coins are still valueless. Lastly, `/prod` connects to the "mainnet", the main blockchain.

Each `docker-compose.yml` represents the runtime configuration of a bitcoin node. That is what version of the node are we running, what network are we connecting to, and what credentials are we using to connect to the node amongst other configurations. 

The configuration file spins up a bitcoin node via a docker container and connects to a network of nodes.

## Deploying the nodes

First we need to generate the credentials that will be used in the http requests to our bitcoin node. Pick any username you like. See the documentation folder for how the credentials are supplied in http requests.
```
❯ curl -sSL https://raw.githubusercontent.com/bitcoin/bitcoin/master/share/rpcauth/rpcauth.py | python3 - <username>
```
```
String to be inserted into docker-compose.yml
rpcauth=foo:7d9ba5ae63c3d4dc30583ff4fe65a67e$9e3634e81c11659e3de036d0bf88f89cd169c1039e6e09607562d54765c649cc

Your password:
qDDZdeQ5vw9XXFeVnXT4PZ--tGN2xNjjR4nrtyszZx0=
```
In the directory of the environment that you are deploying against run:

`docker-compose up`


## Further Reading:
- Docker Bitcoin Node Documentation: https://github.com/ruimarinho/docker-bitcoin-core
- Bitcoin RPC API: https://developer.bitcoin.org/reference/rpc/
- Docker overview: https://docs.docker.com/get-started/overview/
- Docker compose: https://docs.docker.com/compose/
