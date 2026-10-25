
package localhost.SmartPayments;

public interface CreditCardValidatorSoap extends java.rmi.Remote {

    /**
     * Returns the card issuer Visa, MasterCard, AMEX, etc., based
     * on the card number.
     */
    public java.lang.String getCardType(java.lang.String cardNumber)
        throws java.rmi.RemoteException;

    /**
     * Returns (T/F) if the card is a known commercial card
     * (commercial
     * cards require customer code and sales tax amount to receive
     * preferred
     * discount rate pricing.)
     */
    public boolean isCommercialCard(java.lang.String cardNumber)
        throws java.rmi.RemoteException;

    /**
     * Validates the credit card by checking the card length based
     * on the card type, performs a mod 10 checksum and validates the
     * expiration
     * date. Returns 0 if good, 1001 - no card number, 1002 - no exp
     * date,
     * 1003 - invalid card type, 1004 - invalid card length,
     * 1005 - bad mod
     * 10 check, 1006 - bad expiration date.
     */
    public int validCard(java.lang.String cardNumber, java.lang.String
        expDate) throws java.rmi.RemoteException;

    /**
     * Validates the credit card length by checking the card length
     * based on the card type, Returns (T/F).
     */
    public boolean validCardLength(java.lang.String cardNumber)
        throws java.rmi.RemoteException;

    /**
     * Validates the expiration date by making sure it is a valid
     * date and the card has not expired, Returns (T/F).
     */
    public boolean validExpDate(java.lang.String expDate)
        throws java.rmi.RemoteException;
}

/**
 * Validates the credit card by performing a mod 10 checksum on
 * the card number, Returns (T/F).
 */
public boolean validMod10(java.lang.String cardNumber)
    throws java.rmi.RemoteException;

/**
 * Lookup the Debit Network ID using a Card Number. Network ID
 * is a 3 characters string. If there is a match, the card can
 * be used as a Debit Card and processed through the Debit
 * network. Possible
 * Network ID: ACL — Accel, AFN - AFFN, AKO — Alaska Option,
 * C24 — CU24,
 * ILK — Interlink, JEN - Jeanie, MAC — Star Northeast (MAC),
 * MAE — Maestro,
 * NET - NETS, NYC — NYCE, PUL — Pulse, SES — Star
 * Southeast, SHZ — Shazam,
 * STX — Star West, TYM - TYME
 */
public localhost.SmartPayments.Response getNetworkID(java.
lang.String userName, java.lang.String password, java.lang.String
cardNumber) throws java.rmi.RemoteException;
}

There are several methods defined in the class, but we use only the validCard() method, which returns 0 if the credit card is valid.

The source code for the CreditCardValidatorSoapProxy class is explained in the following snippet. This class implements the the CreditCardValidatorSoap interface.

The source code of CreditCardValidatorSoapProxy is shown here for example purposes, so we can really appreciate the tooling that is provided by Eclipse. Developing web services clients manually would mean quite a lot of coding and understanding of SOAP and other related web service standards, but since Eclipse has the tools to generate the necessary implementation classes from the WSDL, we don't really have to worry about SOAP details or other web services "stuff" (other than how to use the client, of course).

```
package localhost.SmartPayments;

public class CreditCardValidatorSoapProxy implements localhost.
SmartPayments.CreditCardValidatorSoap {
    private String _endpoint = null;

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

public localhost.SmartPayments.CreditCardValidatorSoap
getCreditCardValidatorSoap() {
    if (creditCardValidatorSoap == null)
        _initCreditCardValidatorSoapProxy();
    return creditCardValidatorSoap;
}

public java.lang.String getCardType(java.lang.String cardNumber)
throws java.rmi.RemoteException{
    if (creditCardValidatorSoap == null)
        _initCreditCardValidatorSoapProxy();
    return creditCardValidatorSoap.getCardType(cardNumber);
}

public boolean isCommercialCard(java.lang.String cardNumber) throws
java.rmi.RemoteException{
    if (creditCardValidatorSoap == null)
        _initCreditCardValidatorSoapProxy();
    return creditCardValidatorSoap.isCommercialCard(cardNumber);
}

public int validCard(java.lang.String cardNumber, java.lang.String
expDate) throws java.rmi.RemoteException{
    if (creditCardValidatorSoap == null)
        _initCreditCardValidatorSoapProxy();
    return creditCardValidatorSoap.validCard(cardNumber, expDate);
}

public boolean validCardLength(java.lang.String cardNumber) throws
java.rmi.RemoteException{
    if (creditCardValidatorSoap == null)
        _initCreditCardValidatorSoapProxy();
    return creditCardValidatorSoap.validCardLength(cardNumber);
}

public boolean validExpDate(java.lang.String expDate) throws java.
rmi.RemoteException{
    if (creditCardValidatorSoap == null)
        _initCreditCardValidatorSoapProxy();
    return creditCardValidatorSoap.validExpDate(expDate);
}

public boolean validMod10(java.lang.String cardNumber) throws java.
rmi.RemoteException{
```

```java
if (creditCardValidatorSoap == null)
    _initCreditCardValidatorSoapProxy();
return creditCardValidatorSoap.validMod10(cardNumber);
}

public localhost.SmartPayments.Response getNetworkID
    (java.lang.String userName, java.lang.String password,
     java.lang.String cardNumber) throws java.rmi.RemoteException{
    if (creditCardValidatorSoap == null)
        _initCreditCardValidatorSoapProxy();
    return creditCardValidatorSoap.getNetworkID(userName, password,
                                                cardNumber);
}
}
```

The above proxy class is automatically generated by Eclipse tooling, and in most cases, knowledge of the details of the generated classes is not needed. The proxy class basically initializes yet another automatically generated class for the CreditCardValidatorSoap interface that has the code to actually call a remote web service.

## Implementing the Web Service Call

The CreditCardValidatorSoapProxy class will be used in our example. We modify the FormHandler class and replace the implementation of the submitOrder() method with the following code:

```
```java
public boolean submitOrder(String name, String address,
                           String creditCardNumber, String expiryDate) {
    CreditCardValidatorSoapProxy ccValidatorProxy = new
    CreditCardValidatorSoapProxy();
    int rv = -1;
    try {
        rv = ccValidatorProxy.validCard(creditCardNumber,
                                        expiryDate.replace("/",
                                                           ""));
        if (rv != 0) {
            System.out.println("Credit card check failed: " + rv);
        }
    } catch (RemoteException e) {
        e.printStackTrace();
    }
    return rv == 0;
}
```
```
