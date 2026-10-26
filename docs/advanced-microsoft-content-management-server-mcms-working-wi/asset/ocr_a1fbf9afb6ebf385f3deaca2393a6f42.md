# Enhancements to the RSS Feed Generator

+   - Author name—The solution could be enhanced by integration with Active Directory to retrieve the author's full name.
+   - Channel-based feed generation—set up channel properties as configuration parameters for the feed and generate it from a posting or channel rendering script.
+   - Storing history—Organizations may have a requirement to keep a copy of all historical content that has been on their website. The RSS feed currently potentially changes every 5 minutes (when the cache expires). To make the RSS feed act as content that appears in the page revision history, the request to the feed could be made on save and the generated HTML could be saved into an HtmlPlaceholder. An autupdater tool could then be written to update the aggregated feed at a scheduled time, say every 60 minutes.

# Enhancements to the RSS Aggregator Placeholder Control

+   - Design—The RSS aggregation control only has a rudimentary look and feel that could easily be enhanced by adding class attributes to the div elements to apply styles defined in the page's CSS stylesheet.

# Summary

We have now given the Tropical Green website the ability to syndicate and aggregate content via RSS. Content authors will have the ability to specify RSS feeds to aggregate and display on specific postings. End users will now have the chance to subscribe to an RSS feed on our MCMS website so they can be automatically notified when content changes happen without having to browse to the website. In addition, an MCMS website can now aggregate content from RSS feeds coming from other servers on the Internet and display some of that website's own content.