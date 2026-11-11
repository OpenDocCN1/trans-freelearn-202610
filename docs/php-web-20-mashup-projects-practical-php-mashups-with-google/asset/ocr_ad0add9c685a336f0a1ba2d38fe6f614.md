```
<?php

$params = array('nCelcius' => 37);

$client = new SoapClient(
'http://webservices.daelab.net/temperatureconversions/
TemperatureConversions.wso?WSDL',
array('trace' => true)
);
try {
    $result = $client->CelciusToFahrenheit($params);
} catch (SoapFault $e) {
    echo "Error!<br>";
    echo "faultcode: " . $e->faultcode . "<br>";
    echo "faultstring: " . $e->faultstring. "<br>";
    echo "detail: " . $e->detail;
}
?>
<html>.
<head><title>Soap Test</title></head>
<body>
<?php if ($result) { ?>
    Fahrenheit: <?= $result->CelciusToFahrenheitResult ?>
<?php } else { ?>
    Problem with conversion.
<?php } ?>
</body>
</html>
```

This script is named SoapClientTest2.php. Running it on our web browser will show us this:

![](bbox=[0.17, 0.62, 0.83, 0.8])