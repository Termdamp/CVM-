# CVM++

A mini programming language built from scratch in C++ as part of the Even Semester Projects by Coding Club, IIT Guwahati.

The idea is simple you write code in a custom language (`.cvm` files), and this project handles everything needed to actually run it: reading the text, understanding it, converting it to instructions, and executing those instructions. No libraries doing the heavy lifting, just plain C++.

---

## How it works

Your code goes through 4 stages:

```
your .cvm file
     ↓
   Lexer        breaks text into tokens (words and symbols)
     ↓
   Parser       builds a tree showing the structure of the code
     ↓
   Compiler     converts that tree into bytecode instructions
     ↓
   VM           runs those instructions using a stack
     ↓
  output
```

Each stage is written as a separate module in `src/`.

---

## Building the project

You need g++ installed. Then just run:

```bash
g++ src/main.cpp src/lexer.cpp src/parser.cpp src/compiler.cpp src/vm.cpp -o cvm
```

That produces a `cvm` executable in the current folder.

---

## Running it

Three ways to use it:

**Run a script file**
```bash
./cvm scripts\test.cvm
```

**REPL type and run code interactively**
```bash
./cvm
```

Inside the REPL:
- type code line by line, variables persist across lines
- type `reset` to clear all variables and start fresh
- type `debug` to toggle bytecode printing
- type `exit` to quit

**Debug mode see the bytecode your code compiles to**
```bash
./cvm --debug scripts\test.cvm
```

---

## The language

Pretty minimal by design. Here's what it supports:

**Variables**
```
let x = 10
let y = x + 5
```

**Arithmetic**
```
let result = x + y * 2 - 1
```
Supports `+`, `-`, `*`, `/`

**Comparisons**
```
x == 10
x < 20
x > 5
```

**If / else**
```
if (x < 20) {
    print x
} else {
    print 0
}
```

**While loops**
```
let i = 0
while (i < 5) {
    print i
    let i = i + 1
}
```

**Print**
```
print x
print x + y
```

**Input**  reads an integer from the user
```
let x = input
print x
```

**Booleans**
```
let flag = true
let flag = false
```

---

## Sample scripts

There are a few example scripts in the `scripts/` folder:

| File | What it does |
|---|---|
| `test.cvm` | basic variables and arithmetic |
| `loop.cvm` | simple while loop |
| `ifelse.cvm` | if/else branching |
| `countdown.cvm` | counts down from 10 to 1 |
| `input_test.cvm` | takes two numbers from user and adds them |
| `fizzbuzz.cvm` | fizzbuzz up to 15 |

---

## Project structure

```
CVM++/
├── src/
│   ├── lexer.h
│   ├── lexer.cpp
│   ├── parser.h
│   ├── parser.cpp
│   ├── compiler.h
│   ├── compiler.cpp
│   ├── vm.h
│   ├── vm.cpp
│   └── main.cpp
├── scripts/
│   ├── test.cvm
│   ├── loop.cvm
│   ├── ifelse.cvm
│   ├── countdown.cvm
│   ├── input_test.cvm
│   └── fizzbuzz.cvm
└── README.md
```

---

## What I learned

Going into this I understood how to write C++ but had no idea how a language actually works under the hood. Building each stage separately made it click the lexer is just string processing, the parser is just checking grammar rules, the compiler is just a tree walk, and the VM is just a loop with a stack. None of it is magic once you build it yourself.

The trickiest part was getting the jump instructions right for `if/else` and `while` you emit a jump before you know the target, then go back and patch it once you do. That took a bit of debugging to get right.

---

## Reference

The main resource I used was *Crafting Interpreters* by Robert Nystrom the full book is free at craftinginterpreters.com. Chapters 14 onwards cover exactly this kind of bytecode VM.
