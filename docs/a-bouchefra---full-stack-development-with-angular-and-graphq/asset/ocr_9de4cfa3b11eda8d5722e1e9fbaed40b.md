The nvm utility is compatible with Linux, macOS, and **Windows Subsystem for Linux** (WSL). You may use two unofficial alternatives for Windows:

+   - nvm-windows, which may be found at https://github.com/coreybutler/nvm-windows
+   - nodist, which may be found at https://github.com/marcelklehr/nodist

That's everything – you've configured your development machine to run Node.js, allowing you to install and use Node.js packages such as Express.js to build your backend application, as well as frontend libraries and tools such as Angular CLI.

We'll use npm to install the dependencies for our backend and frontend apps throughout this book.

# Summary

In this chapter, we learned about the full stack and monorepo architectures of the application we'll be building throughout this book, as well as the technologies we'll be utilizing to build it.

After covering the architecture and technologies, we looked at how to install MySQL on our development machine. Finally, we learned how to install and set up Node.js, which is required by our full stack application's frontend and backend.

This chapter is now complete! We need to build a Node.js server with GraphQL support, to implement the backend, now that we've set up the development environment and installed Node.js. In the following chapter, we'll begin by installing and configuring Express.js, before adding Apollo Server and looking at how to test and debug our GraphQL server.