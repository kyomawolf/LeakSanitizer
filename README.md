# Welcome to the LeakSanitizer!
This repository contains a library which is designed to help finding leaks. It also has some features
to gain insights into the memory usage of your program.

## Usage
In order to get started, you can either download a compiled version of the library [here](https://www.github.com/mhahnFr/LeakSanitizer/releases).  
Alternatively, you can also build it from source:
- Clone the repository: ``git clone https://www.github.com/mhahnFr/LeakSanitizer``
- and build the library: ``cd LeakSanitizer && make``.

On some systems, you might need to install the ``libexecinfo-dev`` to compile the library successfully.

In order to use this library, compile your code using the following flags: ``-Wno-gnu-include-next -I<path/to/library>/include``.

**Linking**:
- On ``macOS``, link using these flags: ``-L<path/to/library> -llsan -lc++``.
- On ``Linux``, link with ``-rdynamic -L<path/to/library> -llsan -lstdc++``.

Once the library is correctly linked with your code, it will show your memory leaks at exit.  
More features are described in the [wiki](https://www.github.com/mhahnFr/LeakSanitizer/wiki).

## Behind the scenes or: How does it work?
In order to track the memory allocations, the library provides a wrapper around the functions ``malloc``,
``realloc``, ``calloc`` and ``free``. Each allocation is registered and a backtrace for it is stored.

To not track itself, prior to the tracking part the allocation tracking is disabled.

The backtraces are converted to human readable strings once that specific backtrace is printed.

When a allocation function is called for the first time, the ``atexit`` handler and the signal handlers
are installed. The ``atexit`` handler prints all allocations that are still registered when the program
exits.

### Final notes
This project is licensed under the terms of the GPL 3.0.

© Copyright 2022 [mhahnFr](https://www.github.com/mhahnFr)
