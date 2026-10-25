The tenant id could not be modified, but for illustration purpose, let's modify the tenant information and see how the update is handled in the data context. The TenantId that is modified in the Immediate Window, from {743302BD-C230-4BB9-9358-0F4D68F4E820} (Prabhakar-Tenant) to {EC6875CA-21EF-4649-AA95-119EFDF7D55E} (Test-Tenant), is highlighted in the following screenshot:

![](bbox=[0.103, 0.213, 0.896, 0.42])

If we further process with the updated TenantId, we will get the following highlighted exception since the Blog entity is marked with EntityBase. The exception is thrown as highlighted here, because TenantId retrieved from the change tracker does not match TenantId from the data context:

```
private void ValidateMultiTenantPersistence()
{
    var tenantIds = ChangeTracker.Entries()
        .Where(item => item.Entity is EntityBase)
        .Select(item => ((EntityBase)item.Entity).TenantId)
        .Distinct();

    if (!tenantIds.Any()) return;
    if (tenantIds.Count() > 1 || !(tenantIds.Count().Equals(1) && tenantIds.First().Equals(TenantId)))
    {
        throw new MultiTenantException("Invalid tenant id(s) found: " + string.Join(", ", tenantIds));
    }
}
```