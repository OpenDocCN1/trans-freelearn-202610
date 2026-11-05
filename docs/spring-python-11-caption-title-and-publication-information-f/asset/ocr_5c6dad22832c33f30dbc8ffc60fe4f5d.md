```
raise cherrypy.HTTPRedirect(
    "?login=$s&error_msg=Username/password failure"
    % login)

return """
    %s<p>
    <form method="POST" action="">
    <table>
        <tr>
            <td>Login:</td>
            <td><input type="text" name="login"
                value="%s"/></td>
        </tr>
        <tr>
            <td>Password:</td>
            <td>
                <input type="password" name="password"/>
            </td>
        </tr>
    </table>
    <input type="hidden" name="from_page"
        value="%s"/><br/>
    <input type="submit"/>
    </form>
""" % (error_msg, login, from_page)

def attempt_auth(self, username, password):
    token = UsernamePasswordAuthenticationToken(username,
        password)
    SecurityContextHolder.getContext().authentication = \
        self.auth_provider.authenticate(token)
    self.http_context_filter.saveContext()

@cherrypy.expose
def logout(self):
    self.filter.logout()

    self.http_context_filter.saveContext()
    raise cherrypy.HTTPRedirect("/")
```