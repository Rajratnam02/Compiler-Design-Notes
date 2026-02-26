# Compiler Design Notes

---

# Compiler

- A **compiler** is a translator that converts pure High Level Language (HLL) into Low Level Language (LLL).
- Flow:
  
  ```
  HLL → [Compiler] → LLL
  ```

- Examples: Turbo C++, Dev C++, JavaC, Python
- FORTRAN was the first compiler ever built.

---

# Machine Language

- Binary
- Assembly

---

# Translator

- A translator is a software that converts one form of language into another form.

## Types of Translators
- Compiler
- Assembler

---

# Assembler

- A translator that converts **assembly language** into **binary/object code**.
- Assembly language is sometimes called low level language.

```
Assembly → [Assembler] → Binary
```

---

# How a Code is Executed?

```
Input HLL 
→ [Preprocessing] 
→ Pure HLL 
→ [Compiler] 
→ LLL 
→ [Assembler] 
→ Binary 
→ [Linker] 
→ Executable (.exe file)
```

- Any statement starting with `#` is used for preprocessing.

---

# Phases of Compilation

```
Lexical Analysis 
→ Syntax Analysis 
→ Semantic Analysis 
→ Intermediate Code Generation 
→ Code Optimization 
→ Target Code Generation
```

Detailed Flow:

```
Pure HLL
→ [Lexical Analysis] → Stream of Tokens
→ [Syntax Analysis] → Parse Tree
→ [Semantic Analysis] → Annotated Parse Tree
→ [Intermediate Code Generation] → 3 Address Code
→ [Code Optimization] → Optimized 3 Address Code
→ [Target Code Generation] → Binary Code
```

---

# Lexical Analysis

- Scans entire source code.
- Divides it into stream of tokens.
- Input: Source Code
- Output: Stream of Tokens

Example:

```
int a = 10;
keyword  variable  operator  constant  operator
```

---

# Syntax Analysis

- Verifies grammatical correctness of source code.
- Language must be defined using Context Free Grammar (CFG).
- Input: Stream of Tokens
- Output: Parse Tree

---

# Semantic Analysis

- Verifies meaning of each statement.
- Performs type checking.
- Syntax checks structure only.
- Semantic checks correctness of types.

---

# Intermediate Code Generation

- Converts source code into intermediate representation (IR).
- Makes code generation easier.
- Provides platform independence.

---

# Code Optimization

- Reduces number of instructions without changing output.
- Types:
  1. Machine Independent Optimization
  2. Machine Dependent Optimization

---

# Target Code Generation

- Converts optimized intermediate code into assembly/machine code.

---

# Symbol Table

- An abstract data structure used to store information about identifiers.
- Initiated during Lexical Analysis phase.
- Used and updated by later phases.
- Compiler manages memory for symbol table.

## Information Stored

- Name of variable
- Type
- Scope / Lifetime
- Address
- Number of functions

## Implementation Methods

- Array
- Linked List
- Hash Map
- Tree
- List

## Operations

- Lookup()
- Insert()
- Delete()
- Modify()
- Update()

---

# Error Handler

- Module responsible for detecting and reporting errors in different phases of compilation.

---

# Lexical Analyser (Lexer)

- Also known as Lexer.
- Input: Pure HLL
- Output: Stream of Tokens

```
Source Code → [Lexer] → Stream of Tokens
```

## Token
- Group of characters with logical meaning.

## Lexeme
- Actual sequence of characters that matches a token pattern.

---

# Construction of Lexer

1. Define rules for input stream.
2. Create patterns using Regular Expressions.
3. Convert Regular Expressions to Finite Automata.

## Tools for Lexer Construction
1. Hand Coding
2. Lex Tool

---

# Parsing

- Process of determining whether a string can be generated from a given grammar.

---

# Parse Tree

- Tree representation of derivation of a string from a grammar.

---

# Types of Grammar

## 1. On the Basis of Ambiguity

### Ambiguous Grammar
- A string can have more than one parse tree.

### Unambiguous Grammar
- A string has exactly one parse tree.

---

## 2. On the Basis of Recursion

### Left Recursion
- A → Aα

### Right Recursion
- A → αA

---

## 3. On the Basis of Determinism

### Deterministic Grammar
- Grammar without common prefix.
- No backtracking required.

### Non-Deterministic Grammar
- Grammar with common prefix.
- Backtracking required.

---

# Parser

## 1. Top-Down Parser

- Generates parse tree from root to leaves.
- Starts from start symbol.
- Uses Leftmost Derivation.
- Grammar must be free from ambiguity and left recursion.
- Suitable for less complex grammars.
- Time Complexity: O(n^4)

### Types

1. With Backtracking (Brute Force Method)
2. Without Backtracking (Predictive Parser)
   - LL(1) Parser
   - Recursive Descent Parser

---

## 2. Bottom-Up Parser

- Builds parse tree from leaves to root.

---

# Brute Force Method

- Expand non-terminal using first alternative.
- If it doesn't match input, try second alternative.
- Continue until match found.
- If no alternative matches → Parsing fails.

---

# LL(1) Parser

- L → Left to Right scanning
- L → Leftmost derivation
- (1) → One lookahead symbol

---

# Input Buffer

- Divided into cells.
- Each cell holds one symbol.
- Contains only one string at a time.

---

# Tape Header

- Points to one symbol at a time.
- The pointed symbol is called Lookahead Symbol.
- Moves from left to right.
