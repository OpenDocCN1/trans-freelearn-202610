
Quantum systems' interaction with an environment might result in some errors that would need to be corrected. Since the 1990s, a plethora of quantum error correction techniques has been proposed.

Currently, a full-blown, highly scalable quantum computer has not yet been realized. However, smaller quantum computers are already being built. These small-to-medium-scale quantum computers, which typically do not use some of the complex error correction techniques, are referred to as noisy intermediate-scale quantum (NISQ) computers. NISQ computers are typically used for small-scale quantum computing, since they still do not have error correction capabilities. Therefore, NISQ computers are used mainly to demonstrate some limited capabilities of quantum computing, since they are not full-scale quantum computers.

Now that we have covered the quantum computing subfield, the next step is to explore another subfield of QIP, namely quantum games. Therefore, the design of quantum games is briefly discussed next.

## Design of quantum games

Besides quantum cryptography and quantum computing, another subfield of QIP is the design of quantum games. In a conventional arrangement, game theory is concerned with rational decision making in a conflict environment. Quantum game theory generalizes conventional game theory to the quantum domain. Like other QIP subfields, quantum games harness quantum phenomena, especially entanglement, as a resource, and the objective is to outperform their classical counterpart.

So far, we have provided a brief introduction to QIP. We have introduced the subfields of quantum cryptography, quantum computing, and quantum games. The information provided in this chapter will make it possible for you to easily follow the information that will be provided in the later chapters of this book. Subsequent chapters of this book will further elaborate on what was introduced in this chapter.

The next subsection provides a rationale for using Python for QIP.

## Understanding the rationale for choosing Python for QIP

The programming language of choice that will be used in this book is Python.

Python is arguably one of the most popular programming languages. This is due to the fact that Python is a very versatile, high-level programming language with a very active and robust developer and open source community.

Python’s versatility and simplicity make it the language of choice for diverse applications such as embedded systems, gaming, website development, graphics design, data science, artificial intelligence, and complex scientific and numerical computing.

Another reason for choosing Python as the language of choice for this book is that the majority of the quantum information frameworks that will be deployed later in this book are based on Python. These QIP frameworks include those developed by prominent companies such as Google and IBM, together with some from lesser-known QIP start-up companies such as Xanadu in Canada (the official website of Xanadu is https://www.xanadu.ai).

## Installing Python and other packages

Python is a cross-platform programming language in the sense that it can be used in at least three of the major operating systems, namely Microsoft Windows, Apple’s macOS, and open source Linux.

Python’s installer and documentation are freely available on Python’s official website at https://www.python.org/ and are shown in Figure 1.2:

![](img/2508606813a9204c61e8132ec0695cc1_30_0.png)

Figure 1.2 – The official website of the Python programming language

Once on the home page of the website, it is then easy to navigate to either the Downloads or Documentation tabs. For the purposes of this book, we recommend that you download Python version 3.8.2.

Another way of downloading the Python installer is via the use of Continuum Analytics’ Anaconda cross-platform Python distribution. Unlike the official Python installer, the Anaconda installer comes with additional Python packages by default. This makes package management less cumbersome than in the former case. The official website of Anaconda is https://www.anaconda.com/.

Once the appropriate Python installers are downloaded, they can then be installed on the target machine, depending on the type of operating system being used.

> **Note**
> In the preparation of this book, a 64-bit Ubuntu 20.04 LTS operating system was used.

Furthermore, in order to install additional Python packages, a **pip Python installer program** can be used. The pip command is run from the computer's terminal. The syntax for pip package installation is shown as follows:

```
pip install PackageName
```

In order to update an already-installed package using pip, the following command is used:

```
pip install PackageName --upgrade
```

The QIP frameworks that need to be included for use in this book include the following:

- **IBM's Qiskit**: https://qiskit.org/
- **Google's Cirq**: https://cirq.readthedocs.io/en/stable/
- **Xanadu's PennyLane**: https://pennylane.ai/
- **Xanadu's Strawberry Fields**: https://strawberryfields.readthedocs.io/en/stable/
- **QuTiP**: http://qutip.org/

As an alternative to installing Python packages on a local computer machine, Google's **Colaboratory** or **Colab** environment can be used. The official website of Google Colab is https://colab.research.google.com.

The command that is used to install Python packages on Google Colab is similar to the pip command, the exception being that unlike the pip command, the Google Colab command is preceded by the ! symbol. Thus, the Google Colab command used to install a package called PackageName is as follows:

```
!pip install PackageName
```

The command used to update an already-existing Python package is as follows:

```
```python
!pip install PackageName --upgrade
```
```

Now that we have understood the process of the installation of Python packages, let's explore `pipenv`.

## pipenv and creating the environment

Python packages can be installed using installation tools such as pip. In some instances, it might be necessary to separate certain Python packages from the main Python installation. The reason for this might include the need to avoid some installation conflicts and to offer protection to the main Python installation against the installation of unstable packages. Examples of the tools that are used to isolate the main Python installation in this sense include `virtualenv` and `venv`.

Normally, in order to manage Python packages and dependencies, different Python libraries are used. For instance, `pip` is used for package installation, update, and uninstallation, while another tool such as `venv` is used to manage virtual environments. These packages are typically used separately. This in practice makes the management of a Python project very cumbersome. The solution to this is the use of a tool that can do the tasks of package management (installation, updating, and uninstallation), along with virtual environment creation and support. The appropriate tool for this is `pipenv`.

`pipenv` is a Python packaging tool. This tool is used to manage and simplify Python package dependencies.

It can be installed using the following command:

```
```bash
pip install pipenv
```
```

Once `pipenv` is installed, it replaces pip as a package management tool. Furthermore, it becomes responsible for all the package dependency tasks. It also creates two new files, namely the following:

- `pipfile`: Gives a list of all installed packages
- `pipfile.lock`: Used to manage complex dependencies

With that, we have reached the end of the chapter.

## Summary

In this introductory chapter, we provided an overview of QIP. We introduced quantum cryptography, quantum computing, and quantum game design.

We also discussed the difference between discrete-variable QIP and continuous-variable QIP. Furthermore, we introduced the concept of a qubit, which is analogous to a conventional bit (binary digit) used by conventional computers.

Furthermore, we discussed the benefits of using the Python programming language for QIP. Additionally, we discussed the installation of the Python programming language and Python packages both on a local machine and in a cloud-based Google Colab environment. We also discussed how package management can be done by using the `pipenv` tool.

Having introduced QIP and all the necessary tools of QIP, the next chapter will cover the basic mathematics necessary to understand the subsequent chapters. Python will be used as a tool for the hands-on demonstrations of the concepts being discussed.

## Further reading

- Nielsen, M. A., & Chuang, I. L. (2011). *Quantum computation and quantum information: 10th Edition.* New York, NY, USA: Cambridge University Press, 1107002176, 9781107002173.
- Wilde, M. M. (2017). *Quantum information theory.* Cambridge University Press.
- Khan, F. S., Solmeyer, N., Balu, R., & Humble, T. S. (2018). *Quantum games: a review of the history, current state, and interpretation.* Quantum Information Processing, 17 (11), 309.
