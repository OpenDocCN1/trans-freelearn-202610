Let's go over the steps:

1. Return to your command-line interface and run the following command to download the nvm installation script:

```
curl -sL https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh
```

2. Then, use the following command to execute the script:

```
bash install.sh
```

This command will clone the nvm repository under the ~/.nvm folder and update the ~/.profile file as necessary.

3. To begin utilizing nvm, just source the following file or log out and then log back in:

```
source ~/.profile
```

4. Using this command, you can quickly retrieve a list of available Node.js versions that you can install:

```
nvm ls-remote
```

5. We are using v12.22.1 in this book, which you can install using the following command:

```
nvm install v12.22.1
```

This will download the installation binary that is compatible with your operating system and install Node.js together with npm. It will also make this version the default version.

6. To verify the installed version, use the following command:

```
node -v
```

In our case, we get v12.22.1 printed on the Terminal.

More information on the available commands and how to use them may be found in the official repository at https://github.com/nvm-sh/nvm.