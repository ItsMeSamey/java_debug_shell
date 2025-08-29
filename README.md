# Shell
Shell is a simple, dynamic, dependency-free scripting language interpreter written entirely in a single Java file.
It features a straightforward syntax, a Read-Eval-Print Loop for interactive sessions,
and powerful, direct interoperability with the underlying Java Virtual Machine.

## Features

- **Simple Syntax:** Has a very minimal syntax.
- **Dynamic Typing:** Variables are dynamically typed.
- **Java Interoperability:** Access Java classes, instantiate objects, and call methods directly from the script.
- **First-Class Functions:** Functions are objects that can be assigned to variables and passed as arguments.
- **No Dependencies:** Runs with just a standard Java runtime.

## Introduction

To run the language, compile and execute the `Shell.java` file. This will launch the interactive REPL.

```bash
java Shell.java
```
## Getting Started

You can type code directly into the console. Statements are executed in blocks.
A block of code is submitted for evaluation when you end a line with an exclamation mark `!`.

```
> my_var = "Hello, World!";
  .java.lang.System.out.println(my_var);
  !
Hello, World!
> 
```

## Language Specification

### Syntax Basics

- Every statement must end with a semicolon (`;`).
- Code blocks for functions are enclosed in curly braces `{}`.

### Comments

The language does not have a dedicated comment syntax. However, you can use string literals in places where an expression is allowed. Since the string is not assigned or used, it is effectively discarded, acting as a comment.

```java
"This is a comment, it's a string literal that gets thrown away";
a = 1; "This is another comment";
```

## Built-in Globals

The environment comes with a one just global variable.

-   `globalThis`: A reference to the `Shell` interpreter instance itself.

### Variables and Assignment

Variables are created implicitly upon their first assignment. The `=` operator is used for assignment.

```java
"Assignment from a literal value";
a = 100;


"Assignment from one variable to another";
b = a;
```

### Data Types & Literals

#### Numbers
- **Integer:** Standard integer values.
  ```java
  a = 0;
  ```
- **Long:** Suffix a number with `l` to create a `long`.
  ```java
  a = 0l;
  ```
- **Float:** Numbers with a decimal point are parsed as `float` by default.
  ```java
  a = 0.0;
  ```
- **Double:** Suffix a number with `d` to create a `double`.
  ```java
  a = 0.0d;
  ```

#### Characters and Strings
- **Char:** A single character enclosed in single quotes.
  ```java
  a = 'c';
  ```
- **String:** A sequence of characters enclosed in double quotes.
  ```java
  a = "Hello World";
  ```

### Java Interoperability

You can access any Java class, instantiate it, and call its methods or access its fields.

- **Class Access:** Use a leading dot `.` followed by the fully qualified class name. This returns a class object.
  ```java
  FileClass = .java.io.File;
  ```

- **Instantiation:** Call a class object as if it were a function to invoke its constructor. The interpreter will find the correct constructor based on the number and types of the arguments.
  ```java
  "Access and instantiate in one line";
  file = .java.io.File("myFile.txt");

  "Alternatively, assign the class to a variable first, then instantiate";
  FileClass = .java.io.File;
  file = FileClass("myFile.txt");
  ```

- **Method & Field Access:** Use the dot `.` operator to access instance or static fields and methods.
  ```java
  "Get the static PI field from java.lang.Math";
  pi = .java.lang.Math.PI;

  "Call the instance method 'getAbsolutePath' on the file object";
  path = file.getAbsolutePath();
  ```

### Operators

The language does not support traditional infix operators like `+`, `-`, `*`, `/`, or `&&`. All operations, including logical ones, are performed using built-in function calls.
For mathematical operations, you must use Java's built-in classes.

```java
> Math = .java.lang.Math;
> result = Math.addExact(10, 20);!
```

### Return Values

To return a value from a function, you must explicitly create and evaluate a `ReturnValue` object. The parser does not have a `return` keyword.

```java
myFunc = () {
  .ReturnValueClass("this is the return value");
};
```

### Functions

The language treats functions as values. They can be defined, assigned to variables, and called.

- **Definition & Calling:** Functions are defined with a parameter list in `()` and a body in `{}`. They are called using `()`.
  ```java
  "Define a function and assign it to the variable 'add'";
  add = (a, b) {
    "language has no +, -, * or / operators so need to call a function";
    globalThis.ReturnValueClass(.java.lang.Integer.sum(a, b));
  };

  "Call the function";
  result = add(5, 10);
  ```

- **Inline Function Calling:** You can define and call a function immediately without assigning it to a variable.
  ```java
  "The result of the function call is assigned to 'a'";
  a = (x) { globalThis.ReturnValueClass(.java.lang.Integer.sum(a, a)); } (10); "a is now 20";
  ```

