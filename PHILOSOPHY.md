# Project: Beagle
These design documents are my thoughts on designing a language that is:
- Flexible
    + Flexible type system
        * A type system that allows for 'type ignorance' which includes:
            - Null-Safety
            - Type category specification (class or struct)
            - Abstraction specification (abstract type, interface, or trait)
            - Absolute ignorance (no knowledge of an object's type whatsoever, usually found in FFI)
        * Ability to define a range of types
            - Classes for Class Inheritance typing
            - Structs for Data Composition typing
        * Ability to define a range of abstractions
            - Interface for purely abstract class based types
            - Traits for purely abstract struct based types
            - Abstract types for non-specific type abstractions
    + Functional and Declarative Paradigms
- Self Doumenting
    * Language design is centered around definitions
        - Any definable construct uses the `def` keyword, including, but not limited to:
            + Properties
                * `def val` or `def var`
            + Functions
                * `def fun`
            + Classes (and relevant constructs)
                * `def class`
                * `def interface`
            + Structs (and relevant constructs)
                * `def struct`
                * `def trait`
            + Threads
                * `def thread`
            + Tasks (Coroutines)
                * `def task`
- Highly Debuggable
    + Ability to create a debug executable which allows you to utilize *visual debuggers*
    + A built in logging system for customizable stacktrace printing
        * Log routes
        * Log levels
        * Log formats
- Highly Concurrent
    + Simple thread and task definitions
    + Simple management of threads and tasks
    + Async/await features using `suspend` functions and properties (WIP)
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