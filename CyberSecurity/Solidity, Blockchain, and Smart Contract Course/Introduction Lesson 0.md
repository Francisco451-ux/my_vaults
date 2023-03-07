#### Intruduçao ao curso e sebente aqui:

**[full-blockchain-solidity-course-py](https://github.com/smartcontractkit/full-blockchain-solidity-course-py)**

adress: 0x37629fb772754eC306a6976eE9eFd86B1dc48b42
address_Avataris12: 0x26146f3C0855Fc9877E2fE6778f2A16410aB9425

**[Tool_Etherscan](https://etherscan.io/)** 

Cada conta tem a sua private key que permite so a acceder a essa conta especifica

Outra conta tera outra chave privada

Test Network
-Rinkeby
-coven


Ferramenta onde vamos fazer a nossa primeira trasaçao pra a rede de teste de eth:  [Facucet](https://faucet.rinkeby.io/)

Alternative Faucet: [https://faucets.chain.link/rinkeby](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0YxWmY3QkZ0bzJ6OXU3NUlEZzJYb1d0LWo4QXxBQ3Jtc0trRmRDQ1BEb0xhNVJXN0VEVk8teFdlZGZUT0hYRUJGeFIyU1dfMUtjblFadjlORE91RGZadUFxaEVCZUtUMlZTWEQ3Z3d0dmVJM1lybkx6T2poOUZ6UUdIc2tTTWxfdER2dGpSVzFxejJxcGZmeEtZTQ&q=https%3A%2F%2Ffaucets.chain.link%2Frinkeby&v=D7lQyANnXEU)


#### **Faucet**:
Is a `application that gives us free test tokens,`like free test Rinkeby Ethereum


#### **Block Explorer:**
An application that allows us to `view` **transaction that happen on blockchain**

#### **Gas:**
Gas is a uinit of computational measure.The more compution a transaction uses the more "gas" you have to pay for.

Every transaction that happens on-chain pays a "gas fee" to node operators

The amount of "gas" used and how much you pay depends on how "computationally expensive" your transaction is 

Sending ETH to 1 address would be "cheaper" than sending ETH to 1000 addresses

 ver quanto é gasto na rede:
[eth_converter]https://eth-converter.com/

Gas: Measure of computation use
Gas Price: How much it cost per unit of gas 
Gas Limit: Max amount of gas in a transaction

Transaction Fee: Gas Used * gas Price

ie: 21000 gas @ 1 GWEI per gas = 21000 GWEI

#### **Blockchain works**

**How blockchain works:[DEmo](https://andersbrownworth.com/blockchain/)**

Block:
![[Pasted image 20220820115859.png]]


**Hash que a blockchain use**: `keccak-256` e da familia sha256


**Genesis Block:**
The first block in a blockchain


#### Distributed Blockchain
![[Pasted image 20220820120740.png]]

Peer A
Peer B
Peer C

#### Tokens 
![[Pasted image 20220820120838.png]]

Peer A
Peer B
Peer C

**Mining:**
The process of finding the "solution" to the blockchain "problem".
In our example, the "problem" was to find a hash that starts with four zeros.
Nodes get paid for mining blocks

**Block:**
A list of transactions mined together.

**Decentralized:**
Having no single point of authority.

**Nonce:**
A "number used once" to find the "soluction" to the blockchain problem. 
It's also used to define the transaction number for an account/address.

**Private Key:**
Only know to the key holder,it's used to "sign" transactions

Algoritmo: Elliptic Curve Signature Algorithm
hash knowloge and  more

See your private key:
metamask -> AccountDetails -> Export Private Key 

**NOte**: when using a private key somewhere they want it in hexadecimal form.
Append `0x` in the begin of private key

OxPRIVATE_KEY

**Note**: Private key creates your public key which then can create your address
Private Key ||| -> public Key -> address


**Public Key:**
Is derived from your private key. Anyone can "see" it, and use it to verify that a transaction came from you

**Signing a trasaction:**
A "one way" process.Someone with a private key signs a transaction by their private key being hashed with their transation data.
Anyone can then verify this new transaction hash with your public key.

**Note**: Private Key -> public Key -> address

**Node:** 
A single instance in a decentralized network 

Blockchain nodes keep list of the transactions that occur

**Consensus:**
Is the mechanism used to agree on the state of a blockchain
1.Chain Selection
2.Sybil Resistance

1:13.00\

**Pow and Pos:**

**Proof of work(Pow)**

 **Nakamoto Consensus**
Proof of work network they're called miners and in the proof of stake network they're called validators

**Sharded** is the solution to this scalability problem

Layer 1: Base layer blockchain implementation

Layer2: Any application built on top of a layer2

-Chain link:
-optimism
-Arbitrarily

Rollups is kind of like a shareded chain derive their security from  a base layer from the layer one

side chains derive their security from their own protocols, roll ups derive their security from the base layers










