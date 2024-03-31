# Philosophy
Python and Rust are vastly different languages, with profund distinctions in their goals, which then influence their design decisions and consequently the whole language.

While Python focuses fundamentally on ease of use, Rust is a modern take on the original goals of C++: performance, versatility and zero-cost abstractions.

Much of these stem from the fact that, while today Rust can be considered a general purpose programming language like Python, it was created and intended as a systems programming language, i.e. one that is suitable for programming operating systems, controllers, and generally anything that is low level and close to the hardware.

In a system programming performance is key. This means that programs have to be compiled and have zero cost abstractions. We'll start with the first and proceed with the second.

## Compilation
Computers fundamentally understand only bits, so the human readable programs that we write have to be translated into something that the computer can understand, in some manner.

The conceptually simpler way to do this is to transalte everything directly. The problem with this approach is portability: the binary format that a computer understands depend on the CPU architecture, the operating system and possibly also on the individual CPU model and hardware configuration.

This means that in order to execute the same program on different machines one runs into the very complex problem of software distribution. In order to simplify things, many programming lanuages, like Python, are *interpreted*. This means that instead of compiling the program before hand, you simply distribute your source code* and, *during execution*, there is a program that interprets it and understands how to run it. This program is called an *interpreter*. This is why the `python` command you use to run your programs is called an *interpreter*.

While using an interpreter simplifies the problem of software distribution, it performs worse. This is because, of course, instead of being able to just execute the logic we meant when writing the program, the computer first has to translate it and interpret it. Another important factor is that typically interpreters do not have enough time to optimize the program.

Now, the convenience of having an interpreter often outweights the drawbacks of the performance hit, but in some situation it doesn't and that's when Rust comes in.

## Zero-Cost Abstractions
Rust tries to remove all unnecessary stuff from the binary.

This means that you can't operate on language constructs, such as types or functions, at runtime. You can, however, do that at compile time with macros.

## Correctness
Rust has a great emphasis on correctness, i.e. making sure that your program does what it should in the greatest number of possible cases. This is a fundamental difference with Python which focuses most of all on speed of development.

Hacking toghether a script is therefore very quick in Python. If, however, you have to write complex, robust programs, that becomes very hard. Don't get me wrong, there are many large and complex programs around that are written in Python and that are irunning successfully. The fact that it is possible, however, doesn't mean, that using Python for complex program is the *optimal choice*.

The two main culprits here are Python's dynamic nature and its error model - i.e. exception. The first point is simply a recongnition of the fact that in a large program written in vanilla Python it's hard to make sure that you're passing always passing an `int` to a function that expects an `int` and not a string or a class. For this reason, starting in version 3.5 (released in 2014) Python [introduced its own optional type system](https://peps.python.org/pep-0484/), with the idea that for complex enough programs the time lost by declaring the types of the variables, fields and paramaters would be more than compensated by the time saved by the errors that MyPy catches during development.

Python type system, however, remains quite limited in what it can understand and track. Rust type system is much more sophisticated, meaning, among other things, that it can detect more errors during development and that it can be utilized in more advanced ways.

Python's error model is based on exceptions, as it is for most of the languages popular today: JavaScript, Java, C#, Swift, Kotlin, Ruby and, to some extent, C++. Albeit popular, exceptions have a fundamental limitation: they are not part of the singature of the function that we're calling[^signature_exceptions] and are, at best, only part of the documentation of function. It's hard to keep the code and the documentation in sync and, moreoevore, it's actually hard to ensure that when we're listing the exceptinos that our code can throw to ensure that we there's isn't an exception deeply nested in the code that we're calling and that we have missed.

This means that an exception based error model makes it very hard to make sure our program won't throw an unexpected and unhandled error.

Rust error system follows a different philosophy, borrowed from the functional programming word, and makes the errors part of the singature of the function. This makes it **way easier** to ensure that you're handling any possible error case of the code that you're calling. Also, if the error behaviour of the functions you're calling changes you will typically get notified by the compiler, because the singature of the function changes.

If you want a more in depth overview of the history of error modelling in programming languages I recommend [this excellent post](https://joeduffyblog.com/2016/02/07/the-error-model/) by Joe Duff 


[^signature_exceptions]: Actually, some of the languages in the list experimented with making the exceptions part of the singature of functions and methods, but with little success.
