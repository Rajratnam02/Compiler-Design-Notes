# Compiler Design Notes

---

# Compiler

- A compiler is a translator which translates pure High Level Language (HLL) into Low Level Language (LLL).

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

- A translator is a software that translates from one form of language to another form of language.

## Types of Translators
- Compiler
- Assembler

---

# Assembler

- It is a translator that translates assembly language into binary/object code.
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

- Scan entire source code.
- Divide into stream of tokens.
- Input: Source code
- Output: Stream of tokens

Example:

```
int a = 10;
keyword  variable  operator  constant  operator
```

---

# Syntax Analysis

- Verify grammatical mistakes of source code.
- Language must be defined by Context Free Grammar (CFG).
- Input: Stream of tokens
- Output: Parse Tree

---

# Semantic Analysis

- Verify meaning of each sentence using type checking.
- Syntax checks structure only.
- Semantic checks correctness of types.

---

# Intermediate Code Generation

- Convert source code into Intermediate Representation (IR).
- Achieves platform independence.

---

# Code Optimization

- Reduce number of instructions without affecting output.
- Types:
  1. Machine Dependent Optimization
  2. Machine Independent Optimization

---

# Target Code Generation

- Convert optimized intermediate code into assembly/machine code.

---

# Symbol Table

- Abstract data structure used to store information about identifiers.
- Initiated during Lexical Analysis phase.
- Used and updated by later phases.
- Compiler provides memory for symbol table.

## Information Stored

- Name of variable
- Type
- Scope/Lifetime
- Address
- Number of functions

## Implementation

- Array
- Linked List
- Hash Map
- Tree
- List

## Functions

- Lookup()
- Insert()
- Delete()
- Modify()
- Update()

---

# Error Handler

- Module responsible for identifying different types of errors in various compiler phases.

---

# Lexical Analyser (Lexer)

- Also known as Lexer.
- Input: Pure HLL
- Output: Stream of tokens

```
Source Code → [Lexer] → Stream of Tokens
```

## Token
- Group of characters having logical meaning.

## Lexeme
- Actual representation of a token.

---

# Construction of Lexer

1. Define rules for input stream.
2. Construct Regular Expressions.
3. Convert Regular Expressions into Finite Automata.

## Tools
1. Hand Coding
2. Lex Tool

---

# Parsing

- Process of identifying rules to generate a given string from a grammar.

---

# Parse Tree

- Tree representation of derivation of a string from a grammar.

---

# Types of Grammar

## On the Basis of Ambiguity

### Ambiguous Grammar
- More than one parse tree for a given string.

### Unambiguous Grammar
- Only one parse tree for a given string.

---

## On the Basis of Recursion

### Left Recursion
- A → Aα

### Right Recursion
- A → αA

---

## On the Basis of Determinism

### Deterministic Grammar
- Grammar without common prefix.

### Non-Deterministic Grammar
- Grammar with common prefix.

---

# Parser

## 1. Top-Down Parser

- Generate parse tree from root to leaves.
- Start from start symbol.
- Uses Leftmost Derivation.
- Grammar must be free from ambiguity and left recursion.
- Used for less complex grammars.
- Time Complexity: O(n⁴)

### Types
1. With Backtracking (Brute Force Method)
2. Without Backtracking (Predictive Parser)
   - LL(1) Parser
   - Recursive Descent Parser

---

## 2. Bottom-Up Parser

- Build parse tree from leaves to root.

---

# Brute Force Method

- Expand non-terminal with first alternative.
- If mismatch, try next alternative.
- If any alternative matches → Successfully parsed.
- Else → Parsing fails.

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
- Pointed symbol is called Lookahead Symbol.
- Moves from left to right.

---

# Parse Stack

- Contains grammar symbols including terminals, non-terminals, and `$`.
- Symbols are pushed or popped depending on matching.

---

# Parse Table

- 2-D array of size n × (m+1)
  - n = number of non-terminals
  - m = number of terminals
- Contains productions used to derive the string.

---

# Parsing Process

1. Push start symbol into stack.
2. Compare top of stack with lookahead symbol.
3. If match → pop and move input pointer.
4. If not match → use parse table production and push RHS in reverse order.
5. Output productions used.

---

# LL(1) Parsing Algorithm

Let:
- x = top of stack
- a = lookahead symbol

- If x = a = $ → Parsing successful
- If x = a ≠ $ → Pop stack and move input pointer
- If x ≠ a and m[x,a] = X → UVW  
  Replace X with UVW in reverse order

Output all productions used.

---

# LL(1) Grammar

- Grammar used to construct LL(1) parser.
- Parse table must not contain multiple entries in any cell.

---

# FIRST()

- Set of terminals that begin strings derived from α.

## Rules

1. If a is terminal → FIRST(a) = {a}
2. If A derives ε → ε ∈ FIRST(A)
3. If A → XYZ
   - If X ≠ ε → FIRST(A) = FIRST(X)
   - If X = ε → FIRST(A) = FIRST(X) ∪ FIRST(Y)
   - Continue if multiple ε

---

# FOLLOW()

- Set of terminals that appear immediately to the right of A.

## Rules

1. If A is start symbol → FOLLOW(A) = {$}
2. If S → aAB and B ≠ ε → FOLLOW(A) = FIRST(B)
3. If S → aA → FOLLOW(A) = FOLLOW(S)

---

# Construction of LL(1) Parse Table

For every production:

```
X → Y
```

### Step 1

For every terminal `a ∈ FIRST(Y) − {ε}`:

```
M[X, a] = X → Y
```

### Step 2

If `ε ∈ FIRST(Y)`:

For every `b ∈ FOLLOW(X)`:

```
M[X, b] = X → Y
```

If `$ ∈ FOLLOW(X)`:

```
M[X, $] = X → Y
```

---

## Important

- If any cell contains multiple entries → Grammar is NOT LL(1).
- FIRST decides placement.
- FOLLOW is used only when ε is present.