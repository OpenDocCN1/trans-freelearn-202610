```
private localhost.SmartPayments.CreditCardValidatorSoap
    creditCardValidatorSoap = null;

public CreditCardValidatorSoapProxy() {
    _initCreditCardValidatorSoapProxy();
}

public CreditCardValidatorSoapProxy(String endpoint) {
    _endpoint = endpoint;
    _initCreditCardValidatorSoapProxy();
}

private void _initCreditCardValidatorSoapProxy() {
    try {
        creditCardValidatorSoap = (new localhost.SmartPayments.
            CreditCardValidatorLocator()).
            getCreditCardValidatorSoap();
        if (creditCardValidatorSoap != null) {
            if (_endpoint != null)
                ((javax.xml.rpc.Stub)creditCardValidatorSoap)._
                    setProperty("javax.xml.rpc.service.endpoint.address",
                        _endpoint);
            else
                _endpoint = (String)((javax.xml.rpc.Stub)
                    creditCardValidatorSoap).
                    _getProperty("javax.xml.rpc.service.
                        endpoint.address");
        }
    }
    catch (javax.xml.rpc.ServiceException serviceException) {}
}

public String getEndpoint() {
    return _endpoint;
}

public void setEndpoint(String endpoint) {
    _endpoint = endpoint;
    if (creditCardValidatorSoap != null)
        ((javax.xml.rpc.Stub)creditCardValidatorSoap)._
            setProperty("javax.xml.rpc.service.endpoint.address",
                _endpoint);
}
```