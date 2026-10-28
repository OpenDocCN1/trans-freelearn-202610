# What just happened?

We've now added `limit-offset` paging capabilities to both of our Requestors:
the HTTP Requestor that we built from scratch, and the SDK Requestor that's based
on the official SDK.

To test this, create a new `GraphRequest` instance in `CustomGraphContainerController.as`, used after the current Requestor has been initialized:

```
private function onRequestorInitialize(a_event:Event):void
{
    _requestor.attemptToAuthenticate(ExtendedPermissions.READ_STREAM);
    var testRequest:GraphRequest = new GraphRequest("me", "likes");
    testRequest.limit = 20;
    testRequest.offset = 50;
    _requestor.request(testRequest);
}
```

Don't forget to test it with both Requestors!

# Date-Based filtering

Do you notice any potential problems with the paging system outlined above (Other than that it doesn't work as expected at the moment)?

What about for getting the list of comments on a particularly popular post?

Suppose you wish to get the 100 most recent comments on a post, and need to use paging to do so. You request comments 1-25, 26-50, and then 51-75. However, at this point, someone else posts a comment, which is added as the new #1. The old #1 becomes the new #2, the old #2 becomes the new #3, and so on, so that when you request 76-100, you've actually received comment #76 twice – it was #75 a few seconds ago!

Worse than that, you now don't have the new comment #1. So instead, you could try obtaining the pages in reverse order: 76-100, 51-75, and then 26-50. This time, if a new comment is added as #1, the old #25 becomes #26. But you now request comments 1-25; the old #25 doesn't make it into your list, so there's a gap. And that's not even mentioning what would happen if a comment were deleted.

Realistically, these aren't huge problems; they just serve to show that the `limit-offset` paging method is not perfect. As an alternative, we can page data based on time and date.