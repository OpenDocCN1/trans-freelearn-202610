> The code generated for mobile Safari browser follows a similar design approach as the code for the desktop browser; therefore, in this book we'll limit the discussion of Roo-generated code specific to desktop browser.

A module descriptor also describes *entry points* into the GWT application using <entry-point> element, as shown here for applicationScaffold module:

```
<entry-point
class="sample.roo.flightapp.client.scaffold.Scaffold"/>
```

The above code suggests that Scaffold class represents the entry point for the applicationScaffold module. Scaffold class implements com.google.gwt.core.client.EntryPoint interface of GWT—a mandatory requirement for entry point classes.

# The GWT module's entry point

As mentioned earlier, the Scaffold class of the flightapp-gwt application represents an entry point into the applicationScaffold module. The Scaffold class implements GWT's EntryPoint interface and implements its onModuleLoad method to bootstrap the flightapp-gwt application, as shown here:

```
Scaffold.java
package sample.roo.flightapp.client.scaffold;

import com.google.gwt.core.client.EntryPoint;
import com.google.gwt.core.client.GWT;

public class Scaffold implements EntryPoint {
    final private InjectorWrapper injectorWrapper =
    GWT.create(DesktopInjectorWrapper.class);

    public void onModuleLoad() {
    injectorWrapper.getInjector().getScaffoldApp().run();
    }
}
```

The first thing to notice in this code is the use of the create method of the com.google.gwt.core.client.GWT class to create the DesktopInjectorWrapper instance. As the DesktopInjectorWrapper implementation needs to be replaced by MobileInjectorWrapper for the mobile Safari browser, DesktopInjectorWrapper is created using the create method of the GWT class.