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
    "language has no +, -, * or / oprators so need to call a function";
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
