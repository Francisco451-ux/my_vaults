# Types in solidity:
[Types](https://docs.soliditylang.org/en/v0.8.16/types.html)


```solidity


pragma solidity ^0.6.0;

  

contract SimpleStorage {

	uint256 favoriteNumber = 5;

	bool favoriteBool = true;

	string favoriteString = "String";

	int256 favoriteInt = -5;

	address favoriteAddress = 0x26146f3C0855Fc9877E2fE6778f2A16410aB9425;

	bytes32 favoriteBytes = "cat";

}
```


# **Functions**
## Visibility and Getters
[visibility](https://docs.soliditylang.org/en/v0.8.4/contracts.html)

view,pure 

1:44:13

```solidity

pragma solidity ^0.6.0;

  

contract SimpleStorage {

  
  

// this will get initalized to 0!

	uint256 public favoriteNumber;
	
	  
	
	function store(uint256 _favoriteNumber) public {
	
	favoriteNumber = _favoriteNumber;
	
	}
	
	//view,pure
	
	function retrieve() public view returns(uint256) {
	
	return favoriteNumber;

  
	function retrieve12(uint256 favoriteNumber) public pure {
	
	favoriteNumber + favoriteNumber;
	
	}
}

  

}
```

## struct

```
pragma solidity ^0.6.0;

  

contract SimpleStorage {

  
  

// this will get initalized to 0!

uint256 public favoriteNumber;

  

struct people{

uint256 favoriteNumber;

string name;

}

  

people public person = people({favoriteNumber: 2, name:"Patrick"});

  

function store(uint256 _favoriteNumber) public {

favoriteNumber = _favoriteNumber;

}

//view,pure

function retrieve() public view returns(uint256) {

return favoriteNumber;

  

}

```

## Arrays/Strings

**memory:**
Data will only be stored during the execution of the function

**storage**:
Data will  be stored after the execution of the function

```
pragma solidity ^0.6.0;

  

contract SimpleStorage {

  
  
	
	// this will get initalized to 0!
	
	uint256 public favoriteNumber;
	
	bool favoriteBool;
	
	  
	  
	
	struct People{
	
	uint256 favoriteNumber;
	
	string name;
	
	}
	
	  
	
	People[] public people;
	
	  
	
	//people public person = people({favoriteNumber: 2, name:"Patrick"});
	
	  
	
	function store(uint256 _favoriteNumber) public {
	
	favoriteNumber = _favoriteNumber;
	
	}
	
	//view,pure
	
	function retrieve() public view returns(uint256) {
	
	return favoriteNumber;
	
	  
	
	}
	
	  
	
	function addPerson(string memory _name, uint256 _favoriteNumber) public{
	
	people.push(People(_favoriteNumber, _name));
	
	}

  

}
```

**Mappings:**
A dictionary like data structure, with 1 value per key

2:00:17
```

//SPDX-Lincense-identifier: MIT

  

pragma solidity ^0.6.0;

  

contract SimpleStorage {

  
  

// this will get initalized to 0!

uint256 public favoriteNumber;

bool favoriteBool;

  
  

struct People{

uint256 favoriteNumber;

string name;

}

  

People[] public people;

mapping(string => uint256) public nameRoFavoriteNumber;

  

//people public person = people({favoriteNumber: 2, name:"Patrick"});

  

function store(uint256 _favoriteNumber) public {

favoriteNumber = _favoriteNumber;

}

//view,pure

function retrieve() public view returns(uint256) {

return favoriteNumber;

  

}

  

function addPerson(string memory _name, uint256 _favoriteNumber) public{

people.push(People(_favoriteNumber, _name));

nameRoFavoriteNumber[_name]= _favoriteNumber;

}

 

}
```