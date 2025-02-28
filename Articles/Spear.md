### Spear Programming Language

I've had ideas for a programming language for quite some time. The idea is to make a programming language that aims to mitigate analysis paralysis. The reasoning is simple: a significant portion of development time is spent deciding between different techniques, and a significant portion of programming languages today use very ugly, very verbose syntax to specify lots of (sometimes unnecessary)
details and techniques. For example, a popular idea in many languages is to specify whether or not a variable is "constant": 

```C++
// The value of this cannot change
const Foo foo;
// The value of this can change
Foo bar;
```

This decision needs to be made every time the programmer makes a variable, and in most cases the decision does not matter. In theory, specifying whether something is a constant should help with debugging, but in practice the user often needs to write more code to support both constants and non-constants. C++ is no exception. 

```C++
class DataStructure {
private:
  Foo items[100];
public:
  Foo& get_value(size_t index) {
    return items[index];
  }
  const Foo& get_value(size_t index) const {
    return items[index];
  }
};
```
Here, `get_value` needs to be made twice. Once for when the DataStructure is constant and once for when it isn't. This duplicated code is itself error prone. 
The more code that needs to be made, the higher the chances that an inconsistency between the implementations could appear later on, leading to bugs. 

This is, of course, a trade-off, but I don't think it is a trade-off that is worth making. Modern IDE's and debuggers are powerful enough to spot all of the mutations happening to
variables in real time. If we had to trace the mutation of variables ourselves without a debugger, then constants would be a lot more valuable. This whole thing might feel nit-picky, but when your job is to make decisions all the time it is valuable to save that energy for
more valuable problems. 

There's evidence to suggest that willpower is a limited resource - every time an individual makes a decision, the total energy they have for the rest of the day decreases, until eventually
it is very difficult to make progress. Optimising for developer time is about using this limited energy efficiently, and not superfluous problems.

### The Idea

My programming language - Spear, uses as little syntax as possible to specify what you want to implement. It takes inspiration from python but is compiled and high performance.
To declare a variable in Spear, you just use a colon: 

```
foo: 4
```

Types are inferred from assignment and variables must be assigned on the same line they are declared. To make a function, you just use braces:
```
main {
  print("Hello world!")
}
```
Arguments can be specified with parenthesis. 
```
main (args: []string) {
  print("Hello world!")
}
```
To create a struct, you just use parenthesis and separate the members with commas:
```
foo (
  thing: true,
  stuff: "hello world",
)
```
The parser is intelligent enough to disambiguate between types and other symbols, so you can use the same syntax for both. To instantiate a struct, you just
do the following: 
```
instance: foo(thing: true, stuff: "hi")
```

The goal is a language with as little keywords as possible, so your mind can filter the code more efficiently. The language places strong limitations to reduce 
decision making -> functions cannot return values or be used in expressions. You must instead use "out" parameters. This allows the language to clearly disambiguate 
between struct instances and function calls. It also reduces the overall decision making space for the programmer - there's only one way to return values from a function and not two. 

### Memory Management in Spear 

Dynamic memory allocation is problematic. Most programming languages use a garbage collector to automatically search the program and delete things as needed. Garbage collection can cause notable stalls when the search occurs. Some languages use manual memory management but this is error prone. NASA prohibits all usage of dynamic memory allocation - all memory usage must be fixed and known at compile time. In Spear there is no dynamic memory allocation. All memory is allocated on the stack. To support this, the default stack size is much higher than a standard C/C++ program. The philosophy behind this idea is that knowing your memory usage ahead of time is good practice, both for performance and for avoiding errors. This also means there is only "one way" to allocate variables and references to variables always remain valid. 

To enforce memory safety, by default functions also cannot be recursive (it will be a compiletime error.). To make a function recursive you must explicitly mark it as recursive and set a maximum recursion depth. This means that the total required memory usage can be worked out at compiletime.
```
#recursive(10) minimax(chess_board: chess_board, alpha: i32, beta: i32) {
}
```


