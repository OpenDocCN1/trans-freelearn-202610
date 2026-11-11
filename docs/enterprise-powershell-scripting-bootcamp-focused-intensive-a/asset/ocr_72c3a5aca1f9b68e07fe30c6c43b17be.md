# Answer file reading function

You continue with the scanning script by defining the answer file reading function, loading the XML into memory, and removing the answer file. You may choose to comment out the Remove-Item cmdlet step. This will provide for testing in your environment without having to recreate the XML file in the script directory.

To create an XML reading function, load the XML into memory, and delete the XML file, you can perform the following:

```
Function read-xmltag { param ($xmlextract)
return $xml.GetElementsByTagName("$xmlextract")
}

$xmlfile ="$PSScriptRoot\Scan_Answers.xml"

$test = test-path $xmlfile

if (!$test) {
    Write-Error "$xmlfile not found on the system. Select any key to exit!"
    PAUSE
    exit
}

[xml] $xml = get-content $xmlfile
Remove-Item $xmlfile -force
```

There is no output to display from creating this function.

In this example, you define the answer file reading function:

+   1. You first start by defining the read-xmltag function with a parameter block of param ($xmlextract).
+   2. You then leverage the $xml.GetElementsByTagName() method to obtain the xml tag specified in the $xmlextract.
+   3. You take the result and return it to the portion of the script that called the function. You then continue to gather the current path of the script by calling $PSScriptRoot and combining the value with "\Scan_Answers.xml". You store the result in the $xmlfile variable.
+   4. You continue to determine if the file exists by leveraging the test-path cmdlet with the $xmlfile argument.