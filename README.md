# Compiler Design Notes

## 1. Introduction to Compilers
* **Compiler:** A translator that converts pure High-Level Language (HLL) into Low-Level Language (LLL).
    * **Flow:** HLL -> [Compiler] -> LLL
    * **Examples:** Turbo C++, Dev C++, JavaC, Python.
    * **Fact:** Fortran was the first compiler ever built.
* **Machine Language:** * Binary
    * Assembly
* **Translator:** Software that translates from one form of language to another.
    * **Types:** Compiler, Assembler.
* **Assembler:** A translator that converts Assembly language into Binary/Object code.
    * **Flow:** Assembly -> [Assembler] -> Binary
    * **Note:** Assembly language is sometimes referred to as Low-Level Language.

---

## 2. Code Execution Flow
How code moves from source to execution:
1.  **Input HLL**
2.  **Preprocessing:** Handles any statement starting with `#`.
3.  **Pure HLL**
4.  **Compiler**
5.  **LLL**
6.  **Assembler**
7.  **Binary**
8.  **Linker**
9.  **Executable (.exe file)**

---

## 3. Phases of Compilation
The compilation process follows these sequential steps:

1.  **Lexical Analysis:** Scans code and divides it into a **Stream of Tokens**.
    * *Example:* `int a = 10;` becomes `keyword`, `variable`, `operator`, `constant`, `operator`.
2.  **Syntax Analysis:** Verifies grammatical mistakes using **Context-Free Grammar (CFG)**. Generates a **Parse Tree**.
3.  **Semantic Analysis:** Performs **Type Checking** to verify the meaning of sentences (e.g., checking if operators have the correct operand types). Generates an **Annotated Parse Tree**.
4.  **Intermediate Code Generation:** Converts source code into an intermediate representation for platform independence. Generates **3 Address Code**.
5.  **Code Optimization:** Reduces the number of instructions without changing the outcome.
    * *Types:* Machine-dependent and Machine-independent.
6.  **Target Code Generation:** Converts optimized code into **Assembly/Binary Code**.

---

## 4. Components of a Compiler

### Symbol Table
An abstract data type used to store complete information about the source code.
* **Interactions:** Initiated and first interacted with by Lexical Analysis.
* **Memory:** The compiler is responsible for providing memory to the Symbol Table.
* **Information Stored:** Variable Name, Type, Scope/Lifetime, Address, Number of Functions.
* **Implementation:** Array, Linked List, Hashmap, Tree, List.
* **Functions:** `Lookup()`, `Insert()`, `Delete()`, `Modify()`, `Update()`.

### Error Handler
A module responsible for identifying different types of errors across various phases of the compiler.

### Lexical Analyzer (Lexer)
* **Role:** Converts Pure HLL into a Stream of Tokens (logical groupings of characters).
* **Lexeme:** The actual representation of a stream of tokens.
* **Construction Steps:** 1. Define rules based on input stream.
    2. Construct Regular Expressions for rules.
    3. Convert Regular Expressions into Finite Automata.
* **Tools:** Hand Code or Lex.

---

## 5. Parsing and Grammars

### Key Definitions
* **Parsing:** Identifying rules to generate a string from a grammar.
* **Parse Tree:** A tree-based representation of the derivation of a string.

### Types of Grammar
| Category | Types |
| :--- | :--- |
| **Ambiguity** | **Ambiguous** (Multiple parse trees) \| **Unambiguous** (One parse tree) |
| **Recursion** | **Left Recursion** (Variable on LHS matches start of RHS) \| **Right Recursion** (Variable on LHS matches end of RHS) |
| **Determinism**| **Deterministic** (No common prefix) \| **Non-Deterministic** (Common prefix exists) |

---

## 6. Parsers

### Top-Down Parser
* Generates a parse tree from the root node down to the children.
* Starts with a start symbol and ends with a string using **Left Most Derivation**.
* Requires grammar to be free from **Ambiguity** and **Left Recursion**.
* Complexity: $O(n^4)$.
* **Types:**
    1.  **With Backtracking:** (Brute Force Method) - Tries all alternatives until a match is found.
    2.  **Without Backtracking:** (Predictive Parser) - Includes **LL(1) Parser** and **Recursive Descent Parser**.

---

## 7. LL(1) Parsing Details
**LL(1)** stands for: **L**eft-to-right scan, **L**eftmost derivation, **1** symbol lookahead.

### Components
* **Input Buffer:** Holds the input string; divided into cells for symbols.
* **Tape Header:** Points to the "lookahead symbol" and moves left to right.
* **Parse Stack:** Stores grammar symbols, terminals, and the dollar sign ($).
* **Parse Table:** A 2D array $[Non-Terminals \times Terminals]$.

### LL(1) Algorithm
1.  Push the start symbol onto the stack.
2.  Let `x` be the top of the stack and `a` be the lookahead symbol.
3.  If `x = a = $`, parsing is successful.
4.  If `x = a != $`, pop the stack and advance the input pointer.
5.  If `x` is a non-terminal and `M[x, a]` contains a production `x -> UVW`, replace `x` on the stack with `W V U` (reversed order).

### Table Generation Functions
* **First():** The set of terminals that can begin a string derived from a non-terminal.
* **Follow():** The set of terminals that can appear immediately to the right of a non-terminal.

### Parse Table Construction Rules
For every production $X \rightarrow Y$:
1.  Add $X \rightarrow Y$ to $M[X, a]$ for every terminal `a` in **First(Y)**.
2.  If **First(Y)** contains $\epsilon$ (epsilon), add $X \rightarrow Y$ to $M[X, b]$ for every terminal `b` in **Follow(X)**.