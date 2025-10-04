## Error Handling

I think error handling is one of those things that is poorly handled in programming languages. I am not a fan of exceptions, and I'm also not a fan of Result types either (like you find in Rust or Haskell). The one fundamental flaw with both of these approaches is that they do not take into account development vs release builds. Quite often you actually want to handle errors differently depending on the user of the program, and so a one-size fits all solution doesn't work well.

## Failing Fast vs Fault Tolerant

'Failing Fast' is the concept of halting or pausing a program as soon as something goes wrong. In general, 'Failing Fast' is a good thing to do in development builds - you want to pause whatever the program is currently doing so that the programmer is instantly alerted of the problem and can diagnose and fix the issue. 'Failing Fast' for example could be throwing an exception - which can unwind many layers of function calls. The advantage of failing fast is that total catastrophic failure catches attention, so any problem that causes a program to 'fail fast' can be more easily caught by the programmer. 

The problem is, In release builds, quite often there isn't a programmer to fix the problem. So completely pausing the program wouldn't be helpful. Alerting the user that something went wrong is quite often a good idea - but breakpointing or pausing the program entirely is just unnecessary, because there's nothing that the user can do about the problem. Ideally, the program recovers on its own in some way so that the user experience isn't completely ruined. 

What I think is actually desirable in many cases is for programs to be 'Failing Fast' in development builds but 'Fault tolerant' in Release builds. That is that when a problem arises in release, the program tries to recover from the error, but when a problem occurs in development the program 'fails fast', immediately alerting the programmer so that they can fix the problem. The "recovery" aspect of error handling completely depends on the user. If you are a programmer working on the project, you want all issues to be alerted as soon as possible in the most clear way 'Failing Fast'. If you are a user, there's no relevance that 'Failing Fast' has to you, you just want to know if an error occured and hope that the program recovers from it. In some programs it might even be desirable to hide that a problem occured to the user, especially if you want to keep the program looking proffesional despite encountering issues.

So on the one hand you have an audience that wants to see all of the problems immediately to address them, and on the other hand an audience that doesn't want to see problems at all. 

## Why Exceptions and Result types aren't ideal

Exceptions occur in exceptional scenarios - like performing an index out of range from an array. Exceptions unwind the call-stack, so they deliver more widespread destruction to the program as the stack unwinds. This dramatic exceptional event is good for 'Failing Fast': If a problem delivers wider reach then it's more likely to bring its attention to the programmer. In release, this stack-unwind is not ideal, because it has a heavy performance cost and it causes disruption to the user experience as it resets the program - possibly more disruption than necessary (especially if the catch block is far away). In general, we want programs to recover with as little disruption to the rest of the system as possible - that means catching the issue as soon as it occurs and then using alternatives. So exceptions work great for alerting programmers, but not so great for users to deal with, especially if it results in a program crash. 

Result types (errors as values) are the opposite problem. Results do not provide nearly the same level of programmer alert as an exception, but they encourage programs to gracefully recover closer to the source of the problem. An Exception will alert the programmer and quite often provide them with a stack-trace to fix the issue. A Result is just a special return value indicating an error. It is just useful in the sense that the caller can check this return value and gracefully recover if it wants. However, Results do not alert the programmer at all unless you unwrap() them (and even then they probably do not give a stack-trace).

What we actually need is a system for graceful recovery in release but programmer alert in development. assert() in C is very close to this. assert() will only trap the program in debug builds, so that the programmer can address the issue. However, assert() does not provide a means of graceful error recovery in release, so I made a version of assert with this error recovery in mind:

``` C
// BUG macro can be changed in release/development builds. Maybe you want to alert the user of the bug or write it to a log file?
// BREAKPOINT macro also breakpoints in development builds.
#define BUG(...) printf(__FILE__":" TOSTRING(__LINE__) " Bug: " __VA_ARGS__); fflush(stdout); BREAKPOINT()

// Assert macro, similar to c assert() but has a gracefull fallback error-recovery for release mode. 
#define ASSERT(condition, fallback, ...) \
    if (!(condition)) { \
        BUG(__VA_ARGS__); \
        fallback; \
    }
```
This is what I ended up using in my projects instead of exceptions/option types. Every part of the program validates itself with asserts. This allows me to quickly fix issues as soon as they are found (just like unit testing), and in release mode these asserts switch to an error-recovery model to make the experience more user friendly. I don't want the player of my games to get a bad experience - even if a bug happens.  

## Code Comparison

Here I've made a few comparisons so that you can see the differences with these approaches. This is sudo code for opening a file:

```
file open_file(file_name) {
    // ... some code here for managing and opening the file.
    if (!file_exists) {
        throw exception("Failed to open file");
    }
    return file;
}
```

```
result<file> open_file(file_name) {
    // ... some code here for managing and opening the file.
    if (!file_exists) {
        return error("Failed to open file");
    } else {
        return ok(file);
    }
}
```
In both of these, it falls upon the calling code to manage development vs release differences. Maybe you want to be alerted (as a programmer) that there's a missing file, but in release you want the program to recover in some way. Here is the version that I would make to handle these differences: 
```
result<file> open_file(file_name) {
    // ... some code here for managing and opening the file.
    // ASSERT macro has a second clause that says what to do if the assert fails in release-mode (in this case, return error).
    // Otherwise, in development and debug mode this ASSERT will breakpoint.
    ASSERT(file_exists, return error(), "file does not exist");
    return ok(file);
}
```
It still uses result/option types for release, so that the program can recover gracefully, but in development these asserts will immediately inform the programmer that something went wrong by breakpointing the program. Note that I don't return the string for the error message, this is because the ASSERT macro already handles the string and can write it to a log file or do something else with the message. This strategy keeps the calling code nice and clean, because the calling code just needs to worry about recovering the program and not alerting developers. 

