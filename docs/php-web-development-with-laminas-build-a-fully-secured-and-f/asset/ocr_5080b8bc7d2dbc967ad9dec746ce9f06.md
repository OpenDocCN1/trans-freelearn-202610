```
$this->encryptionMethod = $encryptionMethod;
$this->auth = new AuthenticationService ();
}
```

The IdentityManager class will have a login method, which will encapsulate most of the code from the IndexController->loginAction method:

```
public function login(string $identity, string
$credential): bool
{
	$this->adapter->setIdentity($identity);
	$credential = call_user_func($this->
	encryptionMethod, $credential);
	$this->adapter->setCredential($credential);
	$this->auth->setAdapter($this->adapter);
	$result = $this->auth->authenticate ();
	if ($result->isValid()) {
		error_log('user ' . $result->getIdentity() . '
		is logged');
		$this->auth->getStorage ()->write($this->
		adapter->getResultRowObject(null,'password'));
		$this->loadAuthorizationData ();
		return true;
	} else {
		foreach ($result->getMessages() as $message) {
			error_log($message);
		}
	}
	return false;
}
```

Observe that there are some differences introduced into the authentication process encapsulated by IdentityManager->login. The first difference is the use of an injected method to encrypt the credential. This change makes this class more flexible because there is no coupling with the Employee class. It is easier to modify the encryption method without writing into the IdentityManager class.