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
