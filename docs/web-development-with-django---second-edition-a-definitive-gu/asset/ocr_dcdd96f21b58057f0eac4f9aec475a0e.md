If we wanted to restrict a search field to an exact match rather than return results that contain the search string, then the field can be suffixed with `__exact`. So, replacing `isbn` with `isbn__exact` will require the complete ISBN to be matched, and we won’t be able to get a match using a portion of the ISBN.

Similarly, we constrain the search field to only return results that start with the search string by using the `__startswith` suffix. Qualifying the publisher name search field as `publisher_name__startswith` means that we will get results searching for “pack” but not for “ackt.”

This concludes our examination of common customizations of the change list pages. We may also want to customize the behavior of the create and update forms in our app.

# Excluding and grouping fields

There are occasions when it is appropriate to restrict the visibility of some of the fields in the model in the admin interface. This can be achieved with the `exclude` attribute.

This is the review form screen with the **Date edited** field visible. Note that the **Date created** field does not appear – it is already a hidden view because `date_created` is defined on the model with the `auto_now_add` parameter:

![](bbox=[0.175, 0.475, 0.825, 0.88])

Figure 4.57 – The review form