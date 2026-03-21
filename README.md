# Evalexpr - Mathematical Expression Evaluator

> **Privacy Note:** The source code of this project is kept private to comply with EPITA's anti-plagiarism regulations. The code is structured, commented, and available upon request for recruiters during an interview. This repository serves as a technical showcase and provides a testable compiled version.

## 📖 Project Context

**Evalexpr** is an algorithmic and data-structure project from the EPITA curriculum. The goal is to build a robust mathematical evaluator capable of parsing standard inputs (infix notation) and Reverse Polish Notation (RPN), computing the result securely, and exiting with precise POSIX return codes depending on the type of error encountered.

## ✨ Implemented Features

* **Infix Notation Parsing:** Evaluates standard mathematical strings like `3 + 4 * 2 / ( 1 - 5 ) ^ 2`.
* **Reverse Polish Notation (RPN):** Supports RPN parsing natively via the `-rpn` flag (e.g., `3 4 2 * + 1 5 - 2 ^ /`).
* **Supported Operators:** * Arithmetic: `+`, `-`, `*`, `/`, `%` (modulo), `^` (power).
    * Unary operators: Unary minus and unary plus.
    * Parentheses grouping: `(` and `)`.
* **Strict Error Handling:** * **Code 1:** Lexical/Syntax errors (unknown characters).
    * **Code 2:** Parsing errors (unbalanced parentheses, invalid RPN layout).
    * **Code 3:** Arithmetic constraints (division/modulo by zero, negative powers).
    * **Code 4:** CLI Argument errors.

## 🏗️ Technical Architecture

1. **Lexer:** Reads raw strings from `stdin` using `getline` and tokenizes digits and mathematical operators into a Linked-List based Queue (`struct queue`).
2. **Shunting-yard Algorithm (Parser):** If the input is in infix notation, a custom implementation of Dijkstra's Shunting-yard algorithm is used to reorder the tokens into RPN. It leverages an operator stack (`struct element *pile`) to resolve operator precedence.
3. **RPN Evaluator:** Iterates through the sanitized RPN queue. Pushes integers onto a stack and pops them upon encountering operators to compute the intermediate results safely.

## 🧠 Technical Challenges Solved

* **Operator Precedence:** Managing complex priorities (e.g., power `^` being right-associative and having higher precedence than multiplication) without building an Abstract Syntax Tree (AST), using strictly stack-based rules.
* **Memory Safety:** Ensuring that deep parsing errors (like discovering an unbalanced parenthesis mid-evaluation) correctly bubble up the error code while systematically freeing all allocated structures (queues, token stacks, and buffers) to guarantee zero memory leaks.

## 🎯 How to test the project?

A pre-compiled version of the evaluator is available.

1. Go to the **[Releases](../../releases)** section and download the `evalexpr_binary.tar.gz` archive.
2. Extract the archive:
```bash
tar -xvf evalexpr_binary.tar.gz
```
3. Standard Evaluation: Run the binary interactively and type your expression, or pipe it directly.
```bash
# Interactive mode
./evalexpr
3 + 4 * 2
# Output: 11

# Or using a pipe
echo "3 + 4 * 2" | ./evalexpr
# Output: 11
```
4. RPN Evaluation: Use the `-rpn` argument.
```bash
# Interactive mode
./evalexpr -rpn
3 4 2 * +
# Output: 11

# Or using a pipe
echo "3 4 2 * +" | ./evalexpr -rpn
# Output: 11
```
5. Test Error Codes: Check how the program behaves securely on forbidden math operations.
```bash
# Expected Output: 3
echo "42 / 0" | ./evalexpr
echo $?
```
