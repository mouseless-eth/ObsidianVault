## A Playdate With The EVM
Notes from femboy.capital's article on the evm [A Playdate With The EVM](https://femboy.capital/evm-pt1)
#### VM execution loop
- Fetch the insturciton the PC points to 
- Execute the instruction
- If the instruction jumps, set the PC to the new target
- Otherwise, increment the PC

This is what the VM execution loop looks like in Geth
```go
pc = uint64(0)
for {
    op = contract.GetOp(pc)
    operation := in.cfg.JumpTable[op]
    res, err = operation.execute(&pc, in, callContext)
    switch {
    case err != nil:
        return nil, err
    case operation.reverts:
        return res, ErrExecutionReverted
    case operation.halts:
        return res, nil
    case !operation.jumps:
        pc++
    }
}
```

### EVM Nitty-Gritty Specifications
The EVM is a **Stack Based** virtual machine, and uses **Big Endian** byte ordering for instructions, memory, and input data. The *word size* of the EVM is **256 Bits** wide, or **32 Bytes**.

### Memory & Storage In The EVM
The EVM state components include
- **The Stack** -> with maximum 1024 elements. Each element is 256 bits (1 word wide)
- **Memory** (volatile memory) -> byte addressed linear memory
- **Storage** (persistent memory) -> key-value store of 256 bit to 256 bit pairs

![[state_components.png]]

###### EVM Memory
EVM memory can be though of as a **big array** (importance on linearity). It can be addresses (indexed into) at the byte level (return 8 bit values) or at the word level (return 256 bit values)

###### Account Storage
Account Storage can be though of like a **map** pairing 256bit keys with 256bit values (word to word). Each location in storage is 0 initialized.

##### EVM Storage/Memory overview
![[executionLoopOverview.png]]

### Gas
Each opcode requires gas to execute. Opcode's unit of gas depend on how computationally expensive they are. A list of all opcodes and their gas prices can be found [here](https://github.com/crytic/evm-opcodes)