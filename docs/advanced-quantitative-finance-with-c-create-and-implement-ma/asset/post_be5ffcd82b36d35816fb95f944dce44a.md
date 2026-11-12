
## Preface

Quantitative Finance is a highly complex interdisciplinary field, which covers mathematics, finance, and information technology. Navigating it successfully requires specialist knowledge from many sources, such as financial derivatives, stochastic calculus, and Monte Carlo simulation. Crucially, it also requires a hands-on ability to transform theory into practice effectively.

In Advanced Quantitative Finance with C++, we provide a guided tour through this exciting field. The key mathematical models used to price financial derivatives are explained as well as the main numerical models used to solve them. In particular, equity, currency, interest rates, and credit derivatives are discussed. The book also presents how to implement these models in C++ step by step. Several fully working, complete examples are given that can be immediately tested by the reader to support and complement their learning.

## What this book covers

- Chapter 1, What is Quantitative Finance?, gives a brief introduction to Quantitative Finance, delimits the subject to option pricing with C++, and describes the structure of the book.
- Chapter 2, Mathematical Models, offers a summary of the fundamental models used to price derivatives in modern financial markets.
- Chapter 3, Numerical Methods, reviews the three main families of numerical methods used to solve the mathematical models described in the Chapter 2, Mathematical Models.
- Chapter 4, Equity Derivatives in C++, demonstrates the concrete pricing of equity derivatives using C++ in a basic contract (European Call/Put), and an advanced contract (multi-asset options).
- Chapter 5, Foreign Exchange Derivatives with C++, illustrates the pricing of foreign exchange derivatives using C++ in a basic contract (continuous barrier) and an advanced contract (terminal barrier).
- Chapter 6, Interest Rate Derivatives with C++, shows the pricing of interest rate derivatives using C++ in a basic contract and an advanced Interest Rate Swap (IRS).
- Chapter 7, Credit Derivatives with C++, demonstrates the concrete pricing of credit derivatives using C++ in a basic contract (Merton model) and an advanced contract (Credit Default Swap (CDS)).
- Appendix A, C++ Numerical Libraries for Option Pricing, gives a short guide to the various numerical libraries that can be used for option pricing.
- Appendix B, References, lists all the bibliographic references used throughout the chapters of this book.

## What you need for this book

In order to implement the pricing algorithms described in this book, you will need some basic knowledge of C++ and Integrated Development Environment (IDE) of your choice. I have used Code::Blocks, which is a free C, C++, and Fortran IDE, and is highly extensible and fully configurable. You can download it from http://www.codeblocks.org/. You will also need a C++ compiler. I have used MinGW, which is a part of the GNU Compiler Collection (GCC), including C, C++, ADA, and Fortran compilers. This compiler can be downloaded from http://www.mingw.org/.

## Who this book is for

This book is ideal for quantitative analysts, risk managers, actuaries, and other professionals working in the field of Quantitative Finance who want a quick reference or a hands-on introduction to pricing of financial derivatives. Postgraduate, MSc, and MBA students following university courses on derivatives in corporate finance and/or risk management will also benefit from this book. It could be used effectively by advanced undergraduate students who are interested in understanding these fascinating financial instruments. A basic familiarity with programming concepts, C++ programming language, and undergraduate-level calculus is required.

## Conventions

In this book, you will find a number of styles of text that distinguish among different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: "An important feature of this algorithm is the function in code snippet 2 (random.cpp)."

A block of code is set as follows:

```
for (int i=0; i < N; i++)
{
    double epsilon = SampleBoxMuller(); // get Gaussian draw
    S[i+1] = S[i]*(1+r*dt+sigma*sqrt(dt)*epsilon);
}
```

**New terms** and **important words** are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: "In this book, all the programs are implemented with the newest standard C++11 using **Code::Blocks** (http://www.codeblocks.org) and **MinGW** (http://www.mingw.org)".

> Warnings or important notes appear in a box like this.

> Tips and tricks appear like this.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book–what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply send an e-mail to feedback@packtpub.com, and mention the book title via the subject of your message.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on www.packtpub.com/authors.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you would report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/submit-errata, selecting your book, clicking on the errata submission form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded on our website, or added to any list of existing errata, under the Errata section of that title. Any existing errata can be viewed by selecting your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem with any aspect of the book, and we will do our best to address it.

## What is Quantitative Finance?

Quantitative Finance studies the application of quantitative techniques to the solution of problems in finance. It spans diverse areas such as the management of investment funds and insurance companies, the control of financial risks for manufacturing companies and banking industry, and the behavior of the financial markets. Quantitative Finance is eminently interdisciplinary building upon key expertise from the disciplines of finance, mathematics, and informatics.

In this book, we will focus on one aspect of Quantitative Finance – the pricing of financial derivatives using the programming language C++. In the following sections, we will describe the main features of the three key disciplines that constitute Quantitative Finance:

- Finance
- Mathematics
- Informatics

## Discipline 1 – finance (financial derivatives)

In general, a financial derivative is a contract between two parties who agree to exchange one or more cash flows in the future. The value of these cash flows depends on some future event, for example, that the value of some stock index or interest rate being above or below some predefined level. The activation or triggering of this future event thus depends on the behavior of a variable quantity known as **the underlying**. Financial derivatives receive their name because they derive their value from the behavior of another financial instrument.

As such, financial derivatives do not have an intrinsic value in themselves (in contrast to bonds or stocks); their price depends entirely on the underlying.
