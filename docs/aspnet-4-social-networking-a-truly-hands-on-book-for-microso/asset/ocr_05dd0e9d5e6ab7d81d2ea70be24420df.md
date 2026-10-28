```
{
    Int32 profileID = 0;
    Boolean HasAttributeId = false;
    profile.LastUpdateDate = DateTime.Now;
    using (FisharooDataContext dc = conn.GetContext())
    {
        if (profile.ProfileID > 0)
        {
            dc.Profiles.Attach(new Profile { ProfileID = profile.ProfileID });
            dc.Profiles.ApplyCurrentValues(profile);
            if (profile.Attributes != null)
            {
                foreach (ProfileAttribute item in profile.Attributes)
                {
                    dc.ProfileAttributes.Attach(
                        new ProfileAttribute
                        {
                            ProfileAttributeID = item.ProfileAttributeID,
                        });
                    dc.ProfileAttributes.ApplyCurrentValues(item);
                }
            }
        }
        else
        {
            profile.CreateDate = DateTime.Now;
            //when working with new profile we don't expect attributes to be null, but check anyway
            if (profile.Attributes != null)
            {
                foreach (ProfileAttribute item in profile.Attributes)
                {
                    dc.ProfileAttributes.AddObject(new ProfileAttribute
                    {
                        ProfileAttributeID = item.ProfileAttributeID,
                        ProfileAttributeTypeID = item.ProfileAttributeTypeID,
                        ProfileID = item.ProfileID,
```