- **Variadic Functions:** You can define a function that accepts a variable number of arguments by prefixing the last parameter name with `...`. The variadic arguments will be passed as a `List`.
  ```java
  printAll = (...numbers) {
    .java.lang.System.out.println(numbers);
  };

  "prints `[1, 2, 3, 4, 5]`";
  printAll(1, 2, 3, 4, 5);
  ```

## Init.Shell

This file contains a collection of useful function that you probably want to have.
These functions are nothing special, they are just imported from the globalThis instance.

Of course. Here is a markdown documentation table for only the functions and constants defined in the `init` script you provided.

### Core Functions and Constants

| Name | Type | Signature / Value | Description |
| :--- | :--- | :--- | :--- |
| **`globalThis`** | Constant | `Shell`  | A reference to the current `Shell` instance. |
| **`true`** | Constant | `true` | Boolean `true`. |
| **`false`** | Constant | `false` | Boolean `false`. |
| **`Shell`** | Constant | `Shell.class` | The Java `Class` object for the `Shell` interpreter. |
| **`Range`** | Constant | `Shell.RangeClass` | The Java `Class` object for creating numerical ranges, for use in `forEach` loops. e.g., `forEach(Range(1, 10), print);` |
| **`print`** | Function | `(...stuff)` | Prints one or more arguments to the terminal. |
| **`setGlobal`** | Function | `(name, value)` | Sets a variable in the global scope, making it accessible from anywhere. |
| **`getGlobal`** | Function | `(name)` | Retrieves a variable from the global scope, bypassing any local variables with the same name. |
| **`exists`** | Function | `(variableName)` | Returns `true` if a variable with the given name exists in the current scope. |
| **`getParentVar`**| Function | `(key)` | Retrieves a variable from the direct parent function's scope. Does not work for the global scope. |
| **`isClass`** | Function | `(obj)` | Returns `true` if the provided object is a Java `Class` type. |

### Conditional Logic & Control Flow

| Name | Type | Signature | Description |
| :--- | :--- | :--- | :--- |
| **`return`** | Constant | `Shell.ReturnValueClass` | The class used to return a value from a function. Use as `return(value);`. |
| **`returnN`** | Function | `(n, value)` | Returns a `value` from `n` nested function scopes. `returnN(1, v)` is equivalent to `return(v)`. |
| **`if`** | Function | `(condition, functionIfTrue)` | Executes `functionIfTrue` if the `condition` evaluates to a truthy value. |
| **`ifElse`** | Function | `(condition, funcIfTrue, funcIfFalse)` | Executes `funcIfTrue` if `condition` is truthy, otherwise executes `funcIfFalse`. |
| **`while`** | Function | `(condition, loopBodyFunc)` | Executes `loopBodyFunc` repeatedly as long as `condition` evaluates to a truthey value. |
| **`forEach`** | Function | `(iterable, functionPerItem)` | Executes `functionPerItem` for each item in an `iterable` (like an array or `Range`). |
| **`tryCatch`** | Function | `(tryBlock, catchBlock)` | Executes `tryBlock`. If a `ShellException` is thrown, executes `catchBlock`. |
| **`eql`** | Function | `(a, b)` | Performs a deep equality check on two Java objects. Returns `true` if they are equal. |
| **`and`** | Function | `(a, b)` | Returns `true` if both `a` and `b` are truthy. |
| **`or`** | Function | `(a, b)` | Returns `true` if either `a` or `b` is truthy. |
| **`not`** | Function | `(a)` | Returns `true` if `a` is falsey, and `false` if `a` is truthy. |
| **`isNull`** | Function | `(a)` | Returns `true` if the object `a` is `null`. |

## Pitfalls

1. There is a reason why `exists` exists. If a variable is set to null, the identifier is considered deleted.
  Eg: `a = () {} (); print(a);` will cause an error, use `if(exists("a"), () { print(getParentVar("a")); });` instead.
2. `return()` is just a value not a keyword. This means you can have an instance of return without actually returning anything. Infact, this is how returnN function is implemented.
  ```java
  () {
    "Does Not return";
    () {} (x = return(123));

    "This will print `ReturnValue(123)`";
    print(x);

    "This will cause the function to return 123";
    x;
  } ();
  ```
3. This language doesn't really have any keywords, so you can just reassign any of these functions or so called 'constants'. But it is highy advised against.
4. The recommended way to pass variables through function scopes is to use `setGlobal(".age", age);` to set a variable `age` and then `getGlobal(".age")` in the sub-function to get it. This will likely be faster then getParent.
5. The language does NOT coerce types, so you can't use integers instead of floats (must use `5.0` and not `5`).
6. No mathematical operators like `+`, `-`, `*`, `/` are provided. While it is feasible to implement these using stack based evaluation, it makes the code ugly, so i'm just not doing it.
7. `a = b().c;` pattern does not work. Use `a = b(); a = a.c;` instead.

