# Ethernauts

WIP implementation for https://forum.ethernautdao.io/t/deploy-ethernaut-avatar-nfts-to-bootstrap-the-dao-s-tokenomics/299

## Requirements

- Node.js v16.8.0
- NPM v7.21.0
- Docker
- Docker Compose

## Solidity development

The following hardhat tasks will help you to:

- Start a local node: `npx hardhat node`
- Deploy the contract: `npx hardhat deploy`
- Open the sale: `npx hardhat sale-state`
- Mint sample tokens: `npx hardhat mint`

## DAPP/Kepers development

We use Docker and Docker Compose to run the development environment.

Run `docker-compose -f docker/development.docker-compose.yml up` to start the whole project. It includes the following services:

- `hardhat-node`: (http://localhost:4585)
- `hardhat-deploy`: Takes care of deploying the Ethernauts.sol contract to the docker hardhat network for development
- `ipfs` (http://localhost:5001)
- `dapp` (Next.js) (http://localhost:3000)
- `redis` (https://localhost:6379)
- `keeper-queue`: Node server that listens to mint & batch events and enqueues the necessary jobs to be processed.
- `keeper-jobs`: Node server processes the enqueued jobs by `keeper-queue` and executes them with the desired concurrency.

Run `docker-compose -f docker/development.docker-compose.yml down` to stop them.

---

You can also run/stop a single service using:

- `docker-compose -f docker/development.docker-compose.yml up service-name`
- `docker-compose -f docker/development.docker-compose.yml down service-name`

### Interacting with the Ethernauts Contract

You can run hardhat tasks inside running docker containers using the `docker compose exec ...` command.

So, first you want to open the sale state, and you can do it with the `sale-state` hardhat task, like so:

```bash
docker compose -f docker/development.docker-compose.yml exec hardhat-node sh -c 'cd /src/packages/hardhat && npx hardhat --network docker sale-state'
```

Then, you will be able to mint any amount of tokens using the `mint` task:

```bash
docker compose -f docker/development.docker-compose.yml exec hardhat-node sh -c 'cd /src/packages/hardhat && npx hardhat --network docker mint'
```
