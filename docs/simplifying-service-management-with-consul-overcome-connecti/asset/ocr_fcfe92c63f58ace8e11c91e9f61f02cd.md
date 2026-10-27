| Number of Servers | Quorum | Failure Tolerance |
|---|---|---|
| 1 | 1 | 0 |
| 2 | 2 | 0 |
| 3 | 2 | 1 |
| 4 | 3 | 1 |
| 5 | 3 | 2 |
| 6 | 3 | 2 |
| 7 | 4 | 3 |

Figure 2.11 – Never fail for quorum

So, let's look at how that election is performed. For a really cool animated view of the process, the following website is fantastic: http://thesecretlivesofdata.com/raft/.

Walking through the election, we're going to look at three servers: **Bart**, **Lisa**, and **Maggie**. **Bart** is the first server to recognize consciousness, so he submits himself as a *candidate* and sends a request to **Lisa** and **Maggie** to become the *leader*. However, before **Lisa** heard **Bart**'s request, she sent her own request for candidacy with a leader request to **Bart** and **Maggie**, because we all know **Lisa** is smarter.

You can see an overview of the process in Figure 2.12:

![](bbox=[0.11, 0.5, 0.89, 0.76])

Figure 2.12 – Lisa's nomination for election

**Maggie** hears **Lisa**'s request for leadership and responds happily. By this time, **Bart**'s request has made it to both **Lisa** and **Maggie**, but alas, he is too late. **Maggie** has already voted for **Lisa** (I mean, seriously: why would she vote for **Bart** anyway?).