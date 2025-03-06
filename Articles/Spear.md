
### The problem

Programmers are constantly thinking. A significant portion of development time is spent thinking about how to solve a problem (and not actually typing).Sometimes, the programmer is stuck in a loop of analysis paralysis, where they are unable to make a decision because there's so many intracacies and alternatives to consider. This is a problem that I have faced many times, and I have come to the conclusion that it is a problem that can be treated with better programming languages. 

My theory is - every decision that the programmer makes costs valuable mental energy. There's a limited amount of mental energy available to the programmer each day. Every time a decision is made, the total energy available for the rest of the day decreases, until eventually it is very difficult to make further progress.Therefore, a programming language should be as resourceful as possible with mental energy, only forcing the programmer to make a decision when it really matters. 

Quite a common decision that programmers have to make is whether or not a variable is "constant". This decision needs to be made every time a new variable is declared. For example you can do this in C++:
```C++
// The value of this can change
int variable = 0;

// The value of this cannot change
const int constant = 0;
```
This decision between the above two choices is often not needed, you can quite often clearly tell that a variable is mutated just from glancing at the code (especially if the variable is mutated within the same scope). When a language is full of many intracies to consider, there's a real danger of getting distracted and spending valuable time debating over concepts that do not matter in the grand scheme of things. The term "bikeshedding" is often used to describe this phenomenon. Bikesheding is a term that comes from the story of engineers spending more time discussing the color of a bikeshed outside than the design of the nuclear reactor they were working on.

Another important aspect of programming is that the programmer needs to think about how their code will be executed to ensure program correctness. In other words, the programmers mind needs to do essentially *the same thing* as the compiler - translate the program text into a series of logical statements. This is a problem because if the code is difficult to parse for the compiler, then it is likely also difficult to parse for the programmer too, and they can make mistakes. C++ is a notorious example of this, where it is difficult to parse and programmers need to constantly keep undefined behaviour in mind to avoid pitfalls. A significant number of bugs can be caused by mistaken expectations of how the code will be executed. Anything superflous for the compiler is potentially superflous to the programmer and may obfuscates the logic. This is all to say that the syntax of a programming language should be minimal and unambiguous. 

The more technical details and quirks introduced into the language, the higher the mental load expected from the programmer, and the less efficiently they can work. This is why I have decided to make a programming language that is as minimal as possible. The language is called Spear, and it is a compiled, high performance language that takes inspiration from Python.

### The Idea

Spear is a programming language designed around mitigating analysis paralysis. To declare a variable in spear you just use a colon, type is infered:
```
foo: 4
```
To make a function, you just use braces:
```
main (args: int[..]) { 
  print("Hello world!")
}
```
Structs are declared with parenthesis and members are separated by commas:
```
foo (
  thing: true,
  stuff: "hello world",
)
```
## Memory Management

Spear does not support dynamic memory allocation. The reasoning is - dynamic memory allocation is error-prone and can lead to crashes in the program if handled poorly. Some programming languages use a garbage collector to manage memory, but this can lead to unpredictable stalls in the program. To enforce reliable code, Spear instead enforces that all values must be allocated on the stack or allocated in static memory. To avoid stackoverflow, the total required memory usage is calculated at compiletime, so that the stack size can be adjusted depending on the memory needed. Not only is spear totally safe from memory leaks, it is also totally safe from stack overflows.
To allocate an array in Spear, you do the following:
```
foo: int[10]
```
This will allocate an array with a capacity of ten integers. Note that the count of this array starts out at zero, you can change the count by appending to the array:
```
foo: int[10]
append(foo, 5) // array now has a count of 1 and contains [5]. The capacity is 10, allowing for 9 more elements to be appended.
```
Spear does not support recursion by default(it is a compile time error). If you want to make a function recursive, you must explicitly mark it as such and set a maximum recursion depth:
```
#recursive(10) minimax(chess_board: chess_board, alpha: int, beta: int) {
}
```


