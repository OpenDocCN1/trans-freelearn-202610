# Preface

Welcome to the world of IPython 4.0, which is used in high performance and parallel environments. Python itself has been gaining traction in these areas, and IPython builds on these strengths.

High-performance computing (HPC) has a number of characteristics that make it different from the majority of other computing fields. We will start with a brief overview of what makes HPC different and how IPython can be a game-changing technology.

We will then start on the IPython command line. Now that Jupyter has split from the IPython project, this is the primary means by which the developer will interface with the language. This is an important enough topic to devote two chapters to. In the first, we will concentrate on basic commands and gaining an understanding of how IPython carries them out. The second chapter will cover more advanced commands, leaving the reader with a solid grounding in what the command line has to offer.

After that, we will address some particulars of parallel programming. IPython parallel is a package that contains a great deal of functionality required for parallel computing in IPython. It supports a flexible set of parallel programming models and is critical if you want to harness the power of massively parallel architectures.

Programs running in parallel but on separate processors often need to exchange information despite having separate address spaces. They do so by sending messages. We will cover two messaging systems, ZeroMQ and MPI, and in relation to both how they are used in already existing programs and how they interact with IPython.

We will then take a deeper look at libraries that can enhance your productivity, whether included in IPython itself or provided by third-parties. There are far too many tools to cover in this book, and more are being written all the time, but a few will be particularly applicable to parallel and HPC projects.