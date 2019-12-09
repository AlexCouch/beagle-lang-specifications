# Project: Beagle
These design documents are my thoughts on designing a language that is:
- Flexible
    + Flexible type system
        * Both dynamic and static type checking
        * Null-Safety
        * Simple polymorphic programming
        * Ability to define a range of types
            - Classes for Class Inheritance typing
            - Structs for Data Composition typing
    + Functional and Declarative Paradigms
- Self Doumenting
    * Utilizes a state based compiler
        - Definitions being a major part of this language
        - Any definable construct uses the `def` keyword, including, but not limited to:
            + Properties
            + Functions
            + Classes (and relevant constructs)
            + Structs (and relevant constructs)
            + Threads
            + Tasks (Coroutines)
- Highly Debuggable 
    + Compiler creates a *global call stack*
    + Raising exceptions outputs the entire *global call stack* rather than a *local call stack*
    + A built in logging system for customizable stacktrace printing
        * Log routes
        * Log levels
        * Log formats
- Highly Concurrent
    + Simple thread and task definitions
    + Simple management of threads and tasks
    + Async/await features using `suspend` functions and properties
- Highly Portable/Modular
    + Clean build system, similar to Rust's build system and module support
    + Cross-compiling
        * x86
            - Windows
            - Mac
            - Linux
            - Android
        * ARM-based CPU's
            - Raspberry Pi
            - iOs
        * Pretty much everything LLVM supports
        * Maybe even impractical architectures like 6502
    + Clean VCS Integration
        * VCS scripts for detailing module dependencies
            - Module specific repositories
            - Module specific branches
            - Module branch dependencies
                + Specify a module's branch depends on the state of another module's branch, like a parent module