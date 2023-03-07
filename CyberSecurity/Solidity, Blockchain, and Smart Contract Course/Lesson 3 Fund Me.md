## Payable

```
//SPDX-Lincense-identifier: MIT

  

pragma solidity >=0.6.6 <0.9.0;

  

contract FundMe {

  

mapping(address => uint256) public addressToAmountFunded;

function fund() public payable {

addressToAmountFunded[msg.sender] += msg.value;

}

}
```

2:32:00


Getting External Data With Chainlink

`or currency`

Decentralized Oracle Network Chainlink

https://docs.chain.link/docs/link-token-contracts/

acrescentar isto aplicaçao
https://docs.chain.link/docs/get-the-latest-price/

```
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {

    AggregatorV3Interface internal priceFeed;

    /**
     * Network: Goerli
     * Aggregator: ETH/USD
     * Address: 0xD4a33860578De61DBAbDc8BFdb98FD742fA7028e
     */
    constructor() {
        priceFeed = AggregatorV3Interface(0xD4a33860578De61DBAbDc8BFdb98FD742fA7028e);
    }

    /**
     * Returns the latest price
     */
    function getLatestPrice() public view returns (int) {
        (
            /*uint80 roundID*/,
            int price,
            /*uint startedAt*/,
            /*uint timeStamp*/,
            /*uint80 answeredInRound*/
        ) = priceFeed.latestRoundData();
        return price;
    }
}
```

## Importing from NPM/Github

https://github.com/smartcontractkit/chainlink/blob/develop/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol

Interfaces don't have full function implementations

```
//SPDX-Lincense-identifier: MIT

  

pragma solidity >=0.6.6 <0.9.0;

  

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

  

contract FundMe {

  

mapping(address => uint256) public addressToAmountFunded;

function fund() public payable {

addressToAmountFunded[msg.sender] += msg.value;

// what the ETH -> USD conversion rate

}

}
```
2:41

https://docs.chain.link/docs/ethereum-addresses/

Tuple: 
A list of objects of potentially different types whose number is a constant at compile-time.

```solidity
//SPDX-Lincense-identifier: MIT

  

pragma solidity >=0.6.6 <0.9.0;

  

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

  

contract FundMe {

  

mapping(address => uint256) public addressToAmountFunded;

function fund() public payable {

addressToAmountFunded[msg.sender] += msg.value;

// what the ETH -> USD conversion rate

}

  

function getVersion() public view returns (uint256){

AggregatorV3Interface priceFeed = AggregatorV3Interface(0x8A753747A1Fa494EC906cE90E9f37563A8AF630e);

return priceFeed.version();

}

function getPrice() public view returns (uint256) {

AggregatorV3Interface priceFeed = AggregatorV3Interface(0x8A753747A1Fa494EC906cE90E9f37563A8AF630e);

(uint80 roundID,

int256 answer,

uint256 startedAt,

uint256 tupdateAt,

uint80 answeredInRound

) = priceFeed.latestRoundData();

return uint256(answer);

// 1,574,72852252
  

}

}
```

EZtools.Ink 2021-12-01 13:00:34

2021-12-01 13:06:47

C:\Program File\Amazon\Ec2ConfigService\Settings

2021-11-30 10:56:19


