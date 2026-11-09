Additionally, you should decide whether you will only collect average or representative baseline data, or if you will include data collected during critical or peak periods. Would you like to collect information over a day with a big news event if you're a news site, or would you prefer to avoid such days? You should clearly define what types and duration of data you're going to collect, regardless of which data-collection model is appropriate for your industry.

# Step 6 – prioritizing migration components

Aside from migrating your entire application to the cloud at once, you must also decide whether to migrate the application component by component or service by service. Determine which services are dependent upon which other services, and which services are connected to which others. Consider using an application performance monitoring tool that can create dependency diagrams from service maps for larger, more complex applications.

Determining which components and in what order to migrate requires the use of the dependency diagram. Usually, the services with the fewest dependencies are the best to start with. As a result, you would migrate your innermost services first, and then the outermost, which are usually the ones your customers have access to. As an alternative, you might want to begin with the most critical outside services closest to your customers, so you can control any effect on them.

# Step 7 – performing any necessary refactoring

If you want your cloud-based applications and services to work as effectively and efficiently as possible, you might need to make some other changes to your applications and services beforehand. If so, you should refactor your application:

+   - This allows you to dynamically scale your service using a variable number of running instances, potentially saving you money.
+   - By doing so, you will be able to take greater advantage of cloud technologies, such as dynamic allocation and de-allocation of resources as needed, instead of static allocation in advance.
+   - Consider moving to a service-oriented architecture before moving individual services to the cloud so that the migration is easier.

# Step 8 – creating a data migration plan

Cloud migration is notoriously complicated, especially when it comes to migrating data. It's important to understand how data location impacts your application's performance. Adding cloud-based data access when the primary access method is still on-premises can affect performance tremendously. Additionally, data can reside on-premises or in the cloud, depending on which service is accessed.