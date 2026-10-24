# # Going Beyond the Basics

Now that you have seen how you can integrate Chef into your Windows
environment, let’s take a look at some advanced topics, which are as follows:

+   - Managing heterogeneous networks
+   - Handling multiple platforms
+   - Versioning and source control
+   - Testing recipes for a variety of platforms using ChefSpec

# ## Chef’s declarative language

Chef recipes are declarative, which means that it provides a high-level language for
describing what to do to accomplish the task at hand without requiring that you
provide a specific implementation or procedure. This means that you can focus on
building recipes and modeling infrastructure using abstract resources so that it is
clear what is happening without having to know how it is happening. Take, as an
example, a portion of the recipes we looked at earlier for deploying an IIS application
that is responsible for installing some Windows features:

```
features = %w{IIS-ISAPIFilter IIS-ISAPIExtensions
NetFx3ServerFeatures NetFx4Extended-ASPNET45
IIS-NetFxExtensibility45}

features.each do |f|
windows_feature f do
action :install
end
end
```

Because of Chef’s declarative language, the preceding section of code reads in a
natural way. We have a list of features. For each of those features, which we know
to be Windows features, install them.