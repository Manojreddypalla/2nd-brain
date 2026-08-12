# Writing C → Running C — Short Notes

### 1. Overall Pipeline

```text
C Source
  ↓
Preprocessor
  ↓
Modified C Source
  ↓
Compiler
  ↓
Assembly
  ↓
Assembler
  ↓
Object File
  ↓
Linker
  ↓
Executable
  ↓
Loader
  ↓
Program in Memory → Process
```

### 2. Stages

**Preprocessor**

```text
.c → .i
```

Handles preprocessing directives such as `#include`.

**Compiler**

```text
.i → .s
```

Converts C → Assembly.

- Understands C language semantics.
    
- **Operator precedence → Compiler**
    
- Most computationally complex stage among CALL.
    

**Assembler**

```text
.s → .o
```

Converts Assembly → Object file.

⚠️ **Does NOT produce executable.**

**Linker**

```text
.o + other object files/libraries → Executable
```

- Combines separately compiled object files.
    
- Resolves **external references/labels**.
    

**Loader**

```text
Executable → Memory
```

- Loads program into memory for execution.
    
- **Static data placement → Loader.**
    

### 3. Separate Compilation

```text
a.c → a.o
b.c → b.o
      ↓
    Linker
      ↓
  Executable
```

Example:

```text
a.c → calls sum()
b.c → defines sum()

a.o → reference to sum
b.o → definition of sum
       ↓
     Linker
```

### 4. Logical Address

Compiler cannot know the exact physical RAM location where the program will execute later.

```text
Logical / Relocatable Address
          ↓
    Actual memory location
```

**Logical address ≠ Physical address**

### 5. Program vs Process

```text
Program = executable code on disk

Process = executing program in memory
```

### 🔥 GATE Memory

```text
.c → .i       Preprocessor
.i → .s       Compiler
.s → .o       Assembler
.o → EXE      Linker
EXE → Memory  Loader
```

**Compiler = Understand C**  
**Assembler = Make Object**  
**Linker = Join**  
**Loader = Load**