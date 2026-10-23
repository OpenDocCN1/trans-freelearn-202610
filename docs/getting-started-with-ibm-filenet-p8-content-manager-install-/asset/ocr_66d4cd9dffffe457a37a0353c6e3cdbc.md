# Enterprise mandates

The DUCK application will be subject to the same mandates for localization, accessibility, and other standards that all of our enterprise applications must meet. The DUCK prototype does not implement most of those things.

Just to make a point about CM, however, it is easy to see partial localization if you change the preferred locale in your web browser. CE property and class display names are localizable, and localizations are provided by IBM for the display names of system properties and classes for several locales. Here, for example, is a screenshot of the edit screen for the same document as above, but with the web browser locale set to Chinese (locale zh-CN). No software changes are needed in the application to get those localized names because DUCK passes the browser locale into the CE API, and the CE automatically returns localized display names appropriate to that locale.

![](bbox=[0.15, 0.37, 0.82, 0.63])

# Navigational overview

The following figure illustrates the basic navigational transitions between pages in the DUCK application. If you are familiar with editing JSF navigation rules, this may look familiar to you. The four rectangles (BrowseQuery, AddDoc, ViewDoc, and EditDoc) represent the principal screens of the DUCK application. The labeled arrows correspond to user actions which trigger transitions to another screen or back to the same screen.