Since we are using a recent version of Ubuntu or Debian, `python-bs4` will be listed in the `apt` repository. But if the preceding command fails with `Package Not Found Error`, it means that the package list is not up-to-date. This normally happens if we have just installed our operating system and the package list is not downloaded from the package repository. In this case, we need to first update the package list using the following command:

```
sudo apt-get update
```

The preceding command will update the necessary package list from the online package repositories. After this, we need to try the preceding command to install Beautiful Soup.

In the older versions of the Linux operating system, even after running the `apt-get update` command, we might not be able to install Beautiful Soup because it might not be available in the repositories. In these scenarios, we can rely on the other methods of installation using either `pip` or `easy_install`.

# Installing Beautiful Soup using pip or easy_install

The `pip` and `easy_install` are the tools used for managing and installing Python packages. Either of them can be used to install Beautiful Soup.

# Installing Beautiful Soup using pip

From the terminal, type the following command:

```
sudo pip install beautifulsoup4
```

The preceding command will install Beautiful Soup Version 4 in the system after downloading the necessary packages from `http://pypi.python.org/`.

# Installing Beautiful Soup using easy_install

The `easy_install` tool installs the package from **Python Package Index** (PyPI). So, in the terminal, type the following command:

```
sudo easy_install beautifulsoup4
```