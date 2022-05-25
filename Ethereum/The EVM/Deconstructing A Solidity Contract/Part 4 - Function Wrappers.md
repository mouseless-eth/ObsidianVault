## Part Four Function Wrappers
#### Stages of a function call
1) Execution starts at the dispatcher which tries to match the function identifier
2) Once the function identifer matches, execution jumps to the function wrapper
3) Function wrapper calls the function body
4) Funciton body returns the return value and then execution resumes where it left off in the function wrapper section
5) Return value is packed and then returned using the `return` opcode

A **function’s wrapper** is an intermediary that unpacks the calldata for a function’s body to use, routes execution to it, and then repacks whatever comes back for the user. This wrapper structure is there for all functions that are part of the public interface of a contract in Solidity.

This packing and unpacking is defined in [Ethereum's Application Binary Interface Specification](https://docs.soliditylang.org/en/v0.4.24/abi-spec.html) which specifies how incoming and outgoing arguments in function calls are encoded.

Incoming transaction data is unpacked for a function to consume, and the data produced by a function is repacked for the user via function wrappers.