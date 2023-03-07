## Factory Pattern 

Interacting with Contract Deploed Contract

Recap
```

//SPDX-Lincense-identifier: MIT

  

pragma solidity ^0.6.0;

  

import "./SimpleStorage.sol";

  

contract StorageFactory is SimpleStorage {

SimpleStorage[] public simpleStorageArray;

  

function createSimpleStorageContract() public {

SimpleStorage simpleStorage = new SimpleStorage();

simpleStorageArray.push(simpleStorage);

  

}

  

function sfStore(uint _simpleStorageIndex, uint _simpleStorageNumber) public {

//address

//ABI

//SimpleStorage simpleStorage = SimpleStorage(address(simpleStorageArray[_simpleStorageIndex]));

//simpleStorage.store(_simpleStorageNumber);

return SimpleStorage(address(simpleStorageArray[_simpleStorageIndex])).store(_simpleStorageNumber);

}

  

function sfGet(uint256 _simpleStorageIndex) public view returns (uint256) {

//SimpleStorage simpleStorage = SimpleStorage(address(simpleStorageArray[_simpleStorageIndex]));

// return simpleStorage.retrieve();

return SimpleStorage(address(simpleStorageArray[_simpleStorageIndex])).retrieve();

}

  

}

```