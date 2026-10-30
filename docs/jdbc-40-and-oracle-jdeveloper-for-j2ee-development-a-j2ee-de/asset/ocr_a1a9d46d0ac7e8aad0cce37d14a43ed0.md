journal catalog in Oracle database. The procedure to create a panel grid by binding a managed bean is as follows.

+   1. Create a Java Bean class to represent the different fields in a journal catalog.
+   2. Create a managed bean from the Java Bean class.
+   3. Bind the managed bean to a panel grid in a JSF page.

Delete the JSFPanelGrid application created in the pevious section and create a similar application and a project in Applications-Navigator for the panel grid. Generate a managed bean from a JavaBean class. A managed bean implements a resource, a JavaBean class in the example application. The JavaBean class has properties and getter/setter methods for the properties corresponding to the different columns in the example database table. Add a Java class to the project with File | New. In the New Gallery window select General in Categories and Java Class in Items. In the Create Java Class window specify the class name, PanelGridBean, and the package name, example.panelgrid. To the PanelGridBean copy the following listing.

```
```java
package example.panelgrid;
public class PanelGridBean
{
    private int catalogId;
    private String journal;
    private String publisher;
    private String edition;
    private String title;
    private String author;
    private String href;
    public PanelGridBean() {
    }
    public int getCatalogId() {
        return this.catalogId;
    }
    public void setCatalogId(int catalogId) {
        this.catalogId=catalogId;
    }
    public String getJournal() {
        return this.journal;
    }
    public void setJournal(String journal) {
        this.journal=journal;
    }
    public String getPublisher() {
        return this.publisher;
    }
    public void setPublisher(String publisher) {
        this.publisher=publisher;
    }
    public String getEdition() {
```
```