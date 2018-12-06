# LLL to LLVM-IR compiler

![Experimental](https://img.shields.io/badge/readiness-prototype-red.svg)

LLL Smart-contract language to LLVM compiler prototype. 

Created at SerialHack hackathon, Dec. 2018.

* [Presentation PDF](OnGrid_LLVM_to_IR_compiler.pdf)
* [Demo video 1](llvm_test.mov)
* [Demo video 2](eth_test.mov)
* [Youtube Live Stream](https://youtu.be/J0OPMLV2CAk?t=4261)

## Problem to solve

The blockchain ecosystem needs reusable, standard, compatible and pluggable stack consisting of:
- high-level smart-contract languages;
- compilers;
- virtual machines, interpreters and emulators;
- different tools like code analyzers and test frameworks

In legacy world we already have ecosystem-wide standard of compilers and processors - [LLVM](https://llvm.org). 

Decentralised state engines running on top of blockchains/DAGs are very specific compared to hardware machines. And 
high-level languages for smart-contracts development have a lot of functional gaps compared to general-purpose langs.
Solidity and EVM cannot be easy stretched to LLVM design.

## Research. Why LLL, not Solidity
 
The Solidity compiler (solc) was initially designed without keeping LLVM in mind and seemed to lack intermediate 
representation (IR) in its internal design. 
Solc has a pretty complex Lexer, Parser and AST but unfortunately we found its internal mechanics too complex to build 
some minimal viable implementation within hackathon time constraints.

As a more primitive and achievable task we took the alternative language - Lower-Level Lisp-like smart-contract 
lang called [LLL](https://lll-docs.readthedocs.io/en/latest/lll_introduction.html) which is much simpler than Solidity 
then added LLVM-IR output to the compiler.

LLLc still produces EVM bytecode by default in no flag given. But invoked with -l option it builds LLVM-IR consumable by
LLVM backends.

## Results
We implemented LLL to LLVM-IR compiler supporting nested s-expressions with addition and subtraction math. 
The result code can be compiled to any LLVM backend including native execution by [lli](https://releases.llvm.org/1.0/docs/CommandGuide/lli.html).
    
## Install
LLVM toolset should be installed locally. 
If you using MacOS, install LLVM via homebrew and set `LLVM_DIR` env variable before installing solidity.
```
export LLVM_DIR=/usr/local/Cellar/llvm/7.0.0/lib/cmake
```

[Official solidity guide](https://solidity.readthedocs.io/en/latest/installing-solidity.html#building-from-source)

## Run
In text editor create LLL contract source (only math operators and nesting supported)
```
(/ 10 (- 4 2))
```
Compile lll source to LLVM IR
```
lllc -l /path/to/lll/source.lll
```
Run LLVM IR code on CPU
```
lli /path/to/LLVM_IR/source.ll
```

## Verify
Compile the same source to EVM bytecode (without -l flag)
```
lllc /path/to/lll/source.lll
```
Run bytecode (deploy the contract) to EVM wia web3 and retrieve the contract source by address.

The results should be equal.

## Authors

* [Kirill Varlamov](https://github.com/ongrid), OnGrid Systems ([github](https://github.com/OnGridSystems), [site](https://ongrid.pro))
* [Roman Nesytov](https://github.com/profx5), OnGrid Systems ([github](https://github.com/OnGridSystems), [site](https://ongrid.pro))