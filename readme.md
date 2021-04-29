# Cross Chain Transaction

## About

Create token bridge between Ethereum & Binance Smart Chain, allow to transfer token between 2 chains by destorying (burn) one token on one chain and recreating (mint) same token on other chain.

## Architecture:
1. Bridge contracts for each chain
2. Bridge API in between to listen event emit and operate accordingly
3. TokenBase.sol : an ERC20 token contract which has mint and burn function
4. TokenEth.sol , TokenBsc.sol : tokens on both Ethereum and BSC which inherit TokenBase
5. BridgeBase.sol : bridge base contract inherit by bridge contract for both chain
6. eth-bsc-bridge.js : bridge API 

Example : Transfer token from Ethereum to BSC:
1. Send the token to bridge contract of Ethereum which burns the token
2. Event is emitted with the details of the transfer including recipient on BSC and amount
3. Bridge API receive the transfer event singal and send a transaction to the bridge contract of BSC
4. Mint the same token with the same amount and send it to the recipient address

## Avoid common attack

Build a re-entrancy guard with an array which store boolean value of a variable nonce. It is false when the transfer was initalized. It is set to true once the transfer is executed. It makes sure each transfer can be performed only once. 

## Pre-requisites and programs used versions:

- Truffle v5.1.7 (core: 5.1.7)
- Solidity v0.8.0 (solc-js)
- Node v10.17.0
- npm 6.11.3
- Openzeppelin

## Setting up the development environment

1. Install Truffle: 
    >npm install -g truffle

2. Install Openzepplin
    >npm install @openzeppelin/contracts@2.5.1

3. Install npm
    >npm install

4. From infura.io, get the rinkeby websocket testnet address
5. Use an ethereum address, get some rinkeby Eth from Rinkeby Authenticated Faucet site
3. Use an ethereum address, get some BNB from BSC Faucet site

## Testing
1. Migrate to ETH and BSC testnet
    - truffle migrate --reset --network ethTestnet
    - truffle migrate --reset --network bscTestnet
2. Check inital balances:
    - truffle exec scripts/eth-token-balance.js --network ethTestnet , ETH balance should have 1000
    - truffle exec scripts/bsc-token-balance.js --network bscTestnet , BSC balance should be 0
3. Open a new terminal A and run the bridge API
    - truffle exec
    - node scripts/eth-bsc-bridge.js
4. On other terminal B and transfer 1000 from ETH to BSC:
    - truffle exec scripts/eth-bsc-transfer.js --network ethTestnet
    - In Terminal A, bridge API detects an event and trigger the mint function, process the transfer
5. Check new balance:
    - truffle exec scripts/eth-token-balance.js --network ethTestnet , ETH balance should have 0
    - truffle exec scripts/bsc-token-balance.js --network bscTestnet , BSC balance should be 1000

## Project developed by : Solidity, smart contract, web3, node.js

## Refrences
Web3: https://youtube.com/playlist?list=PLS5SEs8ZftgXXPYBH6rDk4TKnDOvinwJr

Crosschain : 
https://docs.symbolplatform.com/concepts/cross-chain-swaps.html
https://andrecronje.medium.com/deploying-your-own-cross-chain-token-101-240420efd0d9


Bridge between the tokens :
https://forum.openzeppelin.com/t/how-to-make-a-bridge-token-between-ethereum-and-binance-smart-chain/4628
https://medium.com/the-liquidapps-blog/cross-chain-bridge-guide-for-experts-947210576668



For reference : 
https://github.com/rsksmart/tokenbridge
