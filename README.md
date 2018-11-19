# Solidity to LLVM-IR compiler @SerialHack hackathon.
This research and code development were made at [SerialHack](https://serialhacking.io) in November 2018. 
Everything under this repo is made just for **research and illustration** purposes, don't use it in production systems.

## Abstract
The goal of this project is to enable Solidity high-level code to be compiled by the standard LLVM toolchain to standard low-level representation (LLVM-IR) to be consumed by any of the LLVM backends.

## Considerations
The reference feature-rich solidity compiler, solc doesn't follow LLVM spec, but it has internal compiler to another standardized intermediate representation called LLL which can be directly compiled to contract bytecode. Produced LLL is much simpler than Solidity, but we identified several issues with taking it as source for transcompiling:
* LLL is not so abstract as IR should be, and oriented for very specific EVM machine exposing its complexity (stack engine and very specific opcodes)
* transcompiling from LLL despite its relative simplicity needs to write our own lexer and parser for LISP language, which is rare enough and has no useful libs and tooling;
So we decided to reuse solc's own lexer, parser code and AST tree but to compile directly to LLVM-IR instead of LLL.
