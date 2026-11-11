
After you load System.Security Assembly, you can start creating the encryption function. RijndaelManaged encryption requires a four-step process to encrypt the strings, which is represented in the preceding diagram.

The RijndaelManaged encryption process is as follows:

- 1. The process starts by creating the encryptor. The encryptor is derived from the encryption key (password and salt) and initialization vector.
- 2. After you define the encryptor, you will need to create a new memory stream using the IO.MemoryStream object. A memory stream is what stores values in memory for use by the encryption assembly.
- 3. Once the memory stream is open, you define a System.Security.Cryptography.CryptoStream object. The CryptoStream is the mechanism that uses the memory stream and the encryptor to transform the unencrypted data to encrypted data. In order to leverage the CryptoStream, you need to write data to the CryptoStream.
- 4. The final step is to use the IO.StreamWriter object to write the unencrypted value into the CryptoStream. The output from this transformation is placed into MemoryStream. To access the encrypted value, you read the data in the memory stream.

> To learn more about the System.Security.Cryptography.RijndaelManaged class, you can view the following MSDN article: https://msdn.microsoft.com/en-us/library/system.security.cryptography.rijndaelmanaged(v=vs.110).aspx.

To create a script that encrypts strings using RijndaelManaged encryption, perform the following:

```
Add-Type -AssemblyName System.Security
function Encrypt-String { param($String, $Pass,
$salt="CreateAUniqueSalt", $init="CreateAUniqueInit")
try{
    $r = new-Object System.Security.Cryptography.RijndaelManaged
    $pass = [Text.Encoding]::UTF8.GetBytes($pass)
    $salt = [Text.Encoding]::UTF8.GetBytes($salt)
    $init = [Text.Encoding]::UTF8.GetBytes($init)
$r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 50000).GetBytes(32)
$r.IV = (new-Object Security.Cryptography.SHA1Managed).ComputeHash($init)[0..15]

$c = $r.CreateEncryptor()
$ms = new-Object IO.MemoryStream
$cs = new-Object Security.Cryptography.CryptoStream $ms,$c,"Write"
$sw = new-Object IO.StreamWriter $cs
$sw.Write($String)
$sw.Close()
$cs.Close()
$ms.Close()
$r.Clear()
[byte[]]$result = $ms.ToArray()
}
catch {
    $err = "Error Occurred Encrypting String: $_"
}
if($err) {
    # Report Back Error
return $err
}
else {
return [Convert]::ToBase64String($result)
}
}
Encrypt-String "Encrypt This String","A_Complex_Password_With_A_Lot_Of_Characters"
```

The output of this script would look like the following:

![](img/a27a138d6586c10902d8e86ab3384834_63_0.png)

This function displays how to encrypt a string leveraging the RijndaelManaged encryption algorithm. You first start by importing the System.Security assembly by leveraging the Add-Type cmdlet, using the -AssemblyName parameter with the System.Security argument. You then declare the function of Encrypt-String. You include a parameter block to accept and set values into the function. The first value is $string, which is the unencrypted text. The second value is $pass, which is used for the encryption key. The third is a predefined $salt variable set to "CreateAUniqueSalt". You then define the $init variable, which is set to "CreateAUniqueInit".

