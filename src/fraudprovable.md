# Fuel is Fraudprovable

UTXO fraudproofs where you can prove that a UTXO is double spending or spending a UTXO that doesn’t exist. 

Tx level fraudproof within the virtual machine execution. Fuel revised the instruction set so that you can execute a single instruction on the vm. I.e. the evm has self destruct or create ,single instructions that are expensive and complicated and dealing with these means a lot of complexities and you may not be able to execute one of these instructions or interpret one of these instructions in the evm. The FuelVM is more expressive i.e. you don’t need a create instruction, you can do all your create logic. Doing a copy of large memory chunks in evm is prohibitively expensive, bc you have to loop and copy words of memory. Enables things at the instructional level vs the application level. There are some weird design choices when designing a VM,
