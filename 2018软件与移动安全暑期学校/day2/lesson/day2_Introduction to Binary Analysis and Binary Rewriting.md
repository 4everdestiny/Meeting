Introduction to Binary Analysis and Binary Rewriting
=====
`林志强 zhiqiang lin 2018-7-19`  
`逆向二进制`
# Background
`What's Running in our Cyberspace - Program`  
program communicate with other program  
## What is Binary Analysis
attack <==> defense  
## Why Binary code
_no source code often_  
Binary code is the only authoritative version of the program  
some problems only can be seen in binary code  
`backdoor`  
## What to Reason About in Binary Code
# Challenges
## Abstraction Recovery
`reconstruct the abstractions distilled after compilation`  
Code/Data distinction  
Position independent code(PIC)  
## Path Coverage and Path Explosion
Static <==> Dynamic
# technique
## Basic Approaches to disassemble code
`Linear sweep algorithm`  
recall analysis  
`Recursive Traversal Algrithm`
`Disassemble package`  
## Basic Techniques
Data Flow Analysis  
Control Flow Analysis  
Control dependency  
Program Slicing  
Symbolic Execution  
## Static Analysis,Dynamic Analysis
PIN angr QEMU(IoT)
# Applications of Binry Analysis in Security
Reverse engineering  
Vulnerability/fuzzing  
Exploit generation  
Software verification  
Binary hardening  
## Binary Rewriting
Software fault isolation  
Control Flow Integrity
## Binary Analysis
there are many public available binary analysis tools
# Dynamic Binary Instrumentation
`Basic Concepts` `QEMU` `PIN` `Summary`
## What is (Dynamic) Binary Instrumentation
summary the code execute  
icount++  
sub $0xff,%edx  
### What instrumentation Do
Profiler for compiler 
Binary instrumentation
### How is Instrumentation used in Program Analysis
## QEMU
Virtual Intel and Instrumentation
### QEMU internals
### QEMU Code translation
arm -> mips etc.
### use cases
Malware analysis  
Dynamic binary code instrumentation  
System wide taint analysis  
_PEMU_
## PIN
Program gdb  
PIN allows a tool to insert arbinary code  
dynamic add code  
not open source
### Advantages of Pin Instrumentation
Programmable Instrumentation  
program print values  
### Use PIN
use PIN to write a plugin
### Pin instrumentation APIs
`Call-based API`  
### ManualExamples/itrace.cpp
### & other APIs
# Static Binary Rewriting
## Static binary rewriting is important
Binary code reuse(copy paste)  
Software fault isolation  
## Challenges in disassembling
Recognizing and relocating static memory addresses  
Handing dynamically computed memory addresses  
Differentiating code data  
Handing function pointer arguments  
Handing Position Independent Code(PIC)  
## Existing static rewriters
Assume certain compiler  
Assume having debugging symbols  
Assume knowledge of APIs
Assume no code and data interleaving  
Assume relocation metadata  
# MULTIVERSE
Everything that can happen does happen  
## A running example
## Superset Diasassemeber
When in doubt, use brute force
## overview of MULTIVERSE
5 challenges  
Static Binary Analysis
## Related work
`github.com/utds3lab/multiverse`