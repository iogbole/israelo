---
layout: post
title:  "Symbolisation: Cracking the code with ELF and DWARF"
author: israel
categories: [ 'Product' ]
tags: [ product ]
image: https://github.com/user-attachments/assets/6cc09baf-b329-4ea5-92ba-7ef1ee1d59b1
date:   2024-06-28 15:01:35 +0300
description: "Symbolisation converts raw memory addresses into human-readable symbols. The roles of DWARF and ELF in the symbolisation process is explored." 

---

#### Understanding DWARF and ELF

Before diving into symbolisation, let's clarify two fundamental concepts: DWARF and ELF.

-   **DWARF**: This is a standard for storing debugging information within executable files. It provides a detailed blueprint of the program's structure, including variable types, function names, and line numbers. Essentially, it's a map from machine code back to the original source code.
    
-   **ELF**: The Executable and Linkable Format (ELF) is the file format commonly used for executables, object files, and shared libraries. It encapsulates the machine code, along with metadata such as section headers and symbol tables. ELF provides the container for the DWARF information.
    

#### The Art of Symbolisation

Symbolisation is the process of converting raw memory addresses (often hexadecimal) into human-readable symbols. These symbols represent function names, file paths, and line numbers. This transformation is crucial for understanding the behavior of a program, especially when analyzing performance metrics or debugging issues.

#### Why Symbolisation Matters

-   **Performance Profiling**: By symbolising stack traces, developers can pinpoint which functions are consuming the most CPU or memory resources. This information is invaluable for optimization.
    
-   **Debugging**: When a program crashes or behaves unexpectedly, symbolisation helps identify the exact code location causing the issue.
    
-   **Incident Analysis**: In production environments, symbolising crash dumps or error logs aids in understanding the root cause of problems.
    

#### The Challenge of Symbol-Less Profiling

Traditionally, profiling required deploying applications with debug symbols, which can increase binary size and potentially impact performance. However, modern profiling tools have overcome this limitation by enabling symbolisation after the fact.

#### How Does it Work?

1.  **Data Collection**: Profiling data is collected without symbols. This usually involves capturing stack traces and performance metrics.
2.  **Upload**: The collected data is uploaded to a central repository or analysis platform.
3.  **Symbol Matching**: The platform attempts to match the memory addresses in the stack traces with symbols from available debug information.
4.  **Symbolisation**: Once a match is found, the addresses are replaced with human-readable symbols.
5.  **Analysis**: The symbolised data can now be analyzed to identify performance bottlenecks or other issues.

### Practical Example: Profiling a Go Application

Imagine profiling a Go application in production without debug symbols. The profiler collects performance data, including stack traces with memory addresses. This data is then uploaded to a profiling platform. The platform attempts to match the addresses against the symbols of the deployed binary or from a symbol server. Once symbolised, the profiler can generate reports showing which Go functions are consuming the most CPU time or memory.

By understanding the fundamentals of DWARF, ELF, and symbolisation, developers can effectively leverage profiling tools to optimize their applications and troubleshoot issues efficiently.

### Real-World Examples

#### Example of a Symbolised Stack Trace

A symbol in profiling is a combination of a file name, function name, and line number, uniquely identifying a line of code. A stack trace is a list of these symbols, representing a sequence of function calls ending in the currently executing function. Each line in this list is a stack frame.

scss

Copy code

```cpp
(fileA, function1, 10) -> 
(fileB, function2, 20) -> 
(fileC, function3, 30) -> 
(fileD, function4, 40)
```

This stack trace indicates that `function4` in `fileD` at line 40 called `function3` in `fileC` at line 30, and so on.

#### Example of an Unsymbolised Stack Trace

rust

Copy code

```cpp
345223 -> 
232678 -> 
785645 -> 
898902
```


These are raw memory addresses that need to be translated into meaningful symbols.

### Symbolisation: Bridging the Gap

Profilers collect these unsymbolised stack traces from native code programs and then use backend processes to associate the correct symbols, a process known as "symbolisation."

Tools like `addr2line` are often used to perform this task. Here's a basic example:

bash

Copy code

`addr2line -e my_program 345223` 

This command would attempt to translate the address `345223` into a symbol using the information in the executable file `my_program`. The output might look something like:

makefile

Copy code

`my_program.c:123` 

Indicating that the address corresponds to line 123 in the file `my_program.c`.

### Conclusion

Understanding DWARF, ELF, and the process of symbolisation is crucial for developers aiming to optimize application performance and troubleshoot issues efficiently. By leveraging these tools and techniques, one can bridge the gap between machine-level execution and human-readable code, enabling more effective debugging and performance analysis.