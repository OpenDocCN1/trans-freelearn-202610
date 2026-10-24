# # Examples of managing Registry values

To set the NTP configuration the same way that would have been done with the
windows_registry resource, a matching registry_key resource would look like
the following code:

```
regkey = 'HKLM\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Parameter'

registry_key regkey do
  values [
    :name => 'AvoidTimeSyncOnWan',
    :type => :reg_dword,
    :data => 0
    }, {
    :name => 'NtpServer',
    :data => "0.pool.ntp.org",
    }, {
    :name => 'Period',
    :type => :reg_string,
    :data => 'SpecialSkew'
    },]
  action :create
end
```

# # Managing the system path

Chef has resources to support manipulating the system path. This is useful
when installing new software or configuration that needs to make changes
to the system path.

The following table illustrates the available actions and parameters when using
the windows_path resource:

| windows_path    |    |
|---------------|----|
| **Action**    | **Description**    |
| add    | This adds a new entry to the system path.    |
| remove    | This removes an entry from the system path.    |
| **Parameter**    | **Description**    |
| path    | This is the resource name attribute specifying the directory to
    add to the system path.    |