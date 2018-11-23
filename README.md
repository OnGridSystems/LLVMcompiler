# Solidity to LLVM-IR compiler @SerialHack hackathon.
This research and code development were made at [SerialHack](https://serialhacking.io) in November 2018. 
Everything under this repo is made just for **research and illustration** purposes, don't use it in production systems.

## Abstract
The goal of this project is to enable Solidity high-level code to be compiled by the standard LLVM toolchain to standard low-level representation (LLVM-IR) to be consumed by any of the LLVM backends.

## Considerations
Reference solc compiler has the alternative low-level lisp-like language called LLL, which can be directly compiled to EVM bytecode.
This language much simpler than Solidity, because of its lisp nature, simple structure and data types. Therefore it can be much more easily compiled into LLVM intermediate representation. But we identified several issues with taking it as a source for transcompiling:
* LLL oriented for very specific EVM machine exposing its complexity (stack engine, very specific opcodes, contract deployment process).
* transcompiling from LLL despite its relative simplicity have some syntax sugar and  needs to write our own lexer and parser for LISP language, which is rare enough and has no useful libs and tooling;

So we decided to reuse solc's own lexer, parser code, and AST tree but to compile directly to LLVM-IR instead of EVM bytecode.

## Results description
In this project, we implemented a compilation of a small subset of LLL. It can compile nested s-expressions with addition and subtraction operations. The result code can be compiled in any LLVM backend or ran via `lli`.
    
## Installing
### Using docker
```
docker pull 
docker run 
```
### Build from source
LLVM should be installed locally. 
If you using MacOS, install LLVM via homebrew and set `LLVM_DIR` env variable before solidity installing.
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
## Further Steps and Problems to solve
The first problem which should be solved is storing smart-contract code in a blockchain and decoupling contract creation (construct) part from the smart-contract body. 
At EVM this problem solved by returning smart-contract bytecode at contract creation stage.
It is not clear how this will work with LLVM IR. Will the LLVM IR or some compiled code be stored in the blockchain?

The second one is a storage model. EVM has two separate zones: memory and storage. (не смог придумать какое-нибудь описание для каждой)
LLVM has only equal of memory and how storage model should be specified is not clear.

The third problem is the lack of ABI generation at LLL. Because of low-level nature of this language and missing of a conception of functions ABI generation becomes an issue.

The first step that we plan to do is an implementation of the rest language feature. Further, we want to debug and test ENS implementation.
