With all the components nicely separated, we can later swap InMemoryUserDetailsService with DatabaseUserDetailsService when we are ready to go online. We can also replace PlaintextPasswordEncoder with either ShaPasswordEncoder or Md5PasswordEncoder, to easily support different hashing algorithms.

Let's define the last filter needed in our chain: filter_security_interceptor.

```
@Object
def filter_security_interceptor(self):
    filter = FilterSecurityInterceptor()
    filter.auth_manager = self.auth_manager()
    filter.access_decision_mgr = self.access_decision_mgr()
    filter.sessionStrategy = self.session_strategy()
    filter.obj_def_source = [
        ("edit.*", ["ROLE_EDIT"]),
        ("/.*", ["ROLE_READ"])
    ]
    return filter
```

```
@Object
def access_decision_mgr(self):
    access_decision_mgr = AffirmativeBased()
    access_decision_mgr.allow_if_all_abstain = False
    access_decision_mgr.access_decision_voters = [RoleVoter()]
    return access_decision_mgr
```

filter_security_interceptor is the last security check in our app. Its job is to make sure the user is authorized to complete the request. It looks at the URL of the web request, and then goes down its list of URL patterns until it finds a match. When it does, it sees a list of roles. The roles along with the user's credentials are given to the access_decision_mgr, who submits a request for a vote from each of its access_decision_voters. In our configuration, we are using RoleVoter, which votes on whether or not the user has the role supplied in the list.

access_decision_mgr tallies up the votes, and decides if access is granted based on the policy. In our case, we have an AffirmativeBased policy, meaning that we need only one up vote, i.e. we only need to have one of the roles in the list. If we switched to a UnanimousBased policy, we would be required to have all the roles. A ConsensusBased policy would require that we have a majority of the roles.