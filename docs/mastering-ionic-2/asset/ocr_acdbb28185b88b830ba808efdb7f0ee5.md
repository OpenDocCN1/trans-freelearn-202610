```
auth$.subscribe((state: FirebaseAuthState) => {
    console.log(state);
    this.authState = state;
});
}

get authenticated(): boolean {
    return this.authState !== null;
}

signInWithFacebook(): firebase.Promise<FirebaseAuthState |
    FacebookLoginResponse> {
    if (this.platform.is('cordova')) {
        return this.fb.login(['email',
'public_profile']).then(res
            => {
            const facebookCredential =
                firebase.auth.FacebookAuthProvider.credential
                (res.authResponse.accessToken);
            return firebase.auth()
                .signInWithCredential(facebookCredential);
        });
    } else {
        return this.auth$.login();
    }
}

signOut(): void {
    this.auth$.logout();
}

authData(): any {
    if (this.authState != null) {
        return this.authState.auth;
    } else {
        return '';
    }
}
}
```

We initially installed in app browser Cordova plugin to handle this authentication and redirects.

We have also checked that, if we are on a real device, we use the Ionic native plugin for Facebook. Here we first call the Facebook plugin, and after a successful login, and a token is received from Facebook, we pass it to the Firebase `firebase.auth.FacebookAuthProvider` function for authentication. We have used Facebook login in our TasteBite application, although if we want to we can implement other providers also.