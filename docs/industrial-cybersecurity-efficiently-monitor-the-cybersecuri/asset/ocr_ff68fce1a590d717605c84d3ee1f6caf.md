# modbus-cli

modbus-cli is a Ruby application that you can add to Kali Linux with the following command:

```
pac@KVM0101011:~$ sudo gem install modbus-cli
Successfully installed modbus-cli-0.0.14
Parsing documentation for modbus-cli-0.0.14
Done installing documentation for modbus-cli after 0 seconds
1 gem installed
```

Once installed, the command to run the Ruby tool is modbus:

```
pac@KVM0101011:~$ modbus
Usage:
    modbus [OPTIONS] SUBCOMMAND [ARG] ...

Parameters:
    SUBCOMMAND
    [ARG] ...    subcommand arguments

Subcommands:
    read        from the device
    write       to the device
    dump        copy contents of read file to the device

Options:
    -h, --help  print help
```

As you can see, the tool allows us to read and write to a Modbus server. Let's see whether we can read work register 10:

```
pac@KVM0101011:~$ modbus read -help
Usage:
    modbus read [OPTIONS] HOST ADDRESS COUNT

Parameters:
    HOST                        IP address or hostname for the Modbus device
    ADDRESS                     Start address (eg %M100,
```