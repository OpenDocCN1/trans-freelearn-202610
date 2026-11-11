In total, you will have taken all 80 characters in the $encodedvalue variable and split them into three variables for use in your script:

+   1. You then continue to encode the salt by leveraging the [System.Text.Encoding]::Unicode.GetBytes($salt) method and storing the value in the $encSalt variable. You then convert the encoded value into a string by leveraging the [Convert]::ToBase64String($encSalt) method and storing the value in the $encSalt variable.
+   2. Finally, you encode the init by leveraging the [System.Text.Encoding]::Unicode.GetBytes($init) method and storing the value in the $encInit variable. You then convert the encoded value into a string by leveraging the [Convert]::ToBase64String($encInit) method and storing the value in the $encInit variable.

# Script 2 - string encryption script

In the instance that you want to have encrypted strings in your answer files, you will be required to develop an encryption script. Using the password (AFD, SSD, and RTD), salt, and init values that you generated with script 1, you can create an encryption function. You can also create a loop for that function, which will enable you to encrypt multiple strings in one session. After you generate the encrypted strings, you can place them securely in the answer file.

![](bbox=[0.1, 0.7, 0.18, 0.76])

> To obtain a detailed explanation of encoding, encryption, and decryption, you can refer to Chapter 4, String Encryption and Decryption.

To create an encryption function to leverage the encoded salt, init, and answer file decryptor, you can do the following:

```
Add-Type -AssemblyName System.Security
function Encrypt-String { param($Sstring)
    try{
        $r = new-Object System.Security.Cryptography.RijndaelManaged
        $pass = [Text.Encoding]::UTF8.GetBytes($pass)
        $salt = [Text.Encoding]::UTF8.GetBytes($salt)
        $init = [Text.Encoding]::UTF8.GetBytes($init)

        $r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 50000).GetBytes(32)
        $r.IV = (new-Object Security.Cryptography.SHA1Managed).
```