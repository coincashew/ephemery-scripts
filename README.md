# Ephemeral testnets scripts

Tooling for participating in [Ephemeral testnet](https://github.com/ephemery-testnet/ephemery-resources) based on its [genesis repository](https://github.com/ephemery-testnet/ephemery-genesis). 

Running a node in this test network requires resetting clients with a new genesis after given period. In this repository, you can find scripts and deployment for this automatized setup. 

## Installation instructions
### Step 1: Clone repo
```
mkdir -p ~/git
cd ~/git
git clone https://github.com/coincashew/ephemery-scripts
```
### Step 2: Setup .env file. Use only 1. Review contents. Edit, if required.
```
# nimbus-nethermind Ephemery setup
cp .env_nimbus-nethermind .env

# lighthouse-reth Ephemery setup
cp .env_lighthouse-reth .env

# teku-besu Ephemery setup
cp .env_teku-besu .env

# Review contents
cat .env
```

### Step 3: Setup crontab to run retention.sh every 5 mins
```
echo "*/5 * * * * $HOME/git/ephemery-scripts/retention.sh" | crontab -
```

## Retention script

Script `retention.sh` provides the main mechanism for resetting the network. It checks for period timeout and resets the node automatically. Configure your script using a file containing the following environment variables, and pass that file as the first argument to `retention.sh`.

- TESTNET_DIR - Path to the directory in which Ephemery testnet files are stored
- EL_CLIENT - Type of execution client, lower case: geth, nethermind, besu, erigon, or reth
- EL_SERVICE - Name of the execution client systemd service to start/stop
- EL_DATADIR - Data directory of the execution client
- EL_USER - **Optional:** Username that the genesis initialization should be run for the execution clients. Runs as script user if not assigned
- CL_CLIENT - Type of consensus client, lower case: prysm, lighthouse, nimbus, teku or lodestar
- CL_SERVICE - Name of the consensus client systemd service to start/stop
- CL_DATADIR - Data directory of consensus client
- CL_PORT - JSON RPC port of consensus client. Default: `3500`
- VC_CLIENT - **Optional:** Type of validator client, lower case: prysm, lighthouse, nimbus, teku or lodestar. Leave unset if using single-process consensus/validator client.
- VC_SERVICE - **Optional:** Name of the validator client systemd service to start/stop. Leave unset if using single-process consensus/validator client.
- VC_DATADIR - **Optional:** Data directory of validator client. Leave unset if using single-process consensus/validator client.
- TESTNET_FILES_USER - **Optional:** User to which testnet directory and files should be assigned. By default file ownership will be left unchanged.
- TESTNET_FILES_GROUP - **Optional:** Group to which testnet directory and files should be assigned. By default the group will be left unchanged.
- FORCE_RESET - **Optional:** Set to `1` to force reset of testnet files and clients for testing purposes. Default: `0`

See `.env.sample` for an example of setting the environment variables. Run `retention.sh .env` to use values set in `.env` environment variables file.

Default values for all environment variables may also be set within the script and the script can be run as `retention.sh`.

By default, the script is controlling clients using their systemd services. You can find examples files for services in `systemd-services` directory, you should also modify them to suit your system.



