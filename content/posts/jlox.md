+++
title = 'JLox - an interpreter for Lox'
date = 2024-08-09T15:50:14+03:00
categories = ["project-showcase"]
draft = false
+++

Lox is a programming language designed to be implemented for educational
purposes. The steps to create an interpreter for it are described in
[this book](https://craftinginterpreters.com/) and my implementation is found
[on GitHub](https://github.com/sus-domesticus/crafting-interpreters).

Since copy-pasting source code isn't really stimulating the author provided each
chapter with some challenges. Solutions to the challenges are not found in the book.

Bellow I want to showcase my solutions to the various problems that the author
of the book proposed.

## Table of contents

<!--toc:start-->
- [Interfaces (challenge 1 from chapter 13)](#interfaces-challenge-1-from-chapter-13)
- [Safe access operator (challenge 3 from chapter 13)](#safe-access-operator-challenge-3-from-chapter-13)
- [Warn about unused local variables (challenge 3 from chapter 11)](#warn-about-unused-local-variables-challenge-3-from-chapter-11)
- [Getter methods for classes (challenge 2 from chapter 12)](#getter-methods-for-classes-challenge-2-from-chapter-12)
- [Others](#others)
<!--toc:end-->

## Interfaces (challenge 1 from chapter 13)

An interface requires its target to implement its methods (arity is checked).

**Source:**

```Lox
class A { }

interface IA {
  printfun(a, b)
}

// a quirk of Lox is that interfaces can be used only after inheriting
class B < A, IA {
  printfun(a, b) {
    print a + b;
  }
}

var b = B();
b.printfun(3, 4);
```

**Output:**
`7`

**Source**:

```Lox
class A { }

interface IA {
  printfun(a, b)
}

class B < A, IA {
  // violation of the interface `IA`
  randomFunction(a, b) {
    print a + b;
  }
}
```

**Output:**

```text
Class must contain the method printfun from interface IA
[line 7]
```

## Safe access operator (challenge 3 from chapter 13)

This is useful when you have a variable, let's call it `a`, and you don't know
whether it's `nil` or not. You can do `a?.field` and when `a` is `nil`
the expression evaluates to `nil`.

**Source:**

```Lox
class A { }

var a = A();
a.something = true;

print a?.something; // true

a = nil;
print a?.something; // nil
```

## Warn about unused local variables (challenge 3 from chapter 11)

**Source:**

```Lox
fun printOnlyB(a, b) {
  print b;
}
printOnlyB(10, 20);
```

**Output:**
`[line 1] Error at 'a': Unused variable 'a'.`

**Source:**

```Lox
{
  // only `b` is used inside block
  var a = 10;
  var b = 10;
  print b;
}
```

**Output:**
`[line 2] Error at 'a': Unused variable 'a'.`

## Getter methods for classes (challenge 2 from chapter 12)

This is syntactic sugar for declaring and calling these new `getter`-methods.

**Source:**

```Lox
class Circle {
  init(radius) {
    this.radius = radius;
  }

  // `getter`-methods don't have parenthesis when declared and called.
  area {
    return 3.141592653 * this.radius * this.radius;
  }
}

var circle = Circle(4);
print circle.area; // Prints roughly "50.2655".
```

## Others

There are also solutions to implement
[static functions](https://github.com/sus-domesticus/crafting-interpreters/blob/chapter12/challenge-1/static-functions/example.lox), [anonymous functions](https://github.com/sus-domesticus/crafting-interpreters/blob/chapter10/challenge-2/anon-functions/example.lox) etc.

You can find more in the branches of [my repo](https://github.com/sus-domesticus/crafting-interpreters).
