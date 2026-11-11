+   5. You declare an if not exist statement on the $test variable and, if the file does not exist, you use the write-error cmdlet to display "$xmlfile not found on the system. Select any key to exit."
+   6. You then issue a PAUSE statement to stop the script, and an exit statement to exit the script after a key is pressed on the keyboard. If the answer file exists, the script will continue to load the XML file into memory.
+   7. You use the get-content cmdlet with the argument of $xmlfile.
+   8. You then place this value into an $xml variable with the data type of [xml].
+   9. After loading the XML file in memory, you remove the XML file by leveraging the Remove-Item cmdlet with the $xmlfile argument and the -Force parameter. The answer file will be removed from the system.

# Decryption function

The next step after you are done loading the XML file into memory is defining the decryption function. Since the answer file has encoded and encrypted values, you will need this function to translate the encrypted values into strings.

To declare the decryption function, you can perform the following:

```
Add-Type -AssemblyName System.Security
function Decrypt-String { param($Encrypted)
    if($Encrypted -is [string]){
        $Encrypted = [Convert]::FromBase64String($Encrypted)
    }
    $r = new-Object System.Security.Cryptography.RijndaelManaged
    $pass = [System.Text.Encoding]::UTF8.GetBytes($pass)
    $salt = [System.Text.Encoding]::UTF8.GetBytes($salt)
    $init = [Text.Encoding]::UTF8.GetBytes($init)
    $r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 50000).GetBytes(32)
    $r.IV = (new-Object Security.Cryptography.SHA1Managed).ComputeHash($init)[0..15]
    $d = $r.CreateDecryptor()
    $ms = new-Object IO.MemoryStream @($Encrypted)
    $cs = new-Object Security.Cryptography.CryptoStream $ms,$d,"Read"
    $sr = new-Object IO.StreamReader $cs
    try {
```