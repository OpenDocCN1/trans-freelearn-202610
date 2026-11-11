You then continue to the random character generation. You declare the 1..30 command, which means, for numbers 1 to 30, repeat the following command. You pipe this to ForEach {, which will designate for each of the 30 iterations. You then call the $ascii array and pipe it to | get-random cmdlet. The get-random cmdlet will randomly select one of the characters in the $ascii array. This value is then joined to the existing values in the $password string using the assignment operator +=. After the 30 iterations, there will be 30 random values in the $password variable. Lastly, you leverage return $password, to return this value to the script. After declaring the function, you call the function four times using create-password. This creates four random passwords for use.

![](bbox=[0.2, 0.28, 0.27, 0.34])

> To create strings that are less than 30 random characters in length, you can modify the 1..30 to be any value that you want. If you want 15 random character Salts and Initialization Vectors, you use 1..15 instead.

# Encrypting and decrypting strings

To start using RijndaelManaged encryption, you need to import the .NET System.Security Assembly into your script. Much like importing a module to provide additional cmdlets, using .NET assemblies provide an extension to a variety of classes you wouldn't normally have access to in PowerShell. Importing the assembly isn't persistent. This means you will need to import the assembly each time you want to use it in a PowerShell session, or each time you want to run the script.

To load the .NET assembly, you can use the Add-Type cmdlet with the -AssemblyName parameter with the System.Security argument. Since the cmdlet doesn't actually output anything to the screen, you may choose to print to the screen after successful importing of the assembly.

To import the System.Security Assembly with display information, you can do the following:

```
Write-host "Loading the .NET System.Security Assembly For Encryption"
Add-Type -AssemblyNameSystem.Security -ErrorActionSilentlyContinue
-ErrorVariable err
if ($err) {
Write-host "Error Importing the .NET System.Security Assembly."
    PAUSE
    EXIT
}
```