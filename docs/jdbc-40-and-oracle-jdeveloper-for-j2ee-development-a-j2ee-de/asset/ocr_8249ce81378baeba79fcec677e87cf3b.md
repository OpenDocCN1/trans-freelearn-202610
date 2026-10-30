# Creating a JSF Data Table

Add a MyWebService1SEI.java SEI for the Bean class with **File | New**. In the **New Gallery** window select **General** in **Categories** and **Java Interface** in **Items**. MyWebService1SEI.java is listed below.

```
```java
package datatable;
import java.rmi.RemoteException;
public interface MyWebService1SEI extends java.rmi.Remote
{   public java.lang.String getTitle() throws RemoteException;
    public void setTitle(java.lang.String param0) throws
RemoteException;
    public int getCatalogId() throws RemoteException;
    public void setCatalogId(int param0) throws RemoteException;
    public java.lang.String getJournal() throws RemoteException;
    public void setJournal(java.lang.String param0) throws
RemoteException;
    public java.lang.String getPublisher() throws RemoteException;
    public void setPublisher(java.lang.String param0) throws
RemoteException;
    public java.lang.String getEdition() throws RemoteException;
    public void setEdition(java.lang.String param0) throws
RemoteException;
    public java.lang.String getAuthor() throws RemoteException;
    public void setAuthor(java.lang.String param0) throws
RemoteException;
}
```
```

Next, generate a web service from the middle-tier JavaBean class and SEI. Right-click on the DataTable.java Bean class and select **Create J2EE Web Service**.

![](bbox=[0.196, 0.535, 0.805, 0.822])