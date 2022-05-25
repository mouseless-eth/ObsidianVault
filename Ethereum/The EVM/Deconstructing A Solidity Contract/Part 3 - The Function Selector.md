## Part Three The Function Seletor
Follow the tutorial [here](https://blog.openzeppelin.com/deconstructing-a-solidity-contract-part-iii-the-function-selector-6a9b6886ea49/)

Function Selector sits at the start of the runtime bytecode and matches the calldata's function selector to find the right location to jump to. If the calldata bytesize is less than 4, it will jump to the fallback function (if it is present, otherwise revert).

