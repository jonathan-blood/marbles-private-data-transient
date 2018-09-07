# Private data collections with Hyperledger Fabric 1.2

## Overview

Hyperledger Fabric v1.2 introduced the concept of private data collections as a way of authorising a subset of channel members in a Fabric to see certain pieces of data, while maintaining the visibility of the transaction to the entire network. For full information on this feature and its details, see [here](https://hyperledger-fabric.readthedocs.io/en/release-1.2/private-data/private-data.html).

This repository is an extension of the chaincode provided in [Fabric Samples](https://github.com/hyperledger/fabric-samples) for Marbles, making use of the BYFN network. In addition there is a basic Node client for interacting with the network. Together they demonstrate how to use the `transient` field within a transaction proposal to ensure the privacy of the data.

## Pre-requisites

In order to use the Node client for invoking the provided chaincode, you must first stand-up the BYFN network - the instructions can be found [here](https://hyperledger-fabric.readthedocs.io/en/latest/build_network.html). The provided chaincode in this repository must then be mounted to the Fabric CLI Docker container by editing the `docker-compose-cli.yaml` file before installing and instantiating it.

Standing up the BYFN network automatically generates crypto-material for a set of users. For the Node client to work, you must extract some of that crypto-material:

1. Copy the file in the `first-network/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore` directory to `/tmp/private_data_transient/cryptostore`, taking care to replace '_sk' with '-priv' in the file name.
1. Copy the file called 'Admin' from the `store` directory of this repository, to `/tmp/private_data_transient/store`.
1. Replace `<CRYPTO_STORE_FILENAME>` in the 'Admin' file with the name of the file in step 1, without '-priv' on the end.
1. Replace `<CERTIFICATE>` in the 'Admin' file with the contents of `first-network/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/signcerts/Admin@org1.example.com-cert`, ensuring to replace any new line with \n.

The `/tmp/private_data_transient` directory used here can be configured in `index.js` by changing the `storePath` and `cryptoStorePath` variables.

## Executing the Node client

`node index.js marble1 red 50 tom 100`