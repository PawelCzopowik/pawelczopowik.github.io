# C# V8 Book notes


# CHAPTER 1 Introducing .NET Core

## Blocks of the .NET Core Platform (.NET core runtine environment)
- CoreCLR - Common Language Runtime, 
- CoreFX - .NET libraries, mostly platform agnostic
- CTS - Common Type System, describes all possible data types and all programming constructs
- CLS - Common Language Specification (for compliance)


---
- .NET Core binaries do not contain platform-specific instructions but rather platform-agnostic Intermediate Language (IL) and type metadata.
- .NET Core projects are always compiled to a file with a .dll extension, even if the project is an executable.
- In addition to CIL instructions, a .NET Core assembly contains full, complete, and accurate metadata, which describes every type defined in the binary, as well as the members of each type.
- Assembly also has metadata about itself.

## Understanding the Common Type System
- CTS Class Types
- CTS Interface Types
- CTS Structure Types
- CTS Enumeration Types
- CTS Delegate Types (Function pointers)
- CTS Type Members
- Intrinsic CTS Data Types
  - System.UInt64, System.String, System.Boolean
