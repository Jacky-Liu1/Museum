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