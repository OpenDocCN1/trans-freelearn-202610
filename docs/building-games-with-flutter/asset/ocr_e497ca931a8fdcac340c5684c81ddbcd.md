cause issues, but Dart is optimized to handle this very quickly.

Most languages require the use of locks to access shared memory, but Dart can perform its garbage collection most of the time without using locks. This fast garbage collection results in very smooth graphics performance, which greatly enhances our game.

# Thread control

The developer has more control over code execution in Dart due to the way threads are implemented. Because Dart doesn't usually require locks for accessing shared memory, unlike most other languages, we have more control over the execution of the code.

Without locks, we avoid a type of call called a race condition, which can happen when separate threads want access to the shared resource (in this case, memory) and it can't be accessed because some other thread has locked access and the lock has to be released before other threads can access it.

In this section, we have discussed how the features of the Dart language help us to write fast games. In the next section, we will summarize what you will learn throughout the book.

# Summarizing the book

In the following subsections, let's start to take a look at what each chapter will explore.

# Flame

In the next chapter, Chapter 2, Working with the Flame Engine, we will cover the basics of how to use the Flame engine library to set up a game loop, and how to organize your assets for efficient loading.

# Designing a game