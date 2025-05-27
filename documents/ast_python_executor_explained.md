
# 🧠 Understanding `ast` and Python Code Execution via AST

## 📌 What is `ast`?

`ast` stands for **Abstract Syntax Tree**. It is a built-in Python module that allows you to:

- **Analyze** Python code before executing it.
- **Transform** or **validate** code for safety.
- **Generate** new code from structured elements.

Instead of executing code directly as a string, you can:

1. Parse the code into an AST using `ast.parse(...)`.
2. Inspect or modify the AST.
3. Compile the AST into a code object.
4. Execute it using `exec(...)`.

### 🔐 Why Use `ast`?
- Add **security checks** (e.g., block `import os`)
- Perform **instrumentation** (add logging, limits)
- Support **static analysis** (check for variables, type hints)

---

## 🔍 Line Explanation

```python
exec(compile(parsed_ast, filename="<ast>", mode="exec"), exec_globals, exec_locals)
```

### 1. `parsed_ast = ast.parse(code_str, mode='exec')`

Converts raw Python code into an abstract syntax tree.

```python
code_str = "x = 2 + 2"
```

Results in an AST like:

```
Assign(
  targets=[Name(id='x')],
  value=BinOp(left=Constant(2), op=Add(), right=Constant(2))
)
```

---

### 2. `compile(parsed_ast, filename="<ast>", mode="exec")`

Compiles the AST into a **code object** that can be executed.

- `filename="<ast>"`: metadata, useful for error messages.
- `mode="exec"`: allows execution of full blocks of code (not just expressions).

---

### 3. `exec(...)`

```python
exec(code_object, exec_globals, exec_locals)
```

Executes the compiled code within the specified **global** and **local** variable scopes.

- `exec_globals`: what’s globally accessible during execution.
- `exec_locals`: usually where tool functions or injected variables live.

---

## ✅ Summary

The following code:

```python
exec(compile(ast.parse(code), "<ast>", "exec"), globals, locals)
```

Is equivalent to:

```python
exec(code, globals, locals)
```

…but with the **ability to inspect and control** the code via the AST in between.

---

## 💡 Bonus: Use Case Example

Want to block dangerous imports like `os`?

You can walk the AST and raise an error if you detect:

```python
import os
```

Would you like a full working example of that?
