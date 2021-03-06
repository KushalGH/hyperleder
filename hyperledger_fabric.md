 # Hyperledger Fabric
- Hyperledger Fabric is an enterprise-grade, permissioned, distributed ledger technology platform designed for use in enterprise contexts, which means Fabric offers features that are required or essential in enterprise applications.
- Hyperledger Fabric is primarily used to build private networks where all of the nodes in the participating network are known and verified. 
- These nodes need not necessarily trust one another, but the identity of every participating node is known.

# Characteristics of Hyperledger Fabric

#### Permissed:
- Unknown peers cannot participate
- Private channel between participants
- All of the participating nodes in this blockchain network have to be verified, and this is done using something called a Membership Service Provider or the MSP.
- Participating nodes in a Hyperledger Fabric network have to have a unique digital identity, which is created using public key infrastructure and MSPs use these identities to determine which nodes can participate.
- All of the participating nodes in this network have digital signatures that uniquely identify them. The peers and the identities are known and verified.

- They use public key infrastructure that is a public and a private key pair in order to associate a unique digital identity with each peer. Every peer has both a public, as well as a private, key. 

-  The private key is what the peer will use who sign transactions, so that transactions can be verified as having originated from a particular known peer. All of the peers that are on a Fabric network will have access to the public channel and there will be a shared ledger associated with this public channel that all of these nodes can update. 


#### Transactions:
- Low latency and high throughtput for transaction verification and committment.


#### Pluggable:
- Hyperledger Fabric has a modular design, which means the format of the ledger data, the membership providers, which determine what nodes can participate in the network, as well as consensus algorithm are all pluggable based on your use case.

# Transaction Flow in Hyperledger fabric
- Transctions or Smart contracts in Hyperledger are implemented using chaincode
- Chaincode is a self-contained program that runs on Hyperledger fabric and satisfies a standard interface, typically used to implement smart contracts.
- Chaincode can be implemented using GoLang, NodeJS or Java.
- Chaincode is a self-contained program, which means that it maintains it's own private state that can be updated when the traansctions are executed.

# Life Cycle of Transction in Hyperledger Fabric
- **Phase 1: Execute Phase:** This is where chaincode for smart contracts are executed, and all of the transactions in a particular block can be executed in parallel as well.  Execution is performed by endorsing peers. Once transactions have been executed and endorsed they are then passed to an ordering service.
- **Phase 2: Order Phase:** The ordering service or orderer nodes are then responsible for arranging all of these transactions in some order after which they are passed to validating peers or a validating service.
- **Phase 3: Validation Phase:** Each peer on the network validates the transactions before committing them to the ledger. The validating peers is where transactions are verified to ensure that they can be applied to the latest state of the ledger. Validating peers help check for issues, such as double spending, ensuring that the same asset is not sent to multiple participants at the same time.

![picture](images/trans_flow.png)

- **Flow of transactions**
    -  The client application will interact with the peer on the blockchain network and submit a transaction proposal. 
    -  This **transaction proposal** comprises of transactions, which are then executed by only a few peers on the network. These are called the **endorsing peers**, and the endorsing peers will execute the transactions, maybe even in parallel, and send back endorsed transactions to the client application.
    -  Executing transactions is done only by a few endorsing peers in parallel. Peers who endorse transactions are chosen by an **endorsement policy**.
    -   That is something that you can configure. Endorsing peers who execute the transaction have access to the chain code directly, which means you can pick and choose your endorsing peers with care. Execution does not update the state of the ledger of you blockchain network. 
    -   The **endorsement policy** is something that you specify for your Chaincode application. For example, 
        -   you might say that you have a specific list of peers on the network, and all of these peers need to endorse your transaction 
        -   or the endorsement policy could be something like the majority of the peers on the network must endorse transactions 
        -   or at least N peers in this list must endorse.
    -  Once the endorsed transactions have been sent back to the client application the client application will then pass these endorsed transactions onto an **ordering service or the orderer nodes**.
    -  The ordering service does not worry about the validity of the transactions. Its only job is to basically **assemble all of these transactions in some serial order**. This is the order in which the transactions will be committed to the ledger of our blockchain network. 
    -  On your Fabric network there will be specific nodes that are designated as orderers, and these nodes are responsible for ordering transactions. Now these nodes might follow different methodologies for ordering. 
        - **SOLO:**  If you follow SOLO ordering there's just one orderer node, which determines the order of transactions.
        - **Kafta**
        - **Practical Byzantine Fault Tolerance**
    -  Orderers, once transactions have been ordered, pass these transactions on to **committing peers**. 
    -  **Committing peers** in your Fabric network are those responsible for validating the transactions that have been ordered and endorsed. Committing peers also update the state of the ledger. All peers which have access to the shared ledger are committing peers, and they apply the transactions to their private ledger copy, but before they apply the transactions they need to ensure that the transactions have been endorsed correctly based on the endorsement policy. 
    -  Committing peers also need to verify that all of the transactions in the set of transactions that they've received are **valid on the latest version** of the ledger. They need to access the latest version, apply the transactions, and see whether they are valid, and committing peers have the power to invalidate transactions. 
    -  For those of you who come from an Ethereum world, observe that there is no cryptocurrency associated with the mining of transactions in Fabric. These make Fabric transactions more deterministic and more suited to an enterprise context. 

-  **Assets and Ledgers in Fabric**
    -  **Assets** in Fabric can be anything of monetary value that can be exchanged. An asset is something that a sender sends a recipient.
    -  **The ledger** in a blockchain network is basically the blockchain itself. The blockchain represents the current state of assets, and all of the transactions that have been performed using those assets.
    -  Assets in the Fabric network can be 
        -  **Tangible assets**, such as property, real estate, cars, hardware, or 
        -  **Intangible assets**, such as contracts or intellection property. 
    - Assets in a Hyperledger Fabric network are modeled in the form of key value pairs. These key value pairs represent the current state of the asset.  
    -  The assets are stored in a state database, and you can update individual fields of an asset by updating the corresponding key value pair. 
    -  **Ledger** in Fabric is made up of two components
        - **World database:**   The world database contains the current state of all of the assets. 
        - **Transaction Ledger:** That is represented using our Fabric network and the transaction ledger contains all of the state changes, which involve one or more of these assets. 
        - When transactions are committed to the ledger the committing peers update the world database representing the latest state of each asset, as well as record the changes in the transaction's ledger. 


# Hyperledger Fabric network using Docker containers on our local machine.

Setting up the Hyperledgere network using docker on local. The peers of the network and the orderers will be hosted in Docker containers.
 
- Step 1: create a directory
```
mkdir fabric_demo
```
- Step 2:  In order to download all of the Fabric binaries and the Docker images that will allow us to run a Fabric network on our local, we need to execute a script that is available at this URL.

![picture](images/setup.png)
![picture](images/hyper_1.png)

```
curl -sSL https://goo.gl/6wtTN5 | bash -s 1.1.0
// docker is not installed then read readme.md file (topic: INSTALLING DOCKER & DOCKER-COMPOSER ON UBUNTU) 
```


- Step 3: run this command to know, what all docker images are downloaded
![picture](images/image-hyper_3.png)

```
docker images
```


It also, provides exaples to play with. you can kind them under: fabric-samples


- Step 4: Navigate to "/demo/fabric_demo/fabric-samples/first-network" and type ls. this will show us differnet YAML files. **These YAML files** contain all of the container specifications for multiple containers. 

- Step 5: Hyperledger fabric offers a very handy script that you can use to bring up your first network. this is called BYFN.sh [Build your first netwrok] 

![picture](images/setup_2.png)

```
/demo/fabric_demo/fabric-samples/first-network$ ./byfn.sh -m generate

// certificates are generated using cryptogen tool
// generate command generates the network
```

> This byfn.sh sets up facbric network which includes:
> -public channel called mychannel - a channel for execution
> -Two peers from 2 organizations each
> -Certificates for both the peers
> -Genesis block for the blockchain 

- Step 6: To bring up the network, we use: 

![picture](images/hyper_up_01.png)
![picture](images/hyper_up_02.png)

```
/demo/fabric_demo/fabric-samples/first-network$ ./byfn.sh -m up
```

Once you execute the command [./byfn.sh -m up ] this is what will happen automatically through the command:
1. Creating network "net_byfn" with the default driver
2. Creating volume "net_peer0.org2.example.com" with default driver
3. Creating volume "net_peer1.org2.example.com" with default driver
4. Creating volume "net_peer1.org1.example.com" with default driver
5. Creating volume "net_peer0.org1.example.com" with default driver
6. Creating volume "net_orderer.example.com" with default driver
7. Channel "mychannel" is created successfully. this can use **ordering service available at  orderer.example.com port 7050**
8. peer0.org1 joined on the channel "mychannel"
9. peer1.org1 joined on the channel "mychannel" 
10. peer0.org2 joined on the channel "mychannel" 
11. peer1.org2 joined on the channel "mychannel"
12. Anchor peers for org "Org1MSP" on "mychannel" is updated successfully. **The anchor peers for each org are the main peers that communicate with peers from other organizations. Those have also been updated successfully.** 
13. Anchor peers for org "Org2MSP" on "mychannel" is updated successfully
14. Chaincode is installed on peer0.org1. **In order to test your netwrok. The script installs bunch of example chaincode on each of your peers.**
15. Chaincode is installed on peer0.org2
16. Chaincode Instantiation on peer0.org2 on channel 'mychannel' is successful
17. Querying on peer0.org1 on channel 'mychannel' **This chain code isthen queried and executed using the peer command**
18. Query on peer0.org1 on channel 'mychannel' is successful 
19. Invoke transaction on peer0.org1 on channel 'mychannel' is successful
20. Chaincode is installed on peer1.org2
21. Querying on peer1.org2 on channel 'mychannel'. **This runs peer chaincode query command to test our network. that is used to query chanincode for a particular value from the state database. It doesnot invoke a transactoin. peer chaincode query command is what we will execute in order to change the state of the ledger, ie. executng a transaction.**
22. Query on peer1.org2 on channel 'mychannel' is successful
23. All GOOD, BYFN execution completed 


- Step 7 