After the parameter block, you declare try { to handle any errors in the .NET assembly. The first step is to declare the encryption class using the new-Object cmdlet with the System.Security.Cryptography.RijndaelManaged argument. You place this object inside the $r variable.

You then convert the $pass, $salt, and $init values to the character encoding standard of UTF8 and store the character byte values in a variable. This is done by specifying [Text.Encoding]::UTF8.GetBytes($pass) for the $pass variable, [Text.Encoding]::UTF8.GetBytes($salt) for the $salt variable, and [Text.Encoding]::UTF8.GetBytes($init) for the $init variable.

After setting the proper character encoding, you proceed to create the encryption key for the RijndaelManaged encryption algorithm. This is done by setting the RijndaelManaged $r.Key attribute to the object created by (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 50000).GetBytes(32). This object leverages the Security.Cryptography.PasswordDeriveBytes class and creates a key using the $pass variable, $salt variable, "SHA1" hash name, and iterating the derivative 50000 times. Each iteration of this class generates a different key value, making it more complex to guess the key. You then leverage the .Get-Bytes(32) method to return the 32-byte value of the key.

> RijndaelManaged 256-bit encryption is a derivative of the 32 bytes in the key. 32 bytes times 8 bits per byte is 256 bits.

To create the initialization vector for the algorithm, you set the RijndaelManaged $r.IV attribute to the object created by (new-Object Security.Cryptography.SHA1Managed).ComputeHash($init)[0..15]. This section of the code leverages Security.Cryptography.SHA1Managed and computes the hash based on the $init value. When you invoke the [0..15] range operator, it will obtain the first 16 bytes of the hash and place them into the $r.IV attribute.

> The RijndaelManaged default block size for the initialization vector is 128 bits. 16 bytes times 8 bits per byte is 128bits.

After setting up the required attributes, you are now ready to start encrypting data. You first start by leveraging the $r RijndaelManaged object with the $r.Key and $r.IV attributes defined. You use the $r.CreateEncryptor() method to generate the encryptor.

Once you've generated the encryptor, you have to create a memory stream to do the encryption in memory. This is done by declaring the new-Object cmdlet, set to the IO.MemoryStream class, and placing the memory stream object in the $ms variable.

Next, you create CryptoStream. CryptoStream is used to transform the unencrypted data into the encrypted data. You first declare the new-Object cmdlet with the Security.Cryptography.CryptoStream argument. You also define the memory stream of $ms, the encryptor of $c, and the operator of "Write" to tell the class to write unencrypted data to the encryption stream in memory.

After creating CryptoStream, you are ready to write the unencrypted data into CryptoStream. This is done using the IO.StreamWriter class. You declare a new-Object cmdlet with the IO.StreamWriter argument, and define CryptoStream of $cs for writing.

Last, you take the unencrypted string stored in the $string variable, and pass it into the StreamWriter $sw with $sw.Write($String). The encrypted value is now stored in the memory stream. To stop the writing of data to the CryptoStream and MemoryStream, you close the StreamWriter with $sw.Close(), close the CryptoStream with $cs.Close() and the memory stream with $ms.Close(). For security purposes, you also clear out the encryptor data by declaring $r.Clear().

After the encryption process is done, you will need to export the memory stream to a byte array. This is done by calling the $ms.ToArray() method and setting it to the $result variable with the [byte[]] data type. The contents are stored in a byte array in $result.

This section of the code is where you declare your catch { statement. If there were any errors in the encryption process, the script will execute this section. You declare the variable of $err with the "Error Occurred Encrypting String: $_" argument. The $_ will be the pipeline error that occurred during the try {} section. You then create an if statement to determine whether there is data in the $err variable. If there is data in $err, it returns the error string to the script.

If there were no errors, the script will enter the else { section of the script. It will convert the $result byte array to Base64String by leveraging [Convert]::ToBase64String($result). This converts the byte array to string for use in your scripts.

After defining the encryption function, you call the function for use. You first start by calling Encrypt-String followed by "Encrypt This String". You also declare the second argument as the password for the encryptor, which is "A_Complex_Password_With_A_Lot_Of_Characters". After execution, this example receives the encrypted value of hK7GHaDD1FxknHu03TYAPxbfAAZeJ6KTSH1nSCPpJ7c= generated from the function. Your results will vary depending on your salt, init, and password you use for the encryption algorithm.

> While this example uses the $salt of "CreateAUniqueSalt", the $init of "CreateAUniqueInit", and the password of "A_Complex_Password_With_A_Lot_Of_Characters", it is strongly recommended to change these values for your own scripts. These should use randomly generated strong values as described in the Creating random salt, initialization vector, and passwords section. Failure to use strong values and failure to keep these values in a safe location may compromise your encrypted data.

![](img/a27a138d6586c10902d8e86ab3384834_65_0.png)
