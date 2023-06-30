<h1 align="center">    <a href="https://themanagers.wtf/">
      <img alt="pull requests welcome badge" src="https://themanagers.wtf/images/Logo.svg">
    </a></h1>



### The Managers - Hardhat implementation

This is the standard Hardhat implementation of ERC6551 used for deploying The Managers NFT project.

**What is implemented in this NFT?**

- ERC721A is used for base NFT to save on gas costs https://github.com/chiru-labs/ERC721A
- ERC2981 is used to set royalties  https://eips.ethereum.org/EIPS/eip-2981
- ERC4906 is used for setting metadata updates  https://eips.ethereum.org/EIPS/eip-4906
- DefaultOperatorFilterer which is used to "enforce" royalties https://github.com/ProjectOpenSea/operator-filter-registry

**Where is ERC6551 part?**

Key part is in **tokenBoundCreation** function that creates a NEW smart contract wallet and it is called when each ERC721A NFT is created

    function tokenBoundCreation(uint256 quantity, uint256 currentMinted) internal returns (bool) {
        for (uint256 i = 1; i <= quantity; i++) {
            ERC6551Registry.createAccount(
                ERC6551AccountImplementation,
                block.chainid,
                address(this),
                currentMinted + i,
                0,
                abi.encodeWithSignature("initialize()", msg.sender)
            );
        }

        return true;
    }

**How to deploy it?**

We are using Hardhat deploy plugin https://github.com/wighawag/hardhat-deploy so all this repo needs is you run
**npx hardhat deploy**