First we need to add the .NET Core package source to the list used by APT. The package source is on Azure, so we write this URL (plus some other info) to a file in the folder of sources used by apt-get. We do this by redirecting the standard output of echo to it, using the following command:

```
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
```

All packages are signed with public key cryptography to verify their integrity (read the later chapters for more on this). We import the public key for the .NET Core packages from the Ubuntu key server with the apt-key command:

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

We need to update our sources so that the latest changes can be picked up with this simple command:

```
sudo apt-get update
```

The update command is different to the upgrade command, but they are often used together. While update simply refreshes the information that APT holds about packages, upgrade allows you to very easily update all of the software on the machine (that hasn't been manually installed) to the latest versions. This can be quite a time-saver.

Now we are in a position to install .NET Core with the following command:

```
sudo apt-get install dotnet-sdk-2.0.0
```

If the SDK package ever makes it to the default list of sources, then this will be all you need to do.

If you are trying out a preview, then the package will have the preview tag suffixed (for example, -preview2-006497).

After a wall of text scrolls, assuming you don't get any errors and you see the telemetry notice, you're ready to go. Type dotnet to see the usage information and dotnet --version to see the current version that you are running.