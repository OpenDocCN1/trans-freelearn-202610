Utilizing OID C, Consul can validate users attempting to access Consul by querying one of the many OID C providers (most enterprise environments utilize at least one). If you've ever logged in to a service and were redirected to another authentication page or popup for your Google, Yahoo, Microsoft, PayPal, or Amazon credentials, it is likely that you've utilized OID C for that authentication. Other popular OID C providers for enterprise environments include OKTA and Auth0. This process not only validates and authenticates the user, but it can also map that user to a set of authorization policies to control access to the system. After all, if you ever have to give a manager access to your Consul cluster, you certainly don't want them having administrative privileges! So, let's walk through this with another lovely example.

As part of this story, we need to assume that our greater employer has implemented OID C and that each of us has our own credentials to authenticate with that OID C provider. Along with our credentials, we all have specific metadata that is associated with our position in the company:

| Credentials | Metadata |
| Username | Password | First Name | Last Name | Position |
| sharris | ********** | Steve | Harris | Bassist |
| bdickinson | ********** | Bruce | Dickinson | Singer |
| dmurray | ********** | Dave | Murray | Guitarist |
| asmith | ********** | Adrian | Smith | Guitarist |
| nmcbrain | ********** | Nicko | McBrain | Drummer |

Figure 3.6 – OID C accounts and metadata

In this example, everybody has their own unique position, except for Adrian and Dave. They are both in the guitarist group, which we're going to map to a role within Consul. Recall that a role consists of a group of ACL policies within Consul. When we configure Consul to utilize this information, we create what's called a claim mapping. This maps the metadata that's been provisioned within the OID C provider to variables within Consul, which can then be used to associate that metadata with a particular role. For example, the following configuration associates specific mappings for the first and last name, and the position metadata is associated with a list in Consul called band_positions:

```
{
"OIDCDiscoveryURL": "https://<OIDC_URL>/",
"OIDCClientID": "<OIDC_CLIENT_ID>",
```