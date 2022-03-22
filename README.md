# Museum
### Webapp that lets anyone post to a museum gallery - my attempt to learn Rust

### Anchor
1. anchor init projectname --javascript  (sort of like create-react-app)
2. anchor test   
- compiles programs
- spin up solana-test-validator and deploy program to our local solana network w/ our wallet
- actually call functions on our deployed program(like hitting a specific route on our server to test)


### Packages
1. npm install -g mocha
2. npm install @project-serum/anchor @solana/web3.js


### Solana
1. solana-keygen new   (generates a local Solana wallet)
2. solana address  (returns public address of local wallet)


### Steps 
1. anchor init
2. modify lib.rs, tests/museum.js, and Anchor.toml
3. anchor test


### Deploying to devnet
1. make sure solana-test-validator is not running anywhere
2. switch to devnet -> solana config set --url devnet
3. solana config get
4. airdrop yourself some SOL on devnet -> solana airdrop 2 (this will run it twice)
5. check balance -> solana balance
6. In Anchor.toml, change [programs.localnet] to [programs.devnet] and change cluster="localnet" to cluster="devnet"
7. anchor build (will create a new build for us with a program id)
8. Access program id -> solana address -k target/deploy/myepicproject-keypair.json
9. Go to lib.rs to get the id (declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS"))
10. Change the id in lib.rs to the one output by -> solana address -k target/deploy/myepicproject-keypair.json
11. Go to Anchor.toml and under [programs.devnet] change myepicproject="Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS" to new id
12. anchor build (build the project w/ our new program id)
13. anchor deploy
14. check solexplorer with your program id


Bridging frontend and backend
1. look for the idl file in target/idl/myepicproject.json
- Contains info about Solana program which is useful for frontend (Like ABI in Solidity)
2. Copy content in myepciproject.json and make a file idl.json on frontend(same level as App.js) and paste it in
3. Change wallet network to devnet
4. Airdrop some dev tokens to wallet -> solana airdrop 2 INSERT_YOUR_PHANTOM_PUBLIC_ADDRESS_HERE  --url devnet
5. Install these packages on frontend -> npm install @project-serum/anchor @solana/web3.js
6. Add these to App.js
  - import { Connection, PublicKey, clusterApiUrl } from '@solana/web3.js';
  - import { Program, Provider, web3 } from '@project-serum/anchor';
7. Creates a provider which is an authenticated connection to Solana
``` 
const getProvider = () => {
  const connection = new Connection(network, opts.preflightCommitment);
  const provider = new Provider(
    connection, window.solana, opts.preflightCommitment,
  );
	return provider;
}
```

```
import React, { useEffect, useState } from 'react';
import twitterLogo from './assets/twitter-logo.svg';
import './App.css';
import { Connection, PublicKey, clusterApiUrl} from '@solana/web3.js';
import {
  Program, Provider, web3
} from '@project-serum/anchor';

import idl from './idl.json';

// SystemProgram is a reference to the Solana runtime!
const { SystemProgram, Keypair } = web3;

// Create a keypair for the account that will hold the GIF data.
let baseAccount = Keypair.generate();

// Get our program's id from the IDL file.
const programID = new PublicKey(idl.metadata.address);

// Set our network to devnet.
const network = clusterApiUrl('devnet');

// Controls how we want to acknowledge when a transaction is "done".
const opts = {
  preflightCommitment: "processed"
}

// All your other Twitter and GIF constants you had.

const App = () => {
	// All your other code.
}
```


```
const getGifList = async() => {
  try {
    const provider = getProvider();
    const program = new Program(idl, programID, provider);
    const account = await program.account.baseAccount.fetch(baseAccount.publicKey);
    
    console.log("Got the account", account)
    setGifList(account.gifList)

  } catch (error) {
    console.log("Error in getGifList: ", error)
    setGifList(null);
  }
}

useEffect(() => {
  if (walletAddress) {
    console.log('Fetching GIF list...');
    getGifList()
  }
}, [walletAddress]);

```


```