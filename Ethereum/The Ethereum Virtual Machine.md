## The Ethereum Virtual Machine
Theses are notes relating to chapter 13 of the *Mastering Ethereum Handbook*

### Contract Exectuion
1) Contract account's ROM is loaded
2) Program ocutner set to zero
3) Storage is loaded from contract's storage
4) Memory set to all zero's
5) Block and Environment variabels are set

OOG = Out Of Gas Exception
- Halt's operation and the transaction is abandoned
- No state changes are applied
- Sender's nonce is incremented
- Sender still has to pay gas fees (sender's balance goes down)

### Contract Deployment Code
When creating a contract, the transaction 
- **to** field is set to the **0x0** address
- **data** field is set to the `initiation code`

When a contract creation transaction is processed, an EVM is instantiated with the code in the **data** field loaded to the program code ROM, then the output of the EVM execution is taken and used as the *code for the new contract account*. 

##### Deployment ByteCode
`Deployment ByteCode`  is used for every aspect of the initialization of a new contract including the `Runtime ByteCode`, and the code that runs in the constructor.

To get the deployment bytecode, we run
```console
solc -bin Faucet.sol
```

##### Runtime ByteCode
`Runtime ByteCode` is the bytecode that ends up being executed whenever a transation is sent to the contract. 

`Runtime ByteCode` is a subset of the `Deployment ByteCode`

To get the deployment bytecode, we run
```console
solc -bin-runtime Faucet.sol
```

### Disassembling The Bytecode
```solidity
// Version of Solidity compiler this program was written for
pragma solidity ^0.4.19;

// Our first contract is a faucet!
contract Faucet {
  // Give out ether to anyone who asks
  function withdraw(uint withdraw_amount) public {

      // Limit withdrawal amount
      require(withdraw_amount <= 100000000000000000);

      // Send the amount to the address that requested it
      msg.sender.transfer(withdraw_amount);
    }

  // Accept any incoming amount
  function () external payable {}
}
```

When sending a transacion to a smart contract, the transaction interacts with the smart contract's **dispatcher**.

The **dispatcher** reads in the data field of the transaction and sends teh relevant part to the appropriate function

```
PUSH1 0x4      // Push 4 to the stack 
CALLDATASIZE   // Push calldata size to stack
LT             // Check if 4 is less than calldata size
PUSH1 0x3f
JUMPI
```

This checks if the transaction's `CALLDATA` is less than 4 bytes long.
If `CALLDATA` < 4bytes
	- Jump to fallback function (receive, fallback)
else 
	- Jump to function that is being called

Function identifiers are always 4 bytes in length. (if calldata size is less than 4 bytes we know that no function is being called)

`LT` pops the top two values from the stack, and if **CALLDATASIZE** is less than **0x4**, we push **1** onto the stack, otherwise we push **0**

`JUMPI` equates to `JUMPI(label, cond)` and jumps to `label` if `condition` is equal to **1**

##### Example : calling contract with no data field in tx
1) After the `PUSH1 0x4` instruction, our stack looks like
| Stack      | Decimal Value |
| ----------- | ----------- |
| 0x4 | 4|
2) After the `CALLDATASIZE` opcode, our stack looks like
| Stack      | Decimal Value |
| ----------- | ----------- |
| 0x0 | 0|
| 0x4 | 4|
3) After the `LT` opcode, our stack looks like
| Stack      | Decimal Value |
| ----------- | ----------- |
| 0x1 | 1|
4) After the `PUSH1 0x3f` opcode, our stack looks like
| Stack      | Decimal Value |
| ----------- | ----------- |
| 0x3f | 63|
| 0x1 | 1|
5) After the `JUMPI` opcode, our stack looks like
| Stack      | Decimal Value |
| ----------- | ----------- |

The program counter is set to `0x3f` (top of stack) because the second top stack element is equal to `1`. This location holds the instructions to the contract's fallback function.