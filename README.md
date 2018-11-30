# Solidity to LLVM-IR compiler

## Problem to solve

**Solidity on LLVM**
- Take Solidity as input
- Produce LLVM IR as output

**Specifications to conform**
- Solidity 0.5.0
- LLVM 7.0.

Take parsed solidity code, and bring it into an LLVM compatible format, such that LLVM IR can be generated and consumed by any of the LLVM backends."

## Research. Why LLL, not Solidity
 
The Solidity compiler (solc) was initially designed without keeping LLVM in mind and seemed to lack intermediate 
representation (IR) in its design. 
Solc has a pretty complex Lexer, Parser and AST but unfortunately we found its internal mechanics
too complex to build some minimal viable implementation within hackathon time constraints.
As more primitive and achievable task we took another lang as the source - lower-level lisp-like smart-contract 
language called [LLL](https://lll-docs.readthedocs.io/en/latest/lll_introduction.html) and made some minimal compiler runnable by LLVM-IR backend.

Reference solc library has the alternative language called LLL, directly compilable to EVM bytecode.
It's much simpler than Solidity, because of its lisp nature, simple structures and data types. Therefore it can be much 
more easily adapted to create LLVM intermediate representation. 
We identified several issues with taking it as a source for transcompiling:
* LLL oriented for very specific EVM machine exposing its complexity (stack engine, very specific opcodes, contract deployment process).
* transcompiling from LLL despite its relative simplicity have some syntax sugar and  needs to write our own lexer and parser for LISP language, which is rare enough and has no useful libs and tooling;
but Solidity still looked to be more painful to make something viable.

## Results
We implemented LLL to LLVM-IR compiler supporting nested s-expressions with addition and subtraction math. 
The result code can be compiled to any LLVM backend including native execution by [lli](https://releases.llvm.org/1.0/docs/CommandGuide/lli.html).
    
## Installing
LLVM toolset should be installed locally. 
If you using MacOS, install LLVM via homebrew and set `LLVM_DIR` env variable before installing solidity.
```
export LLVM_DIR=/usr/local/Cellar/llvm/7.0.0/lib/cmake
```

[Official solidity guide](https://solidity.readthedocs.io/en/latest/installing-solidity.html#building-from-source)

## Using
Compile lll source to LLVM IR
```
lllc -l /path/to/lll/source.lll
```
Run LLVM IR
```
lli /path/to/LLVM_IR/source.ll
```

Example LLL code which can be compiled
```
(/ 10 (- 4 2))
```

## Further ideas and Problems to solve

The first problem which should be solved is storing smart-contract code in a blockchain and decoupling contract creation (construct) part from the smart-contract body. 
At EVM this problem solved by returning smart-contract bytecode at contract creation stage.
It is not clear how this will work with LLVM IR. Will the LLVM IR or some compiled code be stored in the blockchain?

The second one is a storage model. EVM has two separate zones: memory and storage.
LLVM has only equal of memory and how storage model should be specified is not clear.

The third problem is the lack of ABI generation at LLL. Because of low-level nature of this language and missing of a conception of functions ABI generation becomes an issue.

The first step that we plan to do is an implementation of the rest language feature. Further, we want to debug and test ENS implementation.

## Authors

* [Kirill Varlamov](https://github.com/ongrid), OnGrid Systems ([github](https://github.com/OnGridSystems), [site](https://ongrid.pro))
* [Roman Nesytov](https://github.com/profx5), OnGrid Systems ([github](https://github.com/OnGridSystems), [site](https://ongrid.pro))