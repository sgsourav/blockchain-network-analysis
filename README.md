# Measurements, Analyses, and Insights on the Entire Ethereum Blockchain Network
**The Web Conference 2020 (WWW '20), April 20--24, 2020, Taipei, Taiwan**     
*Xi Tong Lee, Arijit Khan, Sourav Sen Gupta, Yu Hann Ong, and Xuan Liu*    
*Nanyang Technological University, Singapore*

> Blockchains are increasingly becoming popular due to the prevalence of cryptocurrencies and decentralized applications.  Ethereum is a distributed public blockchain network that focuses on running code (smart contracts) for decentralized applications. More simply, it is a platform for sharing information in a global state that cannot be manipulated or changed. Ethereum blockchain introduces a novel ecosystem of human users and autonomous agents (smart contracts). In this network, we are interested in all possible interactions: user-to-user, user-to-contract, contract-to-user, and contract-to-contract. This requires us to construct interaction networks from the entire Ethereum blockchain data, where vertices are accounts (users, contracts) and arcs denote interactions. Our analyses on the networks reveal new insights by combining information from the four networks. We perform an in-depth study of these networks based on several graph properties consisting of both local and global properties, discuss their similarities and differences with social networks and the Web, draw interesting conclusions, and highlight important, future research directions.    

---

## Dataset : Four Interaction Networks of Ethereum

We are interested in all interactions between Ethereum accounts, both in terms of standard ether transactions and token transfers. This requires us to construct *interaction network* from the Ethereum blockchain data, where vertices are accounts (users or contracts) and arcs denote their interactions. There are four major types of interaction between Ethereum addresses --- (i) User-to-User (transaction or token transfer), (ii) User-to-Contract (call or kill), (iii) Contract-to-User (transaction or token transfer), and (iv) Contract-to-Contract (create, call, kill or hard fork). In addition, there are some interactions to and from the *Null* address, which denote creation of smart contracts and generation of ether (mining rewards), respectively. We create four interaction networks out of the entire blockchain dataset of Ethereum, as follows.

1. TraceNet -- All possible user and smart contract addresses found in the entire blockchain dataset are vertices, and all successful traces with non-null from/to addresses are arcs.

2. ContractNet -- This is a subgraph of TraceNet, a pure contract-to-contract interaction network on Ethereum, where arcs are direct messages and/or transactions between smart contracts.

3. TransactionNet -- This is the network of all Ethereum transactions, made by users, either to other users or smart contracts, or to a *Null* address in case of smart contract creation.

4. TokenNet -- This pertains only to the transfer of tokens between Ethereum accounts.

This repository consist of the four interaction networks in their respective folders. 

Google Cloud BigQuery curates the entire Ethereum blockchain data in terms of blocks, contracts, transactions, traces, logs, tokens and token transfers. We extract all relevant data for Ethereum from the *ethereum_blockchain* dataset under the Google Cloud *bigquery-public-data* repository, till 2019-02-07 00:00:27 UTC, which amounts to all blocks from genesis (#0) up to #7185508. Files in this repository are converted and extracted from the original data on Google Cloud BigQuery. Scripts for hashing the files and extracting the four networks from the original data set can also be found in the respective folders.

As the files are relatively huge, they are split into multiple text files, separated by commas. After extraction, the files are split using linux command line. 

Example : split -d -l 1500000 trace_net.csv trace_net_       
Format for hash files: address, hash      
Format for net files: from_address_hash, to_address_hash      

Credits:
https://www.kaggle.com/bigquery/ethereum-blockchain (original dataset)

---

## Data Extraction from Google Cloud BigQuery

1. Login to Google Cloud Platform. 
2. Create a bucket to store your files.
2. Go to BigQuery and find the data set 'ethereum_blockchain'
3. Select the table you want and 'Export to GCS'.
4. Then select the GCS location (the bucket created in step 2). 
5. If csv is preferred: <bucket>/<folder>/file*.csv (e.g. tmpbucket/blocks/blocks*.csv). <bR>
   The * will help to number the files as exporting the tables will split the data into multiple files. <br>
   Replace .csv with .txt or .json as per your preference.
6. Pip install gsutil, open command line and download the files. (Tried with Python 2.7 in Ubuntu)<br>
   For downloaded entire folder: gsutil -m cp -r gs://bucketname/folder-name local-location <br>
   For downloaded multiple files: gsutil -m cp -r gs://bucketname/folder-name/filename* local-location<br>

If gstuil does not work, manual download is possible from the bucket (not recommended).<br>
After downloaded the necessary data you might want to delete the bucket to prevent charges.<br>
Alternative method: https://github.com/blockchain-etl/ethereum-etl <br>


   
