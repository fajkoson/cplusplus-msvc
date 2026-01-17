# MSVC (cl.exe) C++ Compiler Options

This table explains **what each MSVC compiler option does**, **why it exists**,  
and **when you would normally use it**, even if you are new to C++ or MSVC.

---

## General / Driver Options

| Option | Explanation |
|------|-------------|
| `/?` | Prints all available compiler options and a short description of each. Useful when you want a quick overview directly from the compiler. |
| `/c` | Compiles source files into object files (`.obj`) but **does not link** them into an executable or DLL. Used when compilation and linking are done as separate steps. |
| `/nologo` | Hides the MSVC startup banner. Commonly used in build systems and CI to keep logs clean and readable. |
| `/MP[n]` | Enables parallel compilation using multiple CPU cores. This significantly reduces build times for large projects. Optional `n` limits the number of compiler processes. |
| `/FC` | Shows the full absolute path of source files in compiler error messages. Very helpful when multiple files have the same name in different directories. |
| `/FS` | Forces serialized access to PDB (debug symbol) files. Prevents build failures when many compiler instances write to the same PDB during parallel builds. |
| `/showIncludes` | Prints every header file included during compilation. Used mainly for dependency tracking and debugging include-related problems. |
| `/utf-8` | Tells the compiler that both source code and string literals are encoded in UTF-8. Strongly recommended for modern projects and cross-platform code. |
| `/source-charset:<set>` | Specifies the character encoding of source files (for example `utf-8` or `windows-1250`). Needed when source files are not UTF-8. |
| `/execution-charset:<set>` | Specifies the encoding used for string literals at runtime. Rarely needed unless dealing with legacy encodings. |
| `/volatile:iso` | Uses ISO C++ standard rules for `volatile`. This is safer and more portable, especially for cross-compiler projects. |
| `/volatile:ms` | Uses Microsoft-specific `volatile` behavior. Required only for legacy or Windows-specific low-level code. |

---

## Output Files / Build Artifacts

| Option | Explanation |
|------|-------------|
| `/Fo<file|dir>` | Specifies the output name or directory for generated object files (`.obj`). Useful for keeping build outputs organized. |
| `/Fe<file>` | Sets the name of the final executable file. Without this, MSVC uses the first source file name by default. |
| `/Fd<file>` | Specifies the name and location of the debug symbol file (`.pdb`). Important for debugging and crash analysis. |
| `/Fi<file>` | Writes preprocessed source output to a file. Used mainly for debugging macros and include problems. |
| `/Fa[<file>]` | Generates an assembly listing showing how C++ code is translated into assembly instructions. |
| `/FA` | Generates assembly output including original source code for easier reading. |
| `/FAs` | Generates assembly output including source code and machine instructions. |
| `/FAcs` | Generates the most detailed assembly output, including symbols and machine code. Mostly used for performance analysis. |
| `/Fp<file>` | Specifies the file name for precompiled headers. Used to speed up compilation of large projects. |

---

## Include Paths / Macros

| Option | Explanation |
|------|-------------|
| `/I <dir>` | Adds a directory to the list of locations searched for header files. Required when using external libraries or custom include paths. |
| `/X` | Disables all standard include directories. Used only in highly controlled or specialized build environments. |
| `/D<name>[=value]` | Defines a macro before compilation. Often used to enable features or select build configurations. |
| `/U<name>` | Removes a predefined macro. Used when a default macro interferes with your code. |
| `/FI <file>` | Forces a header file to be included in every compiled source file. Useful for common configuration or platform headers. |
| `/imsvc <dir>` | Marks headers in a directory as system headers, which reduces warning noise from external code. |
| `/external:I <dir>` | Treats headers in this directory as external dependencies, allowing warning control. |
| `/external:W0..W4` | Sets warning level for external headers independently from project code. |
| `/external:anglebrackets` | Treats headers included with `<...>` as external, reducing warnings from standard libraries. |

---

## Warnings / Diagnostics

| Option | Explanation |
|------|-------------|
| `/W0` | Disables all warnings. Not recommended except for temporary experiments. |
| `/W1` | Enables minimal warnings. May hide real problems. |
| `/W2` | Enables moderate warnings. |
| `/W3` | Default warning level used by Visual Studio. |
| `/W4` | Enables very strict warnings. Strongly recommended for serious projects. |
| `/Wall` | Enables almost all warnings, including many that are usually ignored. Mainly useful for audits, not daily builds. |
| `/WX` | Treats all warnings as errors. Forces clean, warning-free code. |
| `/WX-` | Disables treating warnings as errors. |
| `/we<number>` | Promotes a specific warning to an error. |
| `/wd<number>` | Disables a specific warning that is known and accepted. |
| `/wo<number>` | Shows a warning only once, reducing log spam. |
| `/diagnostics:classic` | Uses legacy MSVC diagnostic formatting. |
| `/diagnostics:column` | Shows column numbers for errors and warnings. |
| `/diagnostics:caret` | Shows a caret (`^`) pointing to the exact error location. |
| `/WL` | Outputs diagnostics on one line, useful for log parsers and CI systems. |
| `/permissive-` | Disables many Microsoft C++ extensions and enforces standard-compliant behavior. Highly recommended for portable code. |

---

## Language Standard / Mode

| Option | Explanation |
|------|-------------|
| `/TP` | Forces all input files to be treated as C++. |
| `/TC` | Forces all input files to be treated as C. |
| `/std:c++14` | Compiles code using the C++14 standard. |
| `/std:c++17` | Compiles code using the C++17 standard. |
| `/std:c++20` | Compiles code using the C++20 standard. |
| `/std:c++latest` | Enables the newest features implemented by MSVC, even if not standardized yet. |

---

## Exceptions / RTTI / Floating-Point

| Option | Explanation |
|------|-------------|
| `/EHsc` | Enables standard C++ exception handling. This is the recommended default for most applications. |
| `/EHs` | Enables exception handling but allows more compiler assumptions. |
| `/EHa` | Enables both C++ and Windows SEH exceptions. Required when catching hardware or OS exceptions. |
| `/EHc` | Assumes `extern "C"` functions do not throw exceptions, allowing better optimization. |
| `/GR` | Enables runtime type information (RTTI), required for `dynamic_cast` and `typeid`. |
| `/GR-` | Disables RTTI to reduce binary size when not needed. |
| `/fp:precise` | Balances floating-point performance and accuracy. Default choice for most applications. |
| `/fp:fast` | Allows aggressive floating-point optimizations that may slightly change results. |
| `/fp:strict` | Enforces strict IEEE floating-point rules, useful for scientific and financial code. |

---

## Optimization

| Option | Explanation |
|------|-------------|
| `/Od` | Disables optimizations, making debugging easier. |
| `/O1` | Optimizes for smaller binary size. |
| `/O2` | Optimizes for maximum execution speed. |
| `/Ox` | Enables the most aggressive optimizations available. |
| `/Ob0` | Disables function inlining. |
| `/Ob1` | Inlines only simple functions. |
| `/Ob2` | Enables aggressive inlining for performance. |
| `/Oi` | Replaces some function calls with faster inline instructions. |
| `/Ot` | Favors execution speed over binary size. |
| `/Os` | Favors smaller binaries over speed. |
| `/Oy` | Omits frame pointers, improving performance but making stack traces harder. |
| `/Oy-` | Keeps frame pointers for better debugging and profiling. |
| `/GL` | Enables whole-program optimization across all source files. |
| `/Gy` | Places each function in its own section, enabling better dead-code removal. |
| `/Gw` | Places global data in individual sections for better optimization. |
| `/GF` | Merges identical string literals to reduce memory usage. |
| `/GS` | Adds runtime checks to detect stack buffer overflows. |
| `/GS-` | Disables stack security checks (not recommended). |

---

*(… continues cleanly for CRT, Debug, Conformance, CPU, Security, Sanitizers — I can finish the remaining sections in the same style if you want.)*